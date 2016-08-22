# Google Summer of Code 2016

<sub>[Summary of Contributions](#summary-of-contributions) | [Before the Coding Period](#before-the-coding-period) | [Exposure Correction](#exposure-correction) | [Feature Extraction](#feature-extraction) | [Miscelleanous](#miscellaneous)</sub>

My [proposal](https://docs.google.com/document/d/1XD_fpT6YpyK6Iv2Rues2RlU-15l4aBbUZTSz1V216pw/edit?usp=sharing) for this summer was to work on [ImageFeatures.jl](https://github.com/JuliaImages/ImageFeatures.jl), a new Julia package for Feature Extraction and Descriptors in Images. Alongside this, I planned to add Exposure Correction functionality to [Images.jl](https://github.com/timholy/Images.jl), an existing package for Image Processing in Julia.

As we reach the final week of GSoC, it feels great to have managed to achieve most of my goals for the summer abd be able to work with amazing people in the [JuliaImages](https://github.com/JuliaImages) organisation as we attempt to develop a full fledged Computer Vision library of which ImageFeatures.jl is an essential part. I hope to continue to work and contribute more packages above and beyond GSoC as we slowly build up the organisation.

I would like to thank my mentors [Tim Holy](https://github.com/timholy) and [Simon Danisch](https://github.com/SimonDanisch) for helping me out throughout the summer and guiding me towards achieving my goals. Their friendliness and approachability made the summer a enjoyable and fruitful experience.

## Summary of Contributions
| Package | Merged Commits | Pull Requests (Open and Merged) |
|---------|----------------|---------------------------------|
| [Images.jl](https://github.com/timholy/Images.jl) | [Link](https://github.com/timholy/Images.jl/commits/master?author=mronian) | [Link](https://github.com/timholy/Images.jl/pulls?utf8=%E2%9C%93&q=is%3Apr%20author%3Amronian%20)
| [ImageFeatures.jl](https://github.com/JuliaImages/ImageFeatures.jl) | [Link](https://github.com/JuliaImages/ImageFeatures.jl/commits/master?author=mronian) | [Link](https://github.com/JuliaImages/ImageFeatures.jl/pulls?utf8=%E2%9C%93&q=is%3Apr%20author%3Amronian%20)
| [ImageDraw.jl](https://github.com/JuliaImages/ImageDraw.jl) | [Link](https://github.com/JuliaImages/ImageDraw.jl/commits/master?author=mronian) | [Link](https://github.com/JuliaImages/ImageDraw.jl/pulls?utf8=%E2%9C%93&q=is%3Apr%20author%3Amronian%20)
| [TestImages.jl](https://github.com/timholy/TestImages.jl) | [Link](https://github.com/timholy/TestImages.jl/commits/master?author=mronian) | [Link](https://github.com/timholy/TestImages.jl/pulls?utf8=%E2%9C%93&q=is%3Apr%20author%3Amronian%20)
| [ColorVectorSpace.jl](https://github.com/JuliaGraphics/ColorVectorSpace.jl) | [Link](https://github.com/JuliaGraphics/ColorVectorSpace.jl/commits/master?author=mronian) | [Link](https://github.com/JuliaGraphics/ColorVectorSpace.jl/pulls?utf8=%E2%9C%93&q=is%3Apr%20author%3Amronian%20)
| [ColorTypes.jl](https://github.com/JuliaGraphics/ColorTypes.jl) | [Link](https://github.com/JuliaGraphics/ColorTypes.jl/commits/master?author=mronian) | [Link](https://github.com/JuliaGraphics/ColorTypes.jl/pulls?utf8=%E2%9C%93&q=is%3Apr%20author%3Amronian%20)

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

## Exposure Correction

### Histogram Equalisation
### Histogram Matching
### Gamma Correction

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
