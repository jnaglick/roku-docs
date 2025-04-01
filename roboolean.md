roBoolean
=========

roBoolean is the object equivalent for intrinsic type Boolean.

This is useful in the following situations:

*   When an object is needed, instead of an intrinsic value. For example, "roList" maintains a list of objects. If an Boolean is added to roList, it will be automatically wrapped in an roBoolean by the language interpreter. When a function that expects a BrightScript Component as a parameter is passed a boolean, BrightScript automatically creates the equivalent BrightScript Component.
*   If any object exposes the ifBoolean interface, that object can be used in any expression that expects an intrinsic value.

Supported interfaces
--------------------

*   [ifBoolean](/docs/references/brightscript/interfaces/ifboolean.md "ifBoolean")
*   [ifToStr](/docs/references/brightscript/interfaces/iftostr.md "ifToStr")