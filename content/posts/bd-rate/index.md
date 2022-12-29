---
title: "Bjontegaard Delta-Rate Metric"
date: 2022-12-29
draft: false
category: "Video Compression"
tags: ["Video Compression", "PSNR", "Quality Metrics"]
mathjax: false
thumbnail: /blog/posts/bd-rate/rd-plot-delta-rate.png
---

A video encoder can be configured in many ways: different GOP (Group of Pictures) structure,
different quantization parameters (QP) or target bitrate,
motion compensation range, etc. Furthermore, different encoders likely produce different compressed streams.
Depending on the encoder and the configuration the same source video is compressed differently, each having its own bitrate and distortion value.
Bjontegaard Delta-Rate (BD Rate) metric can be used to compare and choose the encoder or configuration, that provides the best compression results.

<!--more-->

### Rate-Distortion Plot

Assume there are two video encoders to compare in terms of compression efficiency.
Here by the term "compression efficiency" the following two measures are assumed:

1. Bitrate (bits per second) of a compressed video sequence.
2. Video distortion introduced by lossy compression.

Both encoders allow configuring their compression settings, affecting
target bitrate and distortion.
Within a single configuration the target bitrate can be varied (varying the quantization parameters)
producing different bitrate and distortion.

Let us assume we have four different compression results for each of the two encoders (A and B).
Marking the bitrate on the x-axis and PSNR distortion on the y-axis the rate-distortion plot can be drawn
as shown on figure below.
Obviously, the higher the points are located, the more efficient the compression
is (***less distortion and higher visual quality is provided at lower bit rates***).

{{< figure src="rd-plot.png" caption="Rate-Distortion Compression Efficiency Points of Two Hypothetical Video Encoders">}}

In the example above we obviously see the **Encoder A**
provides superior results to the **Encoder B**.  
<mark>How do we compare the efficiency in numerical form?</mark>

### Rate-Distortion Curve

One way to compare the efficiency in numerical form is to fix one measurement and compare by another.
Let's say, we want to compare bit rate savings at a certain PSNR distortion level
of 38 dB, but available measure points are not exactly there.
We need to interpolate the rate-distortion curve to get an approximation
of the bit rate at the target distortion level (see figure below).

{{< figure src="rd-plot-interpol.png" caption="Rate-Distortion Curves">}}

With the help of interpolation we now have two RD curves instead of points.
Interpolation of an RD-cure can be done in different ways: linear, polynomial,
spline interpolation. The most widely adopted is the piecewise cubic interpolation.

By measuring the distance between the two curves at the certain distortion level
the bitrate "savings" of the one encoder compared to the other one at this distortion level can be compared numerically.

But how do we compare compression efficiency on the whole distortion interval
available?

### BD Rate Metric

Bjontegaard Delta-Rate (BD Rate) metric, proposed in 2001 by Gisle Bjontegaard, is a method for calculating
the average difference between two rate-distortion (RD) curves
[[VCEG-M33]]({{< ref "#references" >}} "Calculation of Average PSNR Differences between RD curves").

The basic steps are:

1. fit a curve through several data points (see [Rate-Distortion Curve]({{< ref "#rate-distortion-curve" >}}));
2. find an expression for the integral of the curve [[VCEG-M33]]({{< ref "#references" >}} "Calculation of Average PSNR Differences between RD curves");
3. the average difference is the difference between the integrals divided by the integration interval.

In other words, to compare two encoders (or two encoding configurations) the
area between the two RD-curves has to be divided by interpolation interval as shown below.

{{< figure src="rd-plot-delta-rate.png" caption="RD Curves Delta Rate">}}

BD-Rate is measured in percent and expresses the average percentage difference in bitrate of the two datasets
at a similar distortion value measured by an objective metric (usually PSNR).
The BD Rate value is zero when calculated metrics are the same.
A negative BD Rate value of Encoder A to Encoder B comparison would mean Encoder A provides better
compression efficiency (lower bitrate at a similar visual quality level).
A positive value would speak for inferior compression efficiency.

If PSNR distortion value is compared using the bitrate as the basis of integration,
the metric is called BD-PSNR and is measured in dB.

It was observed that the difference between the curves is dominated by the high bitrates.
The higher the bitrate is, the wider is usually the distance between the curves.
Hence it is considered to be more appropriate to do the integration based on logarithmic scale of bitrate
[[VCEG-M33]]({{< ref "#references" >}} "Calculation of Average PSNR Differences between RD curves").  
Another approach is to split the curve into three different quality ranges: low, medium and high;
and additionally calculate the metric for low and high quality ranges
[[VCEG-AI11\]]({{< ref "#references" >}} "Improvements of the BD-PSNR model.").

The piecewise cubic interpolation combined with logarithmic scale was later found to provide
more stable results e.g. compared to a regular cubic interpolation
[[VCEG-AL22]]({{< ref "#references" >}} "Reliability metric for BD measurements").

The impact of the number of existing curve fitting approaches is studied in
[[MHV-22]]({{< ref "#references" >}} "Revisiting Bjontegaard delta bitrate computation").
The study also shows that the use of BD Rate metric computation
for metrics other than PSNR should be done with caution, as they might
produce unreliable results.

The BD Rate metric is widely adopted by the ITU-T JCT-VC group
[[STP-VID-WPOM]]({{< ref "#references" >}} "Working practices using objective metrics...").

## Useful Links

- BD Rate estimation in Python. [GitHub: Anserw/Bjontegaard_metric](https://github.com/Anserw/Bjontegaard_metric).
- BD Rate estimation in C++17. [GitHub: tbr/bjontegaard_cpp](https://github.com/tbr/bjontegaard_cpp).
- BD Rate estimation in TypeScript. [GitHub: YoungSx/bjontegaard.js](https://github.com/YoungSx/bjontegaard.js).
- BD Rate estimation in VBA (e.g. Excel). [GitHub: tbr/bjontegaard_etro](https://github.com/tbr/bjontegaard_etro).
- **Subjective** Comparison of ENcoders based on fItted Curves (SCENIC). [GitHub: phanhart/SCENIC](https://github.com/phanhart/SCENIC).
- [BD-rate: one name - two metrics. AOM vs. the World.](https://vicuesoft.com/blog/titles/bd_rate_one_name_two_metrics/) ViCue Soft blog.

## References

**\[VCEG-M33\]** Gisle Bjontegaard, "[Calculation of Average PSNR Differences between RD curves](https://www.itu.int/wftp3/av-arch/video-site/0104_Aus/VCEG-M33.doc)", ITU-T SG16/Q6 VCEG 13th meeting, Austin, Texas, USA, April 2001, Doc. VCEG-M33.

**\[VCEG-AI11\]** Gisle Bjontegaard, "[Improvements of the BD-PSNR model](http://wftp3.itu.int/av-arch/video-site/0807_Ber/)", ITU-T SG16/Q6 VCEG 35th meeting, Berlin, Germany, 16–18 July, 2008, Doc. VCEG-AI11.

**\[VCEG-AL22\]** Kenneth Andersson, Rickard Sjöberg, and Andrey Norkin. 2009. Reliability metric
for BD measurements. ITU-T SG16 Q.6 Document, VCEG-AL22.

**\[MHV-22\]** Nabajeet Barman, Maria G Martini, and Yuriy Reznik. 2022. Revisiting Bjontegaard delta bitrate (BD-BR) computation
for codec compression efficiency comparison. In Proceedings of the 1st Mile-High Video Conference (MHV '22).
Association for Computing Machinery, New York, NY, USA, 113–114. https://doi.org/10.1145/3510450.3517289.

**\[HSTP-VID-WPOM\]** ITU-T Technical Paper. 2020. HSTP-VID-WPOM - Working practices using
objective metrics for evaluation of video coding efficiency experiments.
