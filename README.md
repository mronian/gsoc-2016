# Google Summer of Code 2016

<sub>[Summary of Contributions](#summary-of-contributions) | [Before the Coding Period](#before-the-coding-period) | [Exposure Correction](#exposure-correction) | [Feature Extraction](#feature-extraction) | [Miscelleanous](#miscellaneous)</sub>

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

## Feature Extraction
### Canny Edge Detection
### Corner Detection
### FAST Corners
### BRIEF Descriptors
### Gray Level Co Occurence Matrices
### Local Binary Patterns
### ORB Keypoints and Descriptors
### CENSURE Keypoints
### BRISK Descriptors
### FREAK Descriptors

## Miscellaneous
### ColorVectorSpace.jl
### ColorTypes.jl
