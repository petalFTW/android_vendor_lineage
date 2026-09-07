# Petal V3 3D boot animation

`petal.zip` is selected by `TARGET_BOOTANIMATION` in the Petal macanc product.
The existing `bootanimation.zip` module installs it to `/product/media`; the dark
variant points to the same file. The prebuilt is a declared genrule input so
incremental builds track changes to its content.

The archive contains stored RGB PNGs, trim offsets, and this descriptor:

```
720 720 30
p 1 0 part0 #000000
f 0 0 part1 24 #000000
```

The 90-frame intro leads into a 180-frame repeating 3D turn. On boot completion,
the native player continues that turn while fading for 24 frames (0.8 seconds),
then exits directly. There must be no trailing completed (`c`) part: this ROM's
BootAnimation.cpp retains a positive fadedFramesCount after the fade and uses
it in the outer repetition condition. A subsequent completed part cannot pass
the stop predicate and repeats indefinitely, hiding Android behind its surface.

Source artwork, renderer, offline viewer, and regression checks are maintained
in `/home/rab1d/BootAnim` on the development workstation. Runtime requires only
the ZIP; no rendering libraries or new executable are installed in the ROM.
