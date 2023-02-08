# SphericalHarmonics
Spherical harmonics for radiance maps in Python (numpy). 

I wrote this code because I found it was difficult to obtain coefficients for a radiance map from existing libraries (especially in Python).


Features:
- Obtain coefficients for a radiance map (in equirectangular format)

!!! 1 radiance map，指环境贴图（Environment maps）

- Numpy vectorised for efficiency
- Windowing function for reducing ringing artefacts

!!! 4 两个去除振铃效应的方法
    A low pass filter is done in the frequency domain by smoothing out the higher frequency coefficients (this can be done by windowing). I have two ways of supporting this. The main one is "applyWindowing(...)". The second is the "filterAmount" variable (which just blurs the input image before calculating the coefficients). Alternatively you can do your own weighted falloff on the coefficients.

    Then use the coefficients to reconstruct the radiance map.

    This is basically a way of reconstructing the light with less ringing artifacts.

    Let me know how it goes.
- Reconstruct radiance map from coefficients

!!! 2 从球谐系数复原环境贴图，可以选择用足够多的球谐系数完整复原，也可以选择用少量低阶系数复原出环境贴图中的低频部分。
- Obtain diffuse BRDF coefficients
- Render a diffuse map (given radiance map coefficients)

!!! 3 diffuse map，指的是能直接渲染朗伯体的图，入射光线与法向的效果之和。等同于辐照度环境贴图（irradiance environment map），是一张储存各法向辐照度的图，是由表面法线索引的漫反射贴图。
- Supports an arbitrary number of bands 
- Plot the spherical harmonics in a figure
- Render a ground truth diffuse map to compare with. 

The ground truth diffuse map can be a little slow to compute, so I've added the ability to render the diffuse values at a low resolution while sampling the high resolution source image. After rendering at a low resolution, I increase the resolution (so it's easier to see) using Lanczos interpolation. I found doing it this way was the most efficient while also producing high quality ground truth images.

!!! 4 Lanczos插值具有速度快，效果好，性价比最高的优点，这也是目前此算法比较流行的原因
# Usage
python sphericalHarmonics.py [string filename.ext] [int nBands]

Example:
python sphericalHarmonics.py radianceMap.exr 2

See the main function to see examples of functions you can utilise in your own code.

# References
- Ramamoorthi, Ravi, and Pat Hanrahan. "An efficient representation for irradiance environment maps", 2001.
- Sloan, Peter-Pike, Jan Kautz, and John Snyder. "Precomputed radiance transfer for real-time rendering in dynamic, low-frequency lighting environments", 2002.
- "Spherical Harmonic Lighting: The Gritty Details" by Robin Green
- "Stupid Spherical Harmonics (SH) Tricks" by Peter Pike Sloan
- PBRT source code: https://www.csie.ntu.edu.tw/~cyy/courses/rendering/pbrt-2.00/html/sh_8cpp_source.html
- Probulator source code (I based my windowing code on this): https://github.com/kayru/Probulator
- Some radiance maps can be downloaded here: http://gl.ict.usc.edu/Data/HighResProbes/

Things TODO if people want it:
- Support other formats (e.g. cubemap, angular map, etc.)
- Change, remove, or support other modules (e.g. I use imageio for reading HDR images, cv2 for resizing, etc.)
- More optimisations
- Other windowing methods
- Other visualisations
- Restructure code
- and more. Feel free to message me.

# Dependencies
You can use pip to install the modules I've used. The only gotcha is with imageio. By default it does not provide OpenEXR support.
To add OpenEXR support to imageio, see here:
https://imageio.readthedocs.io/en/stable/_autosummary/imageio.plugins.freeimage.html#module-imageio.plugins.freeimage

e.g., run the following in python (after installing imageio):
imageio.plugins.freeimage.download()

# License
MIT License

Copyright (c) 2018 Andrew Chalmers

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
