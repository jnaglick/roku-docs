roFont
======

roFont represents a particular font, from a font-family (eg. Arial), with a particular pixel size (e.g 20), and a particular boldness or italicness.

It is used in conjunction with [roFontRegistry](/docs/references/brightscript/components/rofontregistry.md "roFontRegistry") to create and manage fonts. Font files are registered with roFontRegistry and then various methods in roFontRegistry can be used to create roFont objects. Applications should not create roFonts with CreateObject() but should always use roFontRegistry to create them. roFont objects in turn can be used with [ifDraw2D.DrawText](/docs/references/brightscript/interfaces/ifdraw2d.md#drawtextrgba-as-integer-x-as-integer-y-as-integer-text-as-string-font-as-object-as-boolean "ifDraw2D.DrawText") to draw text on the screen or into bitmaps.

**Example**

    screen = CreateObject("roScreen")
    white = &hFFFFFFFF
    blue = &h0000FFFF
    font_registry = CreateObject("roFontRegistry")
    font = font_registry.GetDefaultFont()
    
    ' Draw white text in a blue rectangle
    text = "Hello world"
    w = font.GetOneLineWidth(text, screen.GetWidth())
    h = font.GetOneLineHeight()
    x = 200
    y = 100
    border = 8
    screen.DrawRect(x, y, w + 2*border, h + 2*border, blue)
    screen.DrawText(text, x+border, y+border, white, font)
    

Supported interfaces
--------------------

*   [ifFont](/docs/references/brightscript/interfaces/iffont.md "ifFont")