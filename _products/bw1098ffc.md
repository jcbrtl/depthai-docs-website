---
layout: default
title: DepthAI USB3 | Modular Cameras
toc_title: USB3 | Modular Cameras
screenshot: /images/products/depthai-edition-usb.jpg
description: DepthAI for the host of your choice, with modular cameras for easy integration onto/into your platform and custom stereo baselines.
order: 2
show_on_home: true
---

# {{page.title}}

{% include model_number.md %}

![screenshot]({{page.screenshot}})

Use DepthAI on your existing host. Since the AI/vision processing is done on the Myriad X, a typical desktop could handle tens of DepthAIs plugged in (the effective limit is how many USB ports the host can handle).

## Requirements

* Ubuntu 18.04 or Raspbian 10
* Cameras
  * [Modular color camera](/products/color_camera)
  * [Stereo camera pair](/products/stereo_camera_pair/) (if depth is required)
* USB3C cable
* USB3C port on the host
* [A supported Python version](/api/#python_version) on the host

## Board Layout

![USB Layout](/images/products/labeled/1098ffc.jpg)

<table class="table table-sm">
<tbody>
<tr>
<td>A. 5 V IN</td><td>E. Left Camera Port</td></tr>
<tr>
<td>B. USB3C</td><td>F. DepthAI Module</td></tr>
<tr>
<td>C. Right Camera Port</td><td>G. Myriad X GPIO Access</td></tr>
<tr>
<td>D. Color Camera Port</td><td></td></tr>
</tbody>
</table>

{: #in_box}
## What's in the box?

* {{page.title}} Carrier Board
* USB3C cable (6 ft.)
* Power Supply



## Setup

Follow the steps below to setup your DepthAI device.


<h3 class="step js-toc-ignore"><span></span> Connect your modular cameras.</h3>

{% include ffc_connection.md %}


<h3 class="step js-toc-ignore"><span></span> Connect your host to the DepthAI USB carrier board.</h3>

<h3 class="step js-toc-ignore"><span></span> Connect the DepthAI USB power supply (included).</h3>

<h3 class="step js-toc-ignore"><span></span> Install the Python DepthAI API.</h3>

[See our instructions](/api#python_version).

<h3 class="step js-toc-ignore"><span></span> Calibrate Stereo Cameras.</h3>

Have the stereo camera pair? Use the DepthAI [calibration script](/products/stereo_camera_pair/#calibration).

<h3 class="step js-toc-ignore"><span></span> Download and run DepthAI Python examples.</h3>

{% include test_step.md %}
