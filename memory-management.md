Memory management
=================

Texture memory
--------------

A common offense is simply using too much texture memory. It's important to realize that simply using a larger image does not necessarily ensure a higher quality end result. Rather, using unnecessarily large images will more often than not result in performance issues. All Roku devices have a dedicated amount of texture memory, which varies largely from device to device. Each _unique_ image you would like to display on your app will take up a certain amount of the limited texture memory (however, reusing the same image on multiple nodes will not take up more memory). More specifically, the **size in bytes** of the image file does not matter (so compressing your images is inconsequential), rather, the **pixel dimension** of the image is strictly important. Images are loaded into texture memory in the form of bitmaps, taking up 4 bytes per pixel (RGBA). A quick calculation (width x height x 4B) can help you approximate how much texture memory your images will be taking up.

Here is a small list of the amount of texture memory available to your app to use on various devices:

*   [Tyler](/docs/specs/hardware.md#roku-models-and-features): 20 MB
*   [Giga](/docs/specs/hardware.md#roku-models-and-features): 44-45 MB
*   [Mustang/Austin](/docs/specs/hardware.md#roku-models-and-features): 60 MB

If your app ends up going over any of these limits by trying to display more than what the texture memory can allow for, then your app will run into unexpected behavior on those devices. A common symptom is flickering images or slow content loading, as bitmaps will be constantly unloaded and reloaded onto memory in an effort to manage the excess images. Even if your images do not use up all the texture memory on a device, your app will load slower in general if it contains larger images.

### How to see texture memory

1.  Side load your app using one of the normal methods
2.  Run your app and navigate in the UI to where you think there might be an issue.
3.  telnet to your roku at port 8080.
4.  If your app is SceneGraph run this command: "loaded\_textures". Be aware that there is a texture cache, so images that are not visible can be cleared automatically.
5.  if your app is pre-SceneGraph, 2D API or template, run this command: "r2d2\_bitmaps."

### How to avoid going over memory limits

#### Make images smaller

The simplest solution! If you're planning on displaying an image on a 200x200 Poster node, don't load in and render a 1920x1080 image. It will work, but it'll be a waste of system resources for no real benefit. A quick calculation puts a 1920x1080 image at using a whopping (1920•1080•4 = **~8.3MB**) of memory, while the appropriately sized 200x200 image will only take up **~0.16MB**. Using the loadWidth and loadHeight fields of a Poster node would be an equivalent solution to resizing the images themselves.

#### Use minimalistic item renderers

The fewer elements, the better. Use [Rectangle](/docs/references/scenegraph/renderable-nodes/rectangle.md) nodes, which do not require a bitmap loaded into memory, over [Poster](/docs/references/scenegraph/renderable-nodes/poster.md) nodes whenever possible. Keep in mind that even bitmaps for elements like focus rings and keyboard backgrounds take up texture memory - so take care to not use unnecessarily large images for these.

### Debugging texture memory performance

Using r2d2_bitmaps to check the amount of texture memory available:_

![roku815px - texturememory](https://image.roku.com/ZHZscHItMTc2/texturememory.png "texturememory")

You can check your texture memory usage by telnetting to port 8080 on your Roku device and running the command “r2d2\_bitmaps”. This command will output a list of memory addresses representing the assets loaded into texture memory, their width and height, as well as their size in bytes. At the bottom, it also shows the available memory you have on your device left, the amount used, and the amount that the device has in total. If your app uses multiple high resolution images (e.g. more than two 1920 x 1080 images), you will notice that the available memory will hit a peak somewhere less than the max amount and keep fluctuating between values as the texture memory manager tries offloading assets and reloading them back in to manage memory. Make sure to use the performance techniques listed in this guide so that your app doesn't run into these problems!

System memory (DRAM)
--------------------

*   Roku devices have anywhere from **256 MB DRAM** on the lowest end devices, to **1.5 GB DRAM** on the [Dallas](/docs/specs/hardware.md#roku-models-and-features) platform.
    
*   While many applications like **image processing or 3D modeling software** benefit greatly from a large amount of RAM, this is usually not the case for apps running on Roku OS.
    
*   For nearly all apps, **RAM will not be a bottleneck for performance** unless you have a serious memory leak somewhere.
    
*   An app is far more likely to hit the texture memory or CPU ceiling than to ever run out of RAM, and your app is sandboxed such that the Roku device will always allocate and save enough RAM for video buffering. In addition, if your app uses a large amount of RAM it will simply be killed before performance hits are noticeable.
    

### Viewing system memory

**For SceneGraph apps**

For SceneGraph apps, just like above, run the app and `port 8080`. Then run `sgnodes all`

**For pre-SceneGraph apps**

For pre-SceneGraph apps, telnet to the console; hit **^C** to break into the debugger; run `bcs` or `bscs`

> This is also useful for SceneGraph apps.