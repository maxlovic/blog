---
title: "Next Generation Video Codecs: HEVC, VP9, Daala (2013)"
date: 2013-10-22
draft: false
thumbnail: /blog/posts/nextgen-codecs-2013/vp9_vs_hevc_vs_daala.png
---

Nowadays video services (digital television, Internet video etc.) are a common part of our live. According to Cisco [1] mobile video traffic was 51 percent of the entire global Internet traffic by the end of 2012 and it is expected to be 66 percent by 2017.
The demand on video and its quality is very high. Since the appearance of VoIP in 1984 video resolution has increased from QCIF (176×144 pixels) to Full HD (1920×1080). Now it is evolving to 4K (3840×2160) and 8K (7680×4320) UHD. But the higher the resolution and quality is, the higher bitrate it takes.
This paper contains a general review of new video compression standards HEVC, VP9 and Daala. Their compression efficiency is studied and compared to AVC.

<!--more-->

### About the Study

The paper was reported at the XI InternationalTheoretical and Practical Conference of Students and Young Scientists ["outh and Contemporary Information Technology"](http://msit.tpu.ru/eng/). The report can be found [here](https://onedrive.live.com/?authkey=%21AMRK0hFZgmikW40&cid=813D418DFB2CD243&id=813D418DFB2CD243%21757&parId=813D418DFB2CD243%21112&o=OneUp).

The post is migrated from my [old blog on blogpost](https://maxsharabayko.blogspot.com/2013/10/next-generation-video-codecs-hevc-vp9.html).

### HEVC

High Efficiency Video Coding is an evolution of current industrial H.264/AVC standard [2].
AVC was adopted in 2003 and determined the development of HD television.
HEVC was developed to increase AVC compression efficiency by two times and endorse the development
of UHD systems. HEVC is expected to replace AVC in newly developed video systems.

H.265/HEVC [3] is still a block-based DFT codec with the same general design.
One of the main improvements is the increase of maximal block (called Largest Coding Unit - LCU) size from 16×16 to 64×64 pixels.
It aims to improve the efficiency of block partitioning on high resolution video sequences (bitrate savings are about 16% [4]).
Larger blocks provoke the introduction of quad-tree partitioning with adaptive block sub-splitting and adaptive prediction units
and transform units partitioning. Additional 25 intra prediction directions were added to improve intra-frame coding efficiency.
Also there came more motion vector prediction candidates and several other modifications to improve inter-frame compression performance.

### VP9

Superior video compression efficiency of HEVC is able to provide network usage savings,
which should be of a great interest to Google YouTube video service.
The development of free-to-use video compression standard with the efficiency comparable to proprietary HEVC
could become financially successful. That came one of the reasons for Google to create VP9 [5] compression standard.

Google VP9 basically shares common features with AVC coding like VP8 did.
The main evolutional change is also the increase of largest block (called 'super block')
size up to 64×64 pixels and its adaptive sub-splitting. Motion vector prediction was improved,
unlike intra prediction that still has only 10 modes.

### Daala

Unlike HEVC and VP9, Daala [6] is being designed to step aside common video compression techniques. Among the key distinctive features there are lapped transforms instead of block-based DFT, lifting pre- and post-filtering instead of deblock filtering, frequency domain intra prediction, Time/Frequency Resolution Switching etc.

The development of Daala codec is still in progress. It is designed patent free and might potentially become the next generation video codec in 5-10 years if the novel techniques would prove to be successful.

Comparison results For comparison purposes open-source implementations of the reviewed codecs are used. HEVC compression efficiency is measured with HM Test Model. Verification of coding parameters is done with Elecard HEVC Analyzer [7]. For AVC estimation JM reference encoder and Elecard Stream Analyzer [7] are used. Both JM and HM utilize rate-distortion optimization (RDO) techniques to achieve quasi-optimal correlation between compression rate and quality. Both encoder implementations are evaluated in “constant quantizer” mode.

H.264/AVC is an industrial video compression standard at the moment. It was adopted in 2003 and determined the development of HD television. In April 2013 next generation video compression standard H.265/HEVC appeared. It was developed by JCT-VC to increase AVC compression efficiency by two times and endorse the development of UHD systems. As AVC the new standard is proprietary and thus its usage in not free That's one of the main reasons for development of Google VP9 compression standard. Being free from patent claims VP9 should achieve similar compression rates as HEVC.

### Comparison Results

For comparison purposes open-source implementations of the reviewed codecs are used. HEVC compression efficiency is measured with HM Test Model. Verification of coding parameters is done with Elecard HEVC Analyzer [7]. For AVC estimation JM reference encoder and Elecard StreamEye [7] are used. Both JM and HM utilize rate-distortion optimization (RDO) techniques to achieve quasi-optimal correlation between compression rate and quality. Both encoder implementations are evaluated in “constant quantizer” mode.

Estimation of VP9 performance is carried out with the VPX encoder from The WebM Project as it is the only implementation of this standard. CodecVisa VP9 Analyzer [8] is used for verification of compression parameters. VPX encoder is configured with “constrained quality” mode and limited quantization parameter to emulate “constant quantizer” mode. 

Daala is still under development and the only encoder implementation available belongs to Xiph open source community, which is, in fact, developing this standard. Verification of compression parameters is performed manually as no analyzer is available. Only 'constrained quality' mode is available and used in experiments, which, by the way, should provide superior efficiency to “constant quantizer” mode.

Experiments are carried out on JCT-VC video sequences:  BasketballDrill (832×480 @ 50 Hz), Cactus (1920×1080 @ 50 Hz) and Traffic (2560×1600 @ 30 Hz) [9].

For key-frame compression efficiency estimation HM encoder is tested in “All Intra – Main” configuration, JM encoder is tested in “Intra HE” configuration. VP9 and Daala encoders are configured manually.

{{< figure src="BDrill_intra_color.jpg" caption="Fig. 1. Intra-frame compression efficiency on BasketballDrill sequence.">}}

{{< figure src="Cactus_intra_color.jpg" caption="Fig. 2. Intra-frame compression efficiency on Cactus sequence.">}}

{{< figure src="Traffic_intra_color.jpg" caption="Fig. 3. Intra-frame compression efficiency on Traffic sequence.">}}

{{< figure src="BDrill_inter_color.jpg" caption="Fig. 4. Inter-frame compression efficiency on BasketballDrill sequence.">}}

For inter-frame compression efficiency estimation HM encoder is tested in “Low-delay B – Main” configuration, JM encoder is tested in “Random Access B HE” configuration. VP9 and Daala encoders are configured manually. Key frame is set to be only the first in the coded sequence.

{{< figure src="Cactus_inter_color.jpg" caption="Fig. 5. Inter-frame compression efficiency on Cactus.">}}

{{< figure src="Traffic_inter_color.jpg" caption="Fig. 6. Inter-frame compression efficiency on Traffic.">}}

### Conclusion

{{< figure src="fighters.jpg" width="50%" >}}

Experimental results obviously show that Daala video encoder is still rather far from being competitive. While HM provides 31% better compression rates in key-frame only mode and about 40% improvement in inter-coding mode compared to JM, VP9 is only 18% better than JM in both modes. It is worth mentioning that VP9 encoder doesn’t have great RDO model, so the VP9 standard itself may potentially have better performance.

### References

1. Cisco Visual Networking Index: Global Mobile Data Traffic Forecast Update, 2012–2017. White paper. – Cisco, February 2013.
2. Recommendation H.264: Advanced video coding for generic audiovisual services. - ITU, June 2011.
3. Recommendation H.265: High efficiency video coding. - ITU, April 2013.
4. Ponomarev O.G., Sharabayko M.P., Posdnyakov A.A. Analiz effektivnosty metodov i algoritmov videokompressii standarta H.265/HEVC // Elektrosvyaz. – 2013 – №. 3. – pp. 29-33 (in Russian).
5. The WebM Project VP9 Video Codec. – URL: http://www.webmproject.org/vp9/ (14.10.2013).
6. Xiph.org Daala video. – URL: https://xiph.org/daala/ (14.10.2013).
7. Elecard Video analysis products. – URL: http://www.elecard.com/en/products/professional/analysis
8. CodecVisa Cloud. – URL: http://codecvisa.codecian.com/ (14.10.2013).
9. JCT-VC test sequences. – URL: ftp://ftp.tnt.uni-hannover.de/testsequences (14.10.2013).

### Related Papers

- Dan Grois, Detlev Marpe, Amit Mulayoff, Benaya Itzhaky and Ofer Hadar, Performance Comparison of H.265/MPEG-HEVC, VP9, and H.264/MPEG-AVC Encoders (Preprint) // 30th PICTURE CODING SYMPOSIUM 2013 (PCS 2013), San Jose, CA, USA, Dec 8-11, 2013.

- Mukherjee, D., Bankoski, J., Grange, A., Jingning Han, Koleszar, J., Wilkins, P., Yaowu Xu, Bultje, R. The latest open-source video codec VP9 - An overview and preliminary results // Picture Coding Symposium (PCS), 2013, San Jose, CA, USA, Dec 8-11, 2013, pp.390 - 393
