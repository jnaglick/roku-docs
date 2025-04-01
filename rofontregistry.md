roFontRegistry
==============

The roFontRegistry object allows you to create roFont objects, either using the default font or using fonts in TrueType or OpenType files packaged with your application.

This object is created with no parameters:

`CreateObject("roFontRegistry")`

**Example**

    reg = CreateObject("roFontRegistry")
    font = reg.GetDefaultFont(30, false, false)
    screen = CreateObject("roScreen")
    screen.DrawText("hello world", 100, 100, &hFFFFFFFF, font)
    

**Example using a font file**

    reg.Register("pkg:/fonts/myfont.ttf")
    font = reg.GetFont("MyFont", 30, false, false)
    screen = CreateObject("roScreen")
    screen.DrawText("hello world", 100, 100, &hFFFFFFFF, font)
    

Font files can quickly get very large, so be conscious of the size of the font files you include with your application. You should be able to find very good font files that are 50k or less. Anything larger is probably too big.

Supported interfaces
--------------------

*   [ifFontRegistry](/docs/references/brightscript/interfaces/iffontregistry.md "ifFontRegistry")