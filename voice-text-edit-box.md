VoiceTextEditBox
================

Extends [TextEditBox](/docs/references/scenegraph/widget-nodes/texteditbox.md)

The **VoiceTextEditBox** node is similar to the [legacy **TextEditBox** node](/docs/references/scenegraph/widget-nodes/texteditbox.md), but with additional voice entry functionality. Only one voice-enabled **VoiceTextEditBox** node may be on the screen at a time. If another VoiceTextEditBox is rendered on the screen, its voice functionality is disabled implicitly.

Fields
------

| Field | Type | Default | Access Permission | Description |
| --- | --- | --- | --- | --- |
| voiceEnabled | boolean | false | READ\_WRITE | Enables the text box to be voice-enabled. In this case, it will display a mic icon and have a voice UI with voice hints. |
| voiceToolTipWidth | float | FHD: 321HD: 214 | READ\_WRITE | The maximum width of the voice hint tootip. The height scales based on the specified width. |
| voiceEntryType | string | "generic" | READ\_WRITE | The type of voice entry mode to be used:  <br><br>*   "email": letter-by-letter dictation for emails.<br>*   "numeric": letter-by-letter dictation for PIN codes, zip codes, and other numeric input.<br>*   "alphanumeric": letter-by-letter dication for street addresses or other sequences of numbers and letters.<br>*   "generic": Full word input for search queries or other sequences of numbers, letters and symbols.<br>*   "password": letter-by-letter dication for passwords. |
| isDictating | boolean | false | READ-ONLY | Checks whether the user is currently dictating to the keyboard. |
| voiceInputRegexFilter | string | ""  | WRITE-ONLY | Specify which characters may or may not be entered on the keyboard via dictation. For example, setting this field to "^\[A-Za-z0-9\_-\]\*$" prevents any special characters from being entered. |