SceneGraph threads
==================

SceneGraph introduced multi-threaded operations to Roku application programming. The following are the basic threads available to a SceneGraph application programmer:

*   **Main BrightScript thread**: This is the thread that is launched for all Roku applications from the `main.brs` file. For SceneGraph applications, the thread is used primarily to create the scene component object, which starts the SceneGraph render thread. For other applications, this is the only thread for the entire application.

*   **SceneGraph render thread**: The render thread is the main SceneGraph thread that performs all rendering of the application visual elements. Certain BrightScript operations and components that might block or modify the SceneGraph in the render thread cannot be used in this thread. Operations and components that might block the render thread can be used in a Task node thread. The thread usage of these operations and components is listed in [BrightScript support](/docs/developer-program/core-concepts/scenegraph-brightscript/brightscript-support.md).

> If the render thread blocks execution, production apps will terminate after 10 seconds; sideloaded apps will timeout in 3 seconds.

*   **Task node threads**: By creating and running a [Task](/docs/references/scenegraph/control-nodes/task.md) node, you can launch asynchronous BrightScript threads. These threads can perform most typical BrightScript operations.

Thread limits
-------------

As of Roku OS 10.0, the maximum number of concurrent threads per running instance of an app is limited to 100. However, developers should limit their apps to a maximum of 50 concurrent threads.

*   When an instance exceeds 50 concurrent threads, Roku displays a warning on the port 8085 console.

*   When the instance exceeds 100 threads, a “too many threads” error exception (&h29) is raised; if the app does not catch this exception, app operation is terminated, along with a corresponding stack trace.

Task threads that have properly terminated and are no longer running will not count towards the limit, even if the task object itself is still valid (for example, the state is stopped or done).

> Developers should take steps to ensure that their apps always remain well under the 50-thread "warning" limit.

Thread ownership
----------------

All SceneGraph node objects have a thread owner, which by default, is the render thread. Only components that extend the Node node class and the ContentNode class can be owned by a Task node or main BrightScript thread when created by those threads. When a node object is stored in a field, the ownership of the node is changed to the node object that contains the field. When a node object is added as a child of another node object, the ownership is changed to the parent node object.

Operations on node objects are executed on their owning thread. If invoked by another thread, the invoking thread must rendezvous with the owning thread to execute the operation.

Thread rendezvous
-----------------

In a thread rendezvous, the invoking thread makes a request of the owning thread, and then waits for a response. The owning thread receives the request, processes it, and then responds. The invoking thread then continues with the response as if it had made the call itself. The response appears synchronous to the invoking thread.

Only the render thread may serve a rendezvous. Since Task node threads do not have an implicit event loop (though they may have an explicit event loop), they cannot serve a rendezvous. No node object owned by a Task node thread is accessible outside that thread. The Task node itself is owned by the render thread, so the Task node and its fields can only be accessed by rendezvous from other threads, even from the thread launched by the Task node itself.

The entire interface to a node object, including field creation, setting, and getting, uses this rendezvous mechanism to ensure thread safety, without having to use explicit locks in the application, and without the possibility of deadlock. The rendezvous mechanism does add more overhead than simple field getting and setting, so SceneGraph application programmers should use it carefully, taking into account the following concerning Task node threads.

