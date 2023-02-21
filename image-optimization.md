# Image optimization tools

[CarrierWave ImageOptimizer](https://github.com/jtescher/carrierwave-imageoptimizer)

The package will use these optimizers if they are present on your system:

- [JpegOptim](http://freecode.com/projects/jpegoptim)
- [Optipng](http://optipng.sourceforge.net/)
- [Pngquant 2](https://pngquant.org/)
- [SVGO](https://github.com/svg/svgo)
- [Gifsicle](http://www.lcdf.org/gifsicle/)

Here's how to install all the optimizers on Ubuntu:

```bash
sudo apt-get install jpegoptim optipng pngquant gifsicle
```

And here's how to install the binaries on MacOS (using [Homebrew](https://brew.sh/)):

```bash
brew install jpegoptim optipng pngquant gifsicle
```

# Image manupulation tools

```
apt-get install --no-install-recommends libvips42 # install libvips without nip2
```
