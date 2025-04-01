roString
========

roString is the object equivalent for intrinsic type 'String'.

This is useful in the following situations:

*   When an object is needed, instead of an intrinsic value. For example, "roList" maintains a list of objects. If an String is added to roList, it will be automatically wrapped in an roString by the language interpreter. When a function that expects a BrightScript Component as a parameter is passed a string, BrightScript automatically creates the equivalent BrightScript Component.
    
*   If any object exposes the ifString interface, that object can be used in any expression that expects an intrinsic value.
    

Supported interfaces
--------------------

*   [ifString](/docs/references/brightscript/interfaces/ifstring.md "ifString")
*   [ifStringOps](/docs/references/brightscript/interfaces/ifstringops.md "ifStringOps")
*   [ifToStr](/docs/references/brightscript/interfaces/iftostr.md "ifToStr")