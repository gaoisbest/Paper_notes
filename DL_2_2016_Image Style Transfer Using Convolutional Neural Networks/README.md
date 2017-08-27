# Introduction
## Basic
**Title**: [Image Style Transfer Using Convolutional Neural Networks](http://101.96.10.63/www.cv-foundation.org/openaccess/content_cvpr_2016/papers/Gatys_Image_Style_Transfer_CVPR_2016_paper.pdf)

**Author**: Leon A. Gatys, Alexander S. Ecker and Matthias Bethge

**Publish information**: 2016, *CVPR*

**Arxiv version:** [A Neural Algorithm of Artistic Style
](https://arxiv.org/pdf/1508.06576.pdf)

## Application scenario
Initial a white noise image *W*, the goal is transfering the style of image *A* to *W* and transfering the content of image *C* to *W*. Therefore, *W* has the style of *A* and the content of *C*.

# Model
### Total cost
![] (https://github.com/gaoisbest/Paper_notes/blob/master/DL_2_2016_Image%20Style%20Transfer%20Using%20Convolutional%20Neural%20Networks/Formula_7_total%20cost.png)

### Content cost
![](https://github.com/gaoisbest/Paper_notes/blob/master/DL_2_2016_Image%20Style%20Transfer%20Using%20Convolutional%20Neural%20Networks/Formula_1_content%20cost.png)

#### Derivative of content cost 
![ ] (https://github.com/gaoisbest/Paper_notes/blob/master/DL_2_2016_Image%20Style%20Transfer%20Using%20Convolutional%20Neural%20Networks/Formula_2_conent%20derivative.png)

### Style cost
![ ](https://github.com/gaoisbest/Paper_notes/blob/master/DL_2_2016_Image%20Style%20Transfer%20Using%20Convolutional%20Neural%20Networks/Formula_6_style%20cost%20derivative.png)
#### Style cost of each layer
![ ](https://github.com/gaoisbest/Paper_notes/blob/master/DL_2_2016_Image%20Style%20Transfer%20Using%20Convolutional%20Neural%20Networks/Formula_4_style%20cost%20of%20each%20layer.png)

#### Gram matrix
![ ](https://github.com/gaoisbest/Paper_notes/blob/master/DL_2_2016_Image%20Style%20Transfer%20Using%20Convolutional%20Neural%20Networks/Formula_3_Gram%20matrix.png)
#### Derivative of style cost
![ ]()

# My comments
The key idea is the correlation bewteen pairs of feature maps in each layer (i.e., Gram matrix), since the feature maps of each layer in traditional CNN are independent.

# Application
There is a [Tensorflow](https://github.com/anishathalye/neural-style)  implementation
