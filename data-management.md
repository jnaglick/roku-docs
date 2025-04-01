Data management
===============

Thread ownership of Nodes
-------------------------

*   Each node is owned by the thread which created it, which might or might not be the Render thread. However, if a node interacts with the Render thread, the Render thread will automatically take ownership of that node.
    
*   In particular, renderable node instances ([Group](/docs/references/scenegraph/layout-group-nodes/group.md) and every component extended from it) are owned by the render thread, no matter which thread calls `createObject()`. The Global node is owned by the Render thread. When a node is set as a child or field of another node owned by the Render thread, the referenced node becomes owned by the Render thread. Any variable or node in the Task thread which interacts with the Render thread will automatically be owned by the Render thread. In such a case, the nodes are permanently owned by the Render thread.
    
*   The only instance of Non-Render thread ownership is that of a plain [Node](/docs/references/scenegraph/node.md) or [ContentNode](/docs/references/scenegraph/control-nodes/contentnode.md) or a component extending one of those that are created by a Task thread. A node instance in those circumstances will be owned by the Task thread as long as it is not added as a field or a child of a node owned by the Render thread.
    
*   This transfer of ownership is recursively performed on all nodes and field referenced by a transferred node, and it is even performed on nodes referenced in AA or array fields.
    
*   The key to optimizing performance is to allow the owning thread to access the node directly without a rendezvous.
    
*   Node creation, especially of inherited components, can be expensive. It is suggested that "AddFields" be used instead of "Extends" for such components.
    

Use of `m.global`
-----------------

*   Since the global node (`m.global` in a Component) is owned by the SceneGraph render thread, any operations on `m.global` from a Task thread are accomplished via rendezvous.
    
*   If a Task thread creates a node and adds it to `m.global` via a field or as a child (or grandchild, etc.), it is best to have the Task thread perform operations on the node before it is added so that it may do so without rendezvous.
    

### Referencing subsections of `m.global`

Given the rendezvous penalties, don't repeatedly reference the same fields in `m.global` to get data subsections. Use temporaries to hold references to successive parts of the tree. For example, assume that you have a large set of app configuration data stored in `m.global.config`. This data is a large web with elements (AAs or node trees) for settings, analytics, etc.:

    m.global
    {
            config
            {
                settings
                {
                    }
                analytics
                {
                    }
            }
    }
    

Getting some or all of this data into a Task node can be done as follows:

    <component name="DataTask" extends="Task">
    <interface>
        <field id="settings" ... />
        <field id="analytics" ... />
    </interface>
    ...
    
    function init() as void
        m.config = m.global.config
        m.settings = m.config.settings
        m.analytics= m.config.analytics
    end function
    
    ...
    
    </component>
    

The Task makes a local copy of the config global data which it then references via that local variable, avoiding a rendezvous and a copy for subsequent references. Generally speaking a Task should locally copy only what it needs from `m.global`, so there is a design trade-off in grouping data in subtrees versus spreading it all out at the top level.

Task to render thread rendezvous
--------------------------------

*   When a Task thread operates on nodes it owns, it does so directly, and it does not trigger a rendezvous.
    
*   When a Task thread operates on a Render-thread-owned node, it triggers a rendezvous. Depending on the structure of the Task, other Tasks might be able to access its nodes, triggering a rendezvous. Nodes are passed by reference whereas Associative Arrays are passed by value.
    
*   Under the covers, the Task thread queues a requested operation on the Render thread's queue and blocks, waiting for completion of the operation. The Render thread eventually pulls the requested operation off of its queue, executes it, and returns the results into references from where the Task thread may access them. The Task thread sees the results of the operation as soon as it unblocks from the rendezvous. Thus, it appears as a synchronous call to the Task thread. From the Task thread's point of view, both the syntax and the semantics of the call are the same as if the Task thread owned the node itself.
    
*   While rendezvous are designed to be invisible to syntax and semantics, **a rendezvous is at least an order of magnitude more expensive than a direct access.** For this reason, rendezvous should be used somewhat sparingly.
    
