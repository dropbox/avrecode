avrecode: lossless re-compression of compressed video streams
=============================================================

avrecode reads an already-compressed video file and writes a more compressed
file. Unlike transcoding, which loses fidelity, the compression algorithm used
by avrecode is reversible. The decompressed bytes exactly match the original
input file. However, avrecode's compressed format can only be read by avrecode
-- an avrecode-compressed file cannot be played directly by standard software.

avrecode works by decoding the video stream into symbols using ffmpeg's
libavcodec. It tries to predict each symbol as it arrives, and re-encodes the
symbols to the compressed file using arithmetic coding. When avrecode's
predictions are higher quality than the predictions specified by the H.264
standard, it achieves a better compression ratio.


Installing
----------

avrecode consists of a compression/decompression program and a fork of the
libavcodec library, part of the ffmpeg project. These live in separate github
repositories:

- https://github.com/dropbox/avrecode
- https://github.com/dropbox/libavcodec-hooks

The avrecode repository imports the libavcodec-hooks repository as a submodule,
so the `git submodule` command is used to keep them in sync.

Download the source:

```
git clone https://github.com/dropbox/avrecode
cd avrecode
git submodule update --init
```

Build and test:

```
brew install protobuf # for macOS
# apt-get -y build-essential git yasm pkg-config protobuf-compiler libprotobuf-dev # uncomment for Ubuntu 16.04
cd ffmpeg
./configure
make
cd ..
make
./recode roundtrip data/GOPR4542.MP4
```

Warning
-------
This is an experimental test bed for compression research: use on trusted inputs only
This tool does not validate input.

License
-------

avrecode is released under the BSD 3-clause license. See the LICENSE file for details.
The required libavcodec-hooks patch to ffmpeg is licenced under the LGPL.


Contributing
------------

avrecode was originally written by Chris Lesniewski during Dropbox Hack Week
January 2016. It is a redesign of the first version written by Daniel Horn,
Patrick Horn, Chris Lesniewski, and others during Dropbox Hack Week 2015.
We welcome external contributions, but ask that contributors accept our
Contributor License Agreement to grant us a license to distribute the code:

https://opensource.dropbox.com/cla/
