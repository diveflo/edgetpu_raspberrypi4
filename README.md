# Edge TPU Python API for Raspberry Pi 4

This repository contains an easy-to-use Python API to work with the [Coral USB Accelerator](https://coral.withgoogle.com/products/accelerator/).

It follow the original Google repo's [release-chef-branch](https://coral.googlesource.com/edgetpu/+/refs/heads/release-chef) but includes adaptions for the installation on the Raspberry Pi 4 running [Raspian Buster](https://www.raspberrypi.org/downloads/raspbian/). 

**Hint** The script installs the Edge TPU library for `arm32` architecture and `arm-linux-gnueabihf` gnu-type. However, if you're running a 64-bit linux distro, the generic ``aarch64`` detection and settings should work.

The instructions in the [official get started guide](https://coral.withgoogle.com/docs/accelerator/get-started/#set-up-on-linux-or-raspberry-pi) uses the build output of the above repo so we have to do a few more steps to get the Python API working.

### Requirements

* [Python 3.5 or 3.6](https://www.python.org/) and corresponding pip
* [picamera](https://github.com/waveform80/picamera)
* [pillow](https://github.com/python-pillow/Pillow)
* [wheel](https://github.com/pypa/wheel)

### Installation

After you've cloned the repo, change into the new directory and run:

```
bash build_package.sh
python3 setup.py sdist bdist_wheel
cd dist
pip3 install <current-edgetpu-version>-py3-none-any.whl
```

### Troubleshooting & Tips

##### Permission errors

In case you get permission errors from any of the above commands try `sudo`.

##### Pillow install fails

You might be missing libjpeg. [Stackoverflow thread](https://stackoverflow.com/questions/44043906/the-headers-or-library-files-could-not-be-found-for-jpeg-installing-pillow-on).

```
sudo apt-get install libjpeg-dev zlib1g-dev
pip3 install Pillow
```

##### Camera example `classify_capture.py` fails

The install script did not rename the `_edgetpu_cpp_wrapper.so` file for me.

```
cd /usr/local/lib/python3.6/site-packages/edgetpu/swig/
sudo cp _edgetpu_cpp_wrapper.cpython-35m-arm-linux-gnueabihf.so _edgetpu_cpp_wrapper.so
```

##### Install Python 3.6

Python 3.7 failed for me, so I had to get Python 3.6 installed. The instructions you find on the web usually recommend building it from source. The `make` step takes quite a bit of time on the Raspberry Pi.

`make -j4` makes use of all four cores on the Raspberry Pi - that's gonna save you quite a bit of time.