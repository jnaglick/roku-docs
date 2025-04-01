roAudioMetadata
===============

The roAudioMetadata component provides developers access to audio file metadata included in many audio files. This should enable some audiofiles to deliver the information needed to fill out an roSpringboard screen without passing the info in a separate xml feed. roAudioMetadata currently only works with local file URLs.

The component requires the use of a dynamically loaded library that is not part of the initially booted image. Therefore, an entry must be added to the manifest of any applications that use the roAudioMetadata component so it can be loaded when the app is launched. Here's the manifest entry:

_requires\_audiometadata=1_

This object is created without any arguments:

`CreateObject("roAudioMetadata")`

**Example**

    REM printAA() is from generalUtils.brs in our sample apps
    REM and used to print an associative Array
    
    Sub SaveCoverArtFile(filename As String)
        meta = CreateObject("roAudioMetadata")
        meta.SetUrl(filename)
        print "------------- GetTags() -------------------------"
        tags = meta.GetTags()
        printAA(tags)
        print "------------- GetAudioProperties() --------------"
        properties = meta.GetAudioProperties()
        printAA(properties)
        print "------------- GetCoverArt() ---------------------"
        thumbnail = meta.GetCoverArt()
        if (thumbnail <> invalid) then
                if (thumbnail.bytes = invalid) then
                return
            end if
            imgtype = thumbnail.type
            image_ext=""
            if (imgtype = "image/jpeg" or imgtype = "jpg") then
                image_ext = "jpg"
            else if (imgtype = "image/png" or imgtype = "png") then
                image_ext = "png"
            else
                image_ext = "jpg"
            end if
            tmp_img = "tmp:/CoverArtImage" + "." + image_ext
            if (tmp_img <> invalid) then
                DeleteFile(tmp_img)
            end if
            thumbnail.bytes.Writefile(tmp_img)
        end if
    End Sub
    

Supported interfaces
--------------------

*   [ifAudioMetadata](/docs/references/brightscript/interfaces/ifaudiometadata.md "ifAudioMetadata")