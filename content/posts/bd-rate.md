---
title: "Bjontegaard Delta-Rate Metric"
date: 2019-10-04
draft: true
category: "Video Compression"
tags: ["Video Compression", "PSNR", "Quality Metrics"]
#thumbnail: /images/bd-rate/rd-plot-delta-rate.png
---

The Bjontegard delta rate is used to compare two or more encoders.

<!--more-->

Let us have two video encoders we want to compare in terms of compression efficiency.
From now on by the term "compression efficiency" we will assume two measures:

1. bit rate of a compressed video sequence,
2. distortion introduced by lossy compression.

Both encoders provide interfaces to modify their compression settings, affecting
target bitrate and distortion.
Let us assume we have four different settings for each of two encoders.
We can mark our quality measures on rate-distortion plot as shown on figure
below with PSNR distortion metric.
Each point corresponds to compression results obtained with certain compression
settings.
Obviously, the higher the points are located, the more efficient the compression
is (less distortion and higher quality is provided at lower bit rates).

{{< figure src="/images/bd-rate/rd-plot.png" caption="Rate-Distortion Compression Efficiency Points of Two Hypothetical Video Encoders">}}
 
In the example above we obviously see the **Encoder A**
provides superior results to the **Encoder B**.
But how do we compare the efficiency in numerical form?

One way to do this is to fix one measurement and compare by another.
Let's say, we want to compare bit rate savings at PSNR distortion level
of 38 dB, but available measure points are not exactly there.
So we need to interpolate the rate-distortion curve to get an approximation
of the bit rate at the target distortion level (see figure below).

![](/images/bd-rate/rd-plot-interpol.png)

Rate-Distortion Compression Efficiency Points of Two Hypothetical Video
Encoders
With the help of interpolation we can compare compression efficiency of
the two encoders at a fixed distortion level.
But how do we compare compression efficiency on the whole interval of distortion
available? Obviously, integration needs to be applied.

First, RD-curves of two encoders are interpolated (see Fig. 2).
Second, delta rate values within the interpolation interval are accumulated,
which correspond to the area between two RD-curves (Fig. 3).
Finally, to get the average bit rate differences of the two encoders the
area between the two RD-curves has to be divided by interpolation interval.

![](/images/bd-rate/rd-plot-delta-rate.png)

Rate-Distortion Compression Efficiency Points of Two Hypothetical Video
Encoders

Interpolation of an RD-cure can be done in different ways: linear, polynomial,
spline interpolation.
The most advised is piecewise cubic interpolation [?Common test conditions].
Regardless of interpolation method used, there is a way to improve interpolatio
n accuracy and obtain more precise results.
As soon as the most wide-spread compression standards like MPEG-2, H.264/AVC,
H.265/HEVC, VP8, VP9, – they all have a quantization step, where quantization
value is proportional to a power of 2.
