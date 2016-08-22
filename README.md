# Google Summer of Code 2016

<sub>[Before the Coding Period](#before-the-coding-period) | [Summary of Contributions](#summary-of-contributions) | [Exposure Correction](#exposure-correction) | [Feature Extraction](#feature-extraction) | [Miscelleanous](#miscellaneous) | [Future Work](#future-work)</sub>

My [proposal](https://docs.google.com/document/d/1XD_fpT6YpyK6Iv2Rues2RlU-15l4aBbUZTSz1V216pw/edit?usp=sharing) for this summer was to work on [ImageFeatures.jl](https://github.com/JuliaImages/ImageFeatures.jl), a new Julia package for Feature Extraction and Descriptors in Images. Alongside this, I planned to add Exposure Correction functionality to [Images.jl](https://github.com/timholy/Images.jl), an existing package for Image Processing in Julia.

As we reach the final week of GSoC, it feels great to have managed to achieve most of my goals for the summer abd be able to work with amazing people in the [JuliaImages](https://github.com/JuliaImages) organisation as we attempt to develop a full fledged Computer Vision library of which ImageFeatures.jl is an essential part. I hope to continue to work and contribute more packages above and beyond GSoC as we slowly build up the organisation.

I would like to thank my mentors [Tim Holy](https://github.com/timholy) and [Simon Danisch](https://github.com/SimonDanisch) for helping me out throughout the summer and guiding me towards achieving my goals. Their friendliness and approachability made the summer a enjoyable and fruitful experience.

## Before the Coding Period
### TestImages.jl

In the initial phase of drafting my proposal, I had played around with Images.jl and TestImages.jl to understand the packages and try to make some initial contributions to the same. In the process, I felt that TestImages.jl needed a proper documentation along with a guide for contributing test images. Apart from the documentation, there was also a need to store the test images in a proper repository so that the links would not expire. Once my proposal was selected, I started working on this and with help from Tim Holy, I developed a new website for TestImages.jl along with an improved API to easily download images from the repository. The new API checks if the requested image is present in the online repository if it is not found locally and downloads it for the user.

The documentation can be viewed [here](https://timholy.github.io/TestImages.jl).

### Histograms

To get familiar with open source and Images.jl, one of my first PRs was to add the histogram functionality. This was a very simple function which would calculate the histogram of an image i.e. the intensity distribution.

```julia
edges, count = imhist(img, nbins)
edges, count = imhist(img, nbins, minval, maxval)
```
This generates a histogram for the image over `nbins` spread between `(minval, maxval]`. If `minval` and `maxval` are not given, then the minimum and maximum values present in the image are taken. `edges` specifies the range of the bins while `count` is a vector of the individual counts of the bins.

## Summary of Contributions
| Package | Merged Commits | Pull Requests (Open and Merged) |
|---------|----------------|---------------------------------|
| [Images.jl](https://github.com/timholy/Images.jl) | [Link](https://github.com/timholy/Images.jl/commits/master?author=mronian) | [Link](https://github.com/timholy/Images.jl/pulls?utf8=%E2%9C%93&q=is%3Apr%20author%3Amronian%20)
| [ImageFeatures.jl](https://github.com/JuliaImages/ImageFeatures.jl) | [Link](https://github.com/JuliaImages/ImageFeatures.jl/commits/master?author=mronian) | [Link](https://github.com/JuliaImages/ImageFeatures.jl/pulls?utf8=%E2%9C%93&q=is%3Apr%20author%3Amronian%20)
| [ImageDraw.jl](https://github.com/JuliaImages/ImageDraw.jl) | [Link](https://github.com/JuliaImages/ImageDraw.jl/commits/master?author=mronian) | [Link](https://github.com/JuliaImages/ImageDraw.jl/pulls?utf8=%E2%9C%93&q=is%3Apr%20author%3Amronian%20)
| [TestImages.jl](https://github.com/timholy/TestImages.jl) | [Link](https://github.com/timholy/TestImages.jl/commits/master?author=mronian) | [Link](https://github.com/timholy/TestImages.jl/pulls?utf8=%E2%9C%93&q=is%3Apr%20author%3Amronian%20)
| [ColorVectorSpace.jl](https://github.com/JuliaGraphics/ColorVectorSpace.jl) | [Link](https://github.com/JuliaGraphics/ColorVectorSpace.jl/commits/master?author=mronian) | [Link](https://github.com/JuliaGraphics/ColorVectorSpace.jl/pulls?utf8=%E2%9C%93&q=is%3Apr%20author%3Amronian%20)
| [ColorTypes.jl](https://github.com/JuliaGraphics/ColorTypes.jl) | [Link](https://github.com/JuliaGraphics/ColorTypes.jl/commits/master?author=mronian) | [Link](https://github.com/JuliaGraphics/ColorTypes.jl/pulls?utf8=%E2%9C%93&q=is%3Apr%20author%3Amronian%20)

## Exposure Correction

The first part of my proposal was to add exposure correction functionality to Images.jl. This was a bit challenging considering the various type of Images and ColorTypes in julia and my goal was to provide a common API which would handle all types of Images using julia's method dispatch. I am happy to say that we were able to do this to a considerable extent and all the functions are easy to use and intuitive, offering a lot of options at the same time.

### Histogram Equalisation

My first contribution during the coding period was the histogram equalisation functionality. This took up quite a bit of time as I worked on optimising the performance and got to learn about the inner workings of Arrays (Images are derived from Arrays!) in julia and how to write efficient functions for operating on them.  I was also introduced to `@code_warntype` and how to julia infers the type of variables in the code and how to best exploit this to increase performance. Tim Holy was really helpful and patient (Thanks Tim!) throughout the initial phase with a lot of code reviews and advice which really helped me later on.

```julia
hist_equalised_img = histeq(img, nbins)
hist_equalised_img = histeq(img, nbins, minval, maxval)
```

This returns a histogram equalised image with a granularity of approximately `nbins` number of bins. If `minval` and `maxval` are specified then intensities are equalized to the range `(minval, maxval)`. The default values are `0` and `1`.

### Histogram Matching

Histogram matching is another way of achieving exposure correction by trying to modify the histogram of the image to match that of a predefined image with the desired histogram.

```julia
hist_matched_img = histmatch(img, oimg, nbins)
```

This returns a grayscale histogram matched image with a granularity of `nbins` number of bins. `img` is the image to be matched and `oimg` is the image having the desired histogram to be matched to.

### Gamma Correction

Gamma correction is a method of enhancing or reducing the intensity levels in an image by using a power function.

```julia
gamma_corrected_img = adjust_gamma(img, gamma)
```

The `adjust_gamma` function can handle a variety of input types. The returned image depends on the input type. If the input is an `Image` then the resulting image is of the same type and has the same properties.

### Contrast Limited Adaptive Histogram Equalisation

An advanced version of histogram equalisation, it differs from ordinary histogram equalization in the respect that the adaptive method computes several histograms, each corresponding to a distinct section of the image, and uses them to redistribute the lightness values of the image. It is therefore suitable for improving the local contrast and enhancing the definitions of edges in each region of an image.

In the straightforward form, CLAHE is done by calculation a histogram of a window around each pixel and using the transformation function of the equalised histogram to rescale the pixel. Since this is computationally expensive, we use interpolation which gives a significant rise in efficiency without compromising the result. The image is divided into a grid and equalised histograms are calculated for each block. Then, each pixel is interpolated using the closest histograms.

```julia
hist_equalised_img = clahe(img, nbins, xblocks = 8, yblocks = 8, clip = 3)
```

The `xblocks` and `yblocks` specify the number of blocks to divide the input image into in each direction. `nbins` specifies the granularity of histogram calculation of each local region. `clip` specifies the value at which the histogram is clipped. The excess in the histogram bins with value exceeding `clip` is redistributed among the other bins.

## Feature Extraction

After working on exposure correction, I began with the other major chunk of my proposal which was ImageFeatures.jl. Edge and corner detection was already a part of Images.jl so I added some of my work in the same to Images.jl itself (this will be moved to ImageFeatures.jl before release). 

I started ImageFeatures.jl by adding texture matching descriptors like GLCMs (Gray Level Co-occurence Matrix) and LBPs (Local Binary Patterns). Before starting with the feature descriptors, we decided on a easy to use common API which could be used with all the algorithms. 

```julia
keypoints = extract_features(img, params)
```

The `params` argument is dependent on the algorithm chosen and its type is the name of the algorithm. For eg. `censure_params <: CENSURE`


```julia
descriptor, ret_keypoints = create_descriptor(img, params)
descriptor, ret_keypoints = create_descriptor(img, keypoints, params)
```

Depending on the algorithm , the `create_descriptor` API can be used to directly create a feature descriptor from the image (eg. ORB, BRISK) or from the keypoints (eg. BRIEF, FREAK). In case of the latter, the keypoints can be obtained using algorithms such as FAST or CENSURE. The `params` argument is dependent on the algorithm chosen and its type is the name of the algorithm. For eg. `brief_params <: BRIEF`

### Canny Edge Detection

The canny edge detector works by finding intensity gradients of an image and then double thresholding pixels as being part of weak or strong edges. Then the weak edges not connected to any strong edge are discarded and the result has the edges detected in the image.

```julia
canny_edges = canny(img, sigma = 1.4, upperThreshold = 0.80, lowerThreshold = 0.20)
```

### Corner Detection

### FAST Corners

### BRIEF Descriptors

### Gray Level Co Occurence Matrices

### Local Binary Patterns

### ORB Keypoints and Descriptors

### CENSURE Keypoints

### BRISK Descriptors

The BRISK () descriptor has a predefined sampling pattern as compared to BRIEF or ORB. Pixels are sampled over concentric rings. 

The BRISK descriptor can be used by calling the BRISK constructor method to define the parameters and then using it with the `create_descriptor` API. 

```julia
brisk_params = BRISK(pattern_scale = 1.0)
desc, ret_keypoints = create_descriptor(img_array, keypoints, brisk_params)
```

### FREAK Descriptors

The FREAK (Fast REtinA Keypoint) descriptor, similar to BRISK as a predefined sampling pattern. It uses a retinal sampling grid with more density of points near the centre with the density decreasing exponentially with distance from the centre.

The FREAK descriptor can be used by calling the FREAK constructor method to define the parameters and then using it with the `create_descriptor` API. Unlike BRISK, FREAK does not calculate the keypoints.

```julia
freak_params = FREAK(pattern_scale = 22.0)
desc, ret_keypoints = create_descriptor(img_array, keypoints, freak_params)
```

## ImageDraw.jl

## Miscellaneous

### ColorVectorSpace.jl
### ColorTypes.jl

## Future Work

The BRISK and CENSURE PRs are yet to be merged. The BRISK PR is currently under review while the CENSURE one needs more tests before it can be merged. I hope to finish them before the GSoC period ends.

I am also working on creating a set of tutorials on extracting features and using them to match images. The link to the tutorials can be found at the ImageFeatures.jl documentation [website](http://juliaimages.github.io/ImageFeatures.jl/latest).
