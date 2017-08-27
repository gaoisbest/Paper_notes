# Introduction
## Basic
**Title**: [Image Style Transfer Using Convolutional Neural Networks](http://101.96.10.63/www.cv-foundation.org/openaccess/content_cvpr_2016/papers/Gatys_Image_Style_Transfer_CVPR_2016_paper.pdf)

**Author**: Leon A. Gatys, Alexander S. Ecker and Matthias Bethge

**Publish information**: 2016, *CVPR*

**Arxiv version:** [A Neural Algorithm of Artistic Style
](https://arxiv.org/pdf/1508.06576.pdf)

## Application scenario
Initialize a white noise image *W*, the goal is transfering the style of image *A* to *W* and transfering the content of image *C* to *W*. Therefore, *W* has the style of *A* and the content of *C*.

# Model
The paper uses the weights of pre-trained VGG19 network (16 convolutional layers + 3 fullly-connected layers) to perform feed-forword convolutional operation. Then updating the parameters using gradient descent.

```
# The following codes are from https://github.com/anishathalye/neural-style
CONTENT_LAYERS = ('relu4_2', 'relu5_2')
STYLE_LAYERS = ('relu1_1', 'relu2_1', 'relu3_1', 'relu4_1', 'relu5_1')
```

The **content cost** is sum of content loss for layers in `CONTENT_LAYERS`. The **style cost** is sum of style loss for layers in `STYLE_LAYERS`. The **total loss** is sum of **content cost** and **style cost**. 

### Content cost
![](https://github.com/gaoisbest/Paper_notes/blob/master/DL_2_2016_Image%20Style%20Transfer%20Using%20Convolutional%20Neural%20Networks/Formula_1_content%20cost.png)

```
# content loss
content_layers_weights = {}
content_layers_weights['relu4_2'] = content_weight_blend
content_layers_weights['relu5_2'] = 1.0 - content_weight_blend

content_loss = 0
content_losses = []
for content_layer in CONTENT_LAYERS:
    content_losses.append(content_layers_weights[content_layer] * content_weight * (2 * tf.nn.l2_loss(
            net[content_layer] - content_features[content_layer]) /
            content_features[content_layer].size))
content_loss += reduce(tf.add, content_losses)
```

#### Derivative of content cost 
![](https://github.com/gaoisbest/Paper_notes/blob/master/DL_2_2016_Image%20Style%20Transfer%20Using%20Convolutional%20Neural%20Networks/Formula_2_conent%20derivative.png)

### Style cost
![](https://github.com/gaoisbest/Paper_notes/blob/master/DL_2_2016_Image%20Style%20Transfer%20Using%20Convolutional%20Neural%20Networks/Formula_6_style%20cost%20derivative.png)

```
# style loss
style_loss = 0
for i in range(len(styles)):
    style_losses = []
    for style_layer in STYLE_LAYERS:
        layer = net[style_layer]
        _, height, width, number = map(lambda i: i.value, layer.get_shape())
        size = height * width * number
        feats = tf.reshape(layer, (-1, number))
        gram = tf.matmul(tf.transpose(feats), feats) / size
        style_gram = style_features[i][style_layer]
        style_losses.append(style_layers_weights[style_layer] * 2 * tf.nn.l2_loss(gram - style_gram) / style_gram.size)
    style_loss += style_weight * style_blend_weights[i] * reduce(tf.add, style_losses)
```

#### Style cost of each layer
![](https://github.com/gaoisbest/Paper_notes/blob/master/DL_2_2016_Image%20Style%20Transfer%20Using%20Convolutional%20Neural%20Networks/Formula_4_style%20cost%20of%20each%20layer.png)

#### Gram matrix
![](https://github.com/gaoisbest/Paper_notes/blob/master/DL_2_2016_Image%20Style%20Transfer%20Using%20Convolutional%20Neural%20Networks/Formula_3_Gram%20matrix.png)
#### Derivative of style cost
![](https://github.com/gaoisbest/Paper_notes/blob/master/DL_2_2016_Image%20Style%20Transfer%20Using%20Convolutional%20Neural%20Networks/Formula_6_style%20cost%20derivative.png)

### Total cost
![](https://github.com/gaoisbest/Paper_notes/blob/master/DL_2_2016_Image%20Style%20Transfer%20Using%20Convolutional%20Neural%20Networks/Formula_7_total%20cost.png)
The [Tensorflow](https://github.com/anishathalye/neural-style) implementation also includes *total variation denoising*, may be used for avoiding overfitting.

```
# total variation denoising
tv_y_size = _tensor_size(image[:,1:,:,:])
tv_x_size = _tensor_size(image[:,:,1:,:])
tv_loss = tv_weight * 2 * (
        (tf.nn.l2_loss(image[:,1:,:,:] - image[:,:shape[1]-1,:,:]) /
            tv_y_size) +
        (tf.nn.l2_loss(image[:,:,1:,:] - image[:,:,:shape[2]-1,:]) /
            tv_x_size))
# overall loss
loss = content_loss + style_loss + tv_loss
```

# My comments
The key idea is the correlation bewteen pairs of feature maps in each layer (i.e., Gram matrix), since the feature maps of each layer in traditional CNN are independent.

# Application
There is a [Tensorflow](https://github.com/anishathalye/neural-style) implementation. The codes showed in **Model** section are from this implementation.

