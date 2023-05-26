# Deprecation Notice

This repository is no longer maintained.

# goav
Golang binding for FFmpeg

A comprehensive binding to the ffmpeg video/audio manipulation library.

[![GoDoc](https://godoc.org/github.com/chenhengjie123/goav?status.svg)](https://godoc.org/github.com/chenhengjie123/goav)

## Usage

`````go

import "github.com/chenhengjie123/goav/avformat"

func main() {

	filename := "sample.mp4"

	// Register all formats and codecs
	avformat.AvRegisterAll()

	ctx := avformat.AvformatAllocContext()

	// Open video file
	if avformat.AvformatOpenInput(&ctx, filename, nil, nil) != 0 {
		log.Println("Error: Couldn't open file.")
		return
	}

	// Retrieve stream information
	if ctx.AvformatFindStreamInfo(nil) < 0 {
		log.Println("Error: Couldn't find stream information.")

		// Close input file and free context
		ctx.AvformatCloseInput()
		return
	}

	//...

}
`````

## Libraries

* `avcodec` corresponds to the ffmpeg library: libavcodec [provides implementation of a wider range of codecs]
* `avformat` corresponds to the ffmpeg library: libavformat [implements streaming protocols, container formats and basic I/O access]
* `avutil` corresponds to the ffmpeg library: libavutil [includes hashers, decompressors and miscellaneous utility functions]
* `avfilter` corresponds to the ffmpeg library: libavfilter [provides a mean to alter decoded Audio and Video through chain of filters]
* `avdevice` corresponds to the ffmpeg library: libavdevice [provides an abstraction to access capture and playback devices]
* `swresample` corresponds to the ffmpeg library: libswresample [implements audio mixing and resampling routines]
* `swscale` corresponds to the ffmpeg library: libswscale [implements color conversion and scaling routines]


## Installation

[FFMPEG INSTALL INSTRUCTIONS](https://github.com/FFmpeg/FFmpeg/blob/master/INSTALL.md)

``` sh
wget https://code.videolan.org/videolan/x264/-/archive/master/x264-master.zip
unzip x264-master.zip
cd x264-master
./configure --prefix=/usr/local/libx264 --enable-shared --enable-static
make 
sudo make install



wget  https://github.com/FFmpeg/FFmpeg/archive/refs/tags/n3.4.6.zip
unzip n3.4.6.zip
cd FFmpeg-n3.4.6
./configure --prefix=/usr/local/ffmpeg --enable-shared --enable-libx264 --enable-gpl --extra-cflags=-I/usr/local/libx264/include --extra-ldflags=-L/usr/local/libx264/
make -j8
sudo make install
``` 

Check if libx264 is successfully added(it should occur in command output like below)

```
$ /usr/local/ffmpeg/bin/ffmpeg -encoders | grep 264
ffmpeg version 3.4.6 Copyright (c) 2000-2019 the FFmpeg developers
  built with Apple clang version 14.0.0 (clang-1400.0.29.202)
  configuration: --prefix=/usr/local/ffmpeg --enable-shared --enable-libx264 --enable-gpl --extra-cflags=-I/usr/local/libx264/include --extra-ldflags=-L/usr/local/libx264/
  libavutil      55. 78.100 / 55. 78.100
  libavcodec     57.107.100 / 57.107.100
  libavformat    57. 83.100 / 57. 83.100
  libavdevice    57. 10.100 / 57. 10.100
  libavfilter     6.107.100 /  6.107.100
  libswscale      4.  8.100 /  4.  8.100
  libswresample   2.  9.100 /  2.  9.100
  libpostproc    54.  7.100 / 54.  7.100
 V..... libx264              libx264 H.264 / AVC / MPEG-4 AVC / MPEG-4 part 10 (codec h264)
 V..... libx264rgb           libx264 H.264 / AVC / MPEG-4 AVC / MPEG-4 part 10 RGB (codec h264)
 V..... h264_videotoolbox    VideoToolbox H.264 Encoder (codec h264)
```


Add following commands in `~/.profile`

``` sh
export FFMPEG_ROOT=/usr/local/ffmpeg
export CGO_LDFLAGS="-L$FFMPEG_ROOT/lib/ -lavcodec -lavformat -lavutil -lswscale -lswresample -lavdevice -lavfilter"
export CGO_CFLAGS="-I$FFMPEG_ROOT/include"
export LD_LIBRARY_PATH=/usr/local/ffmpeg/lib
```

Add module to project

``` 
go get github.com/chenhengjie123/goav

``` 

## More Examples

Coding examples are available in the examples/ directory.

## Note
- Function names in Go are consistent with that of the libraries to help with easy search
- [cgo: Extending Go with C](http://blog.giorgis.io/cgo-examples)
- goav comes with absolutely no warranty.

## Contribute
- Fork this repo and create your own feature branch.
- Follow standard Go conventions
- Test your code.
- Create pull request

## TODO

- [ ] Returning Errors
- [ ] Garbage Collection
- [X] Review included/excluded functions from each library
- [ ] Go Tests
- [ ] Possible restructuring packages
- [x] Tutorial01.c
- [ ] More Tutorial

## License
This library is under the [MIT License](http://opensource.org/licenses/MIT)
