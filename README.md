# MosquitoNR
git archive of the MosquitoNR AVS plugin by b_inary

 Description

MosquitoNR is a noise reduction filter designed for mosquito noise, which is often caused by lossy compression such as MPEG.
This filter works as follows:

    1. Compute low frequency components of the image. 
    2. Apply direction-aware blur (smoothing). This blur effectively reduces compression artifacts, but also breaks detail. 
    3. Restore the low frequency components to the blurred image. 

MosquitoNR processes luma only, to correctly process chroma take a look at the example below.

Requirements

    [x86] AviSynth+ or AviSynth 2.6.0
    [x64] AviSynth+
    Progressive input only
    Supported color formats: YUY2, YV12, YV16, YV24, YV411, Y8 

    CPU with SSE2 support 


Syntax and Parameters

    MosquitoNR (clip, int "strength", int "restore", int "radius", int "threads") 


        clip   =

            Input clip 


        int  strength = 16

            Range: 0 - 32

                Sets the strength of the blur. 
                Setting this value higher brings stronger noise reduction effect, but side effect will also become stronger. 


        int  restore = 128

            Range: 0 - 128

                Sets the rate of restoring. 
                If set to 0, no restoring is performed. 
                If set to 128, low frequency components of the blurred image is completely replaced with those of the original image, and runs slightly faster. 


        int  radius = 2

            Range: 1 - 2

                Sets the radius of the blur. 
                1 is faster, but will have insufficient effect in some cases. 


        int  threads = 0

            Range: 0 - 32

                Controls how many threads are used. 
                By default, threads is set equal to automatically detected number of processors. 
                Since this filter needs a lot of memory access, thread efficiency is not very good. Setting this value lower might improve overall processing speed. 


Examples

MosquitoNR with default settings:

AviSource("Blah.avi")
MosquitoNR(strength=16, restore=128, radius=2, threads=0)


MosquitoNR processes only luma. If you want to process chroma too, you can write a script like the following, but there might be no noticeable change.

AviSource("Blah.avi")
MosquitoNR()
u = UtoY8().MosquitoNR() 
v = VtoY8().MosquitoNR()
YtoUV(u, v, last)


Changelog

Version      Date            Changes

v0.10        2013/11/29      - x64 version
                             - Compiled with Intel C++ Compiler XE 14.

v0.10        2013/03/14      - fixed a crash with AviSynth MT
                             - added support for YV16, YV24, Y411, Y8
                             - some speed optimizations
                             - added English documentation

v120210      2012/02/10      - initial release