> Use the [**logrendezvous** command](/docs/developer-program/debugging/debugging-channels.md#scenegraph-debug-server-port-8080-commands) in the SceneGraph debug console to identify performance issues in the Task thread caused by a rendezvous. This command indicates whether a rendezvous is occurring and the length of it is taking (in milliseconds).

### BrightScript operations without SceneGraph node objects

If a BrightScript operation does not involve a SceneGraph node object, such as reading a URL, or parsing XML, the rendezvous mechanism is not used. These types of operations can be used in a Task node thread without the overhead of the rendezvous mechanism.

### Task node thread fully-owned node objects

Task node threads can create node and ContentNode objects that are fully owned by the Task node thread. These node objects cannot be parented to unowned nodes, or be set as fields of unowned nodes. Entire trees of the nodes fully owned by the Task node thread may be created.

### Renderable node (Group) objects

You should generally not create renderable node objects in a Task node thread. The rendezvous mechanism will be required to create and operate on those node objects. Every field set or get operation on such nodes will require a full rendezvous, and this could impact the performance of your application.

### Excessive rendezvous operations

You should avoid as many rendezvous operations as possible to ensure maximum performance of your application. It is better to build an entire tree of nodes or ContentNodes, then pass the tree to the render thread using one rendezvous, than to repeatedly pass each node in the tree as it is created. For field setting and getting, [ifSGNodeField](/docs/references/brightscript/interfaces/ifsgnodefield.md) methods such as `getFields()` and `setFields()`, which set and get multiple fields at once, should be used rather than several get and set operations.

### Task node thread rendezvous timeout

> Thread rendezvous do not timeout and will wait indefinitely. The rendezvous information below is only valid for Roku OS version 7.2 and older.

A Task node thread rendezvous can time out after a few seconds. This can happen if the render thread is very busy, maybe with other rendezvous operations. If a timeout occurs while getting a field, the response will be invalid, which may crash the application if the script is not prepared for the invalid response. During application development, you should check for these timeouts to increase the performance of your published application.

Also note that the render thread can time out while it is executing long linear searches, long iterative calculations, synchronous file accesses, and the like. These types of operations not only slow down the rendering of the UI, but can lead to a rendezvous timeout generating an invalid response, possibly crashing the application. Push computing not directly related to rendering or reacting to the UI into Task node threads.

### Task node objects ownership

Since Task node objects are owned by the render thread, setting Task node object fields is done on the render thread, and all observer callbacks on the fields are executed in the render thread. The only case where observer callbacks are executed in a Task node object is if the observed field is in a node object owned by the Task node thread.

### Re-running a task

If a Task is already in a given state as indicated by its state field, including RUN, setting its control field to that same state value has no effect. To rerun a Task, it must be in the STOP state, either by returning from its function or being commanded to STOP via its control field. At the time a Task transitions to RUN, it will look at its functionName field to determine what function to execute. It will transition to the STOP state automatically when that function returns. To get multiple independent threads running from the same Task component class, create multiple Task instances. There can be several simultaneously executing objects of the same Task component class, and each can be running different functions from the component.

### Component global associative array

All components have a global associative array designated as `m`, including Task node objects. This associative array is local to the component but global within it. For Task node objects, this associative array is not shared between threads. The Task node m is initially owned by the render thread. The Task node `init()` function then populates the Task node object with references in the associative array. On every setting of the Task node control field to `RUN`, a new thread is launched, and the Task node object associative array is cloned, with the launched thread receiving the original object associative array, and the render thread receiving the clone. Only basic object types are cloned: integers, Booleans, strings, floats, nodes, and recursively, arrays and associative arrays. These are generally the same object types that are copied through the fields, plus functions and timespans. Because of this cloning mechanism, some object references, such as a message port created in `init()`, are only passed to the first thread launched from a Task node, and not to subsequent threads launched by the same Task node.

### Task node changes in Roku OS 7.2

In both Roku OS 7.1 and 7.2, within the same Task node component object, the `m` for the render thread is distinct from that of any spawned node Task node thread. The initialization of a Task node object happens in the render thread, so the Task node `init()` function operates on the `m` for the render thread.

In Roku OS 7.1, on transition of a Task node control state to `"RUN"`, the `m` for the Task node thread was almost empty save for the references to the top and global nodes.

In Roku OS 7.2, on each transition of the Task node control state to `"RUN"`, the render thread `m` is cloned, the original is passed to the new Task node thread, and the render thread retains the clone. Members of `m` which are basic types like integers, Booleans, strings, and floats are cloned. Associative arrays and arrays are cloned recursively. RoSGNodes are also cloned, but not recursively, since they are already thread-safe. This is all much like what would happen when an associative array is set to a field.

Since the non-port observer callbacks can modify the render thread `m`, any state changes they make are seen by new threads spawned after such changes.

This means that previously, in Roku OS version 7.1, only the render thread `m` had state from `init()`. In Roku OS version 7.2, both the render thread `m` and the Task node thread `m` have state from `init()`. Since the Task node thread gets the original, non-cloneable objects, like message ports, they end up in the Task node thread `m` . The render thread `m` would no longer have them, and would have to recreate them in its `m` if it wanted to pass such things to the Task node thread for subsequent control state transitions to `"RUN"`.

The general idea is that you should expect `init()` to operate on the `m` state visible to both the render thread and the Task node thread.

In the port form of the ifSGNodeField [`observeField()`](/docs/references/brightscript/interfaces/ifsgnodefield.md#observefieldfieldname-as-string-functionname-as-string-as-boolean) method, it can be helpful to create the port, and observe using it in `init()`, so that no subsequent field settings are missed due to race. Transferring the original to the Task node thread rather than the clone allows this to work since the port is not cloneable.