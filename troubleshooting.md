---
layout: default
title: Troubleshooting ~ DepthAI
toc_title: Troubleshooting
description: Tips for common DepthAI issues.
order: 6
---

# DepthAI Troubleshooting

{: #disable_demo data-toc-title="Disable the startup demo"}
### How can the startup demo on the RPi Compute Edition be disabled?

Delete the autostart file:

```
rm /home/pi/.config/autostart/runai.desktop
```
<hr/>

{: #device_reset data-toc-title="Reset the Myriad X"}
### `depthai: Error initalizing xlink` errors and DepthAI fails to run.

The Myriad X needs to be reset. Click the "MODULE RST" or "RST" button on your carrier board.

On the RPi Compute edition, you can reset the Myriad X via the following shell commands:

```
raspi-gpio set 33 op  # set 33 as output
raspi-gpio set 33 dh  # drive high to reset Myriad X
sleep 1
raspi-gpio set 33 dl  # drive low to allow Myriad X to run
```

<hr/>

{: #depthai_import_error}
### ImportError: No module named 'depthai'

This indicates that the `depthai` was not found by your python interpreter. There are a handful of reasons this can fail:

1. Is the DepthAI API [installed](https://docs.luxonis.com/api/)? Verify that it appears when you type:
    ```
    python3 -m pip list | grep depthai
    ```
2. Are you using a [supported operating system](/api/#supported_platforms) for your operating system? If not, you can always [install depthai from source](/api/#compile_api):
    ```
    cat /etc/lsb-release
    ```

<hr/>

{: #slow_calibration data-toc-title="Slow camera calibration"}
### Why is the Camera Calibration running slow?

Poor photo conditions [can dramatically impact the image processing time](https://stackoverflow.com/questions/51073309/why-does-the-camera-calibration-in-opencv-python-takes-more-than-30-minutes) during the camera calibration. Under normal conditions, it should take 1 second or less to find the chessboard corners per-image on an RPi but this exceed 20 seconds per-image in poor conditions. Tips on setting up proper photo conditions:

* Ensure the checkerboard is not warped and is truly a flat surface. A high-quality option: [print the checkerboard on a foam board](https://discuss.luxonis.com/d/38-easy-calibration-targets-for-depthai-opencv-checkerboard).
* Reduce glare on the checkerboard (for example, ensure there are no light sources close to the board like a desk lamp).
* Reduce the amount of motion blur by trying to hold the checkerboard as still as possible.

<hr/>

{: #python_api_permission_denied data-toc-title-"pip install - permission denied"}
### [Errno 13] Permission denied: '/usr/local/lib/python3.7/dist-packages/...'

If `python3 -m pip install` fails with a `Permission denied` error, your user likely doesn't have permission to install packages in the system-wide path. 
Try installing in your user's home directory instead by adding the `--user` option. For example:

```
python3 -m pip install depthai --user
```

[More information on Stackoverflow](https://stackoverflow.com/questions/31512422/pip-install-failing-with-oserror-errno-13-permission-denied-on-directory).


### DepthAI does *not* show up under /dev/video* like web cameras do.  Why?

The USB device enumeration could be checked with lsusb | grep 03e7  . It should print:

`03e7:2485 after reset (bootloader running);`  
`03e7:f63b after the application was loaded.`

No `/dev/video*` nodes are created. 

DepthAI implements VSC (Vendor Specific Class) protocol, and libusb is used for communication.

### Intermittent Connectivity with Long (2 meter) USB3 Cables

- We've found that some hosts have trouble with USB3 + long cables (2 meter).  It seems to have something do do with the USB controller on the host side.  
- Other hosts have no problem at all and run for days (tested well over 3 days on some), even with long cables (tested w/ a total length of a bit over 8 feet).  For example, all Apple computers we've tested with have never exhibited the problem.
- Ubuntu 16.04 has an independent USB3 issue, seemingly only on new machines though.  We think this has to do w/ Ubuntu 16.04 being EOLed prior or around when these new machines having hit the market.  For example, on this computer ([here](https://pcpartpicker.com/list/KTDFQZ)) has rampant USB3 disconnect issues under Ubuntu 16.04 (with a 1 meter cable), but has none under Ubuntu 18.04 (with a 1 meter cable).

So unfortunately we discovered this after we shipped with long USB3 cables (2 meter cables) with DepthAI units.

So if you have see this problem with your host, potentially 3 options:
1. Switch to a shorter USB3 cable (say 1 meter) will very likely make the problem disappear.  [These](https://www.amazon.com/gp/product/B07S4G4L4Z/ref=ppx_yo_dt_b_asin_title_o00_s00?ie=UTF8&psc=1) 1 meter (3.3 ft.) cables are a nice length and are now what we ship with DepthAI USB3 variants.
2. Force USB2 mode with `--force_usb2` option (examples below).  This will allow use of the long cable still, and many DepthAI usecases do not necessitate USB3 communication bandwidth - USB2 is plenty.
3. Upgrade from Ubuntu 16.04 to Ubuntu 18.04.

#### Forcing USB2 Communication
`python3 depthai_demo.py --force_usb2`
Or, the shorter form:
`python3 depthai_demo.py -usb2`

We've also seen an unconfirmed issue of running Ubuntu-compiled libraries on Linux Mint.  If running on not Ubuntu 18.04/16.04 or Raspbian, please compile DepthAI from source (see [here](/api/#compile_api) for instructions).
