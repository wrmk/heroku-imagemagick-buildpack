# heroku-imagemagick-buildpack

Vendors a prebuilt ImageMagick 7 into the slug, for apps on the `heroku-22`
stack, where the stack image ships no ImageMagick of its own.

> **Temporary home.** This repository only exists so that builds have a working
> buildpack URL to clone. It is expected to move to an organization account, and
> should not be depended on from anywhere else meanwhile.

## Usage

List it in `.buildpacks` ahead of the language buildpack, so that `magick` is on
`PATH` while dependencies are installed:

```
https://github.com/wrmk/heroku-imagemagick-buildpack.git
```

## The binary

`build/imagemagick.tar.bz2` is ImageMagick 7.1.0 Q16HDRI, with `webp`, `heic`,
`jpeg`, `png` and `tiff` among its delegates, built against its own copies of
the libraries it needs.

It unpacks to `vendor/imagemagick`, which becomes `/app/vendor/imagemagick` on
the dyno, and `bin/compile` puts that path on `PATH` and `LD_LIBRARY_PATH`
through `.profile.d/imagemagick.sh`.

`MAGICK_CONFIGURE_PATH` is deliberately left unset, so ImageMagick uses its
built-in defaults rather than the XML configuration in `etc/ImageMagick-7`.

To verify a deploy, check that the `Delegates` line includes `webp`:

```bash
magick -version
```