*   A distinct rendezvous is used on each separate operation invoked by a Task thread on a Render-thread-owned node. In particular, consider an expression like x.y.z, where x is a node and y and z, are fields on that node. This is evaluated as a succession of getField() calls. If x is owned by the Render thread and this expression is evaluated in a Task thread, each dot represents a distinct rendezvous. Simply counting dots in naively organized Task thread scripts can indicate the number of rendezvous and the performance penalties incurred. In well designed Task thread scripts, most dots avoid as many rendezvous as it can.
    

> Use the [**logrendezvous** command](/docs/developer-program/debugging/debugging-channels.md#scenegraph-debug-server-port-8080-commands) in the SceneGraph debug console to identify performance issues in the Task thread caused by a rendezvous. This command indicates whether a rendezvous is occurring and the length of it is taking (in milliseconds).

### Utilizing the UriFetcher data model

Use the [UriFetcher sample app](https://github.com/rokudev/samples/blob/master/utilities/uri-fetcher-master) to handle URL requests. UriFetcher provides a simple, non-blocking implementation of a basic download manager which is capable of multiple asynchronous URL requests implemented through a long-lived data task and is the preferred data model to fetch multiple URLs, even simultaneously. Since this data model acts as a long-lived task, it allows for more consistent memory usage by implementing a queuing mechanism for the tasks.

Data modeling
-------------

*   Generally, data passed to and from fields is passed by copy. This is necessary to support a multi-threaded model that avoids explicit locking.
    
*   In particular, containers like associative arrays and plain arrays get passed to and from fields by deep copy. This is a complete recursive copy of all of the data in the container, including copies of all the containers in the container.
    
*   Unlike traditional BrightScript, passing large webs of AAs or arrays through SceneGraph node fields is not efficient and should be avoided.
    
*   On the other hand, using AAs to model small data structures is a reasonable way to consolidate related fields that are usually set or read as a whole. If the AA is being passed between the Render thread and a Task thread, this will incur a single rendezvous. This way, it is not required to trigger multiple rendezvous for each field access if separate data elements are modeled as separate fields.
    
*   Each instance of a SceneGraph node is an exception to the copy rule through fields. **SceneGraph nodes get passed to and from fields by reference.** Using a node tree to model complex content means that a single field or child can accept a large change in the data model with one reference change.
    
*   The result of the above considerations is that the data that needs to be accessible via nodes and fields fall into two categories:
    
    *   The first category represents small, shallow data structures where each structure instance is usually treated as a single cohesive item. These are reasonable and efficient to model as **AA fields**.
    *   The second represents large, deep data webs where copying would be prohibitive. It is reasonable to model these as **node trees**.
*   Prune unneeded data in ContentNode AAs. Keep the data limited to what might be utilized when the node is called. As an example, when in a TimeGrid view, users won't see the show descriptions. Hence, when populating the Grid, parse what data is displayed before it gets to the ContentNode. This will prevent slow loading because of significant amounts of Meta-Data.
    

Marshalling data
----------------

*   Loading large amounts of server data should be done in the background using a Task node. The Task thread should try to keep ownership of the node while updating its fields, and should only pass the node into a field or as a child of a node owned by the Render thread after performing all the operations on the node that it must.
    
*   Rendering the content on the screen in the app on the Render thread is done by setting the ContentNode of the appropriate SceneGraph UI component. By the above guidance, this ContentNode should only be set into the UI node's content field once all of the data models are loaded by the Task thread into the ContentNode. Since each dot operation on a node requires a rendezvous, once the node is owned by the Render thread, a Task thread should use one dot reference at the end to set the entire ContentNode and its subtree rather than transferring the node and then operating on it with many subsequent dot operations.
    
*   As a best practice, small amounts of data that form input or status for Task threads can be grouped in AA fields, and large data webs should be returned from the Task thread to the Render thread as node trees.
    
*   To get data structures (more than one scalar value for example) into a Task node at a time, it is best to create a single interface field on the Task node with type assocarray. The Render thread can then set that interface field with the data it wants to pass. Only one rendezvous is required for the whole copy, versus multiple if there are individual interface fields for each value.
    

Threading and observer callbacks
--------------------------------

*   The observer callback function for setting a particular field is executed in the thread that owns the node in which the field was set initially.
    
*   Any callbacks set up on the Task node's fields will be executed by the Render thread if the node is owned by the Render Thread. For a Task thread to respond to the setting of such a node's fields, use the port form of the [`observeField()`](/docs/references/brightscript/interfaces/ifsgnodefield.md) call and wait for an [roSGNodeEvent](/docs/references/brightscript/events/rosgnodeevent.md) on that port. For nodes owned by the Task thread itself, the callback functions are set to call back directly to the Task thread.
    

Task initialization
-------------------

1.  The initialization of a Task does a bit more work and has more nuances than for other node types. The differences are designed to be mostly invisible, much like a rendezvous operation versus a normal operation.
    
2.  During the creation of the Task node, `init()` is executed by the Render thread (because the Task thread is in the process of being created).
    
3.  The Component `m` that is created before and modified during `init()` belongs to the Render thread at this point. Function callbacks of Task node fields setup during `init()` executes in the Render thread and then access this `m`.
    
4.  When the Task's control field is set to RUN, the Render thread's m is cloned (deep copied), the Render thread gets the clone, and the Task thread gets the original m. This ensures that a port created in init() is available to the Task thread, as it is the thread that needs to access the port
    
5.  In `init()`, the newly created port can be immediately used to observe fields; these fields guarantee to generate events that will be received by the Task thread once it is started. Waiting to create a port and observe fields in the Task thread itself can create a race condition where settings of the field from the Render thread may occur before the Task thread has had a chance to set up observation.
    
6.  If the same instance of a Task node stops and is started again, the Render thread's m is cloned; the Render thread gets this clone, and the Task thread gets the "original" (now a 1st generation clone created in 4). If there is setup that must be done for m (like creating a new port), it must be done in a field callback of the Task node (executed by the render thread during task initialization), that can modify the appropriate m and set the Task node's control to `RUN`.
    

Garbage Collector
-----------------

*   **SceneGraph Nodes** Nodes are reference counted. When the reference count goes to zero, the Roku OS automatically handles the clean up of SceneGraph nodes.
    
*   **Brightscript Objects** Brightscript objects can be cleaned up using the built-in Garbage Collector if no other elements are referencing the Brightscript object. Generally, this is done once right before video playback.
    

> **Note**: There is no advantage to calling the Garbage Collector frequently. For more information refer to [RunGarbageCollector](/docs/references/brightscript/language/global-utility-functions.md#rungarbagecollector-as-object).

Circular Dependencies in SceneGraph
-----------------------------------

A **circular reference** is a series of references where the last object references the first, resulting in a closed loop. Since all the nodes are reference counted, as soon as the reference count for the node is zero, the Roku OS automatically clears out that node. Although, the Roku OS won't be able to clean out circular dependent nodes, as they will always have a reference to one another. This, in turn, consumes memory indefinitely.

Using getScene()
----------------

getScene() is a useful function to avoid having to store parent pointers. It gets the Node's root Scene, and from there you can search down for the Nodes you want to access without storing any pointers. More for information about getScene(), refer to [getScene() as roSGNode](/docs/references/brightscript/interfaces/ifsgnodechildren.md#getscene-as-rosgnode).

Image Sizing
------------

*   Using properly sized images for various resolutions ensures that the quality of the UI is maintained. Smaller images can be added to the script using different URLs for different images. Moreover, [uri\_resolution\_autosub](/docs/developer-program/getting-started/architecture/channel-manifest.md#graphics-scaling-attributes) can be used to substitute the resolution type for a particular image URL, and the function regresses the URL automatically.
    
*   If the developer cannot provide the images using the server, they can use the Poster node fields: LoadWidth, LoadHeight, and LoadDisplayMode, at minimum values to provide low-resolution images. For more information about these fields, refer to [Posters](/docs/references/scenegraph/renderable-nodes/poster.md#autoscaling).