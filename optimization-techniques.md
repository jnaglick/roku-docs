Optimization techniques
=======================

SGNode initialization
---------------------

**Offload Initialization**

Keep init() in the main scene and its static children as minimal as possible. Having too much "upfront" initialization will cause the app to take a long time loading the main scene. Leave as much initialization as possible for the task thread function.

Data flow
---------

**Copying Nodes**

When copying nodes, do not simply call:

    node2.setFields(node1.getFields())
    

Setting nonexistent fields in a node will invoke additional internal verification and warning outputs to the debug console, causing UI lag. Instead, do something like:

    function cloneNode(oldNode as Object) as Object
      newNode = createObject("roSGNode",oldNode.subtype()) 'subtyped node should automatically have all the fields of the original node
      newNode.setFields(oldNode.getFields())
      return newNode
    end function
    

**ContentNode vs. associative arrays**

Use [ContentNode](/docs/references/scenegraph/control-nodes/contentnode.md) fields to represent complex trees of nested data that are expensive to copy, since they will be passed by reference. Use associative arrays to store small, shallow data structs. Associative arrays will be deep-copied through fields (pass-by-value) and has the advantage of keeping parallelization safer and more efficient.

**Avoiding onChange in Task threads**

If you'd like a Task node to execute a function in response to a field change, use the overloaded version of [**observeField()**](/docs/references/brightscript/interfaces/ifsgnodedict.md) to send an roSGNodeEvent to your message port. Do this instead of setting **onChange** in the field you want to watch. While this is certainly not necessary, it might improve visual performance since onChange is usually executed on the render thread.