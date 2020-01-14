# Live Tumour Segmentation with Magnetic Resonance (MR) Image

## Programming Environment
- Python 3.73
- Jupyter Notebook
- AWS EC2
  - Deep Learning AMI (Ubuntu 16.04) Version 26.0 
  - p2.xlarge


## Abstract
Convolutional Neural Network (CNN) is generally used in image recognition. For image segmentation, several approaches, which are CNN models with extra process, have been developed in recent years. This paper introduces three major algorithms for image segmentation: FCN, SegNet, and U-Net, and evaluates segmentation methods of liver tumor with magnetic resonance (MR) images using them.

## 1. Introduction
Accurate labelling of regions of interest, known as semantic segmentation, is highly useful in many fields such as autonomous vehicles, geo sensing and bio medical image diagnosis. In this project, we adopt three different segmentation algorithms which are U-Net, SegNet, and Fully Convolutional Network (FCN), and investigate the performance to an in-house MR image dataset consisting of 40 patients, in particular the arterial phase, to address the missing annotation challenge. These algorithms are based on convolutional neural network, and the difference lies in the final layers. In convolutional neural network, the last few layers are full connected layers and produces a one-dimensional vector for classification (patch classification). On the other hand, U- Net, SegNet, and FCN has an extra decoding process corresponding to the encoding process, which is accomplished by upsampling, hence producing a two-dimensional image that preserves the location information.

## 2. Method
The task required in segmenting tumour from magnetic resonance image is to label each pixel of an image with a corresponding class, which is commonly referred to dense prediction. The difference between dense prediction and the previous tasks accomplished with simply convolutional neural network is that the expected output in semantic segmentation is a high-resolution image with corresponding label for each pixel, rather than a one-dimensional vector.
In a typical convolutional network, the size of image decreases due to pooling which helps the filters in the deeper layer to focus on a larger receptive field. Intuitively speaking, by down sampling the model can make a good prediction on “what” is in the image but loses information on “where” it is in the image.
This is where fully convolutional network comes in (2014), which is one of the prominent pioneers in deep learning architecture. It does dense prediction without any full connected layers. This allows segmentation maps to be generated for image of any size and much faster. Segnet, u-net and many other similar segmentation models based upon it. Simply put, it introduces up sampling mechanism to convert a low-resolution image to high resolution image to recover the “where” information. Transposed convolution (also known as deconvolution) is probably the most preferred technique to perform up sampling, which learns parameters through back propagation.
U-Net consists of two paths as well. First path is the contraction path (encoder) is
simply convolutional neural network with convolutional and max pooling layers. The second path is the symmetric expanding path (decoder) to enable precise localization using transposed convolutions, which is performing concatenation on image with the same dimensions originally and after deconvolution.
SegNet adopts pooling indices. This is because location information is loss at pooling layer, so saving pooling indices can help deconvolution layer to recover location information more easily. This makes SegNet more memory efficient than FCN.

## 3. Experiments and Conclusion
As it is explained above, three segmentation approaches, which are FCN, SegNet, and U-Net, are built and trained with MR images of the artery phase from 40 patients. As a loss function, dice coefficient is used.
In this experiment, the image data are normalized when converted a NumPy array and used as an original image size as inputs and outputs which is 320×260 pixel. The time to complete each epoch among the models is totally different. For example, FCN does not take much time to train one epoch, but it takes a huge number of epochs to improve the accuracy on the training set. The dice coefficients on the training set by FCN, SegNet, and U-Net were 10, 0.9720, and 0.8888 respectively and on the test set, 10, 0.8180, and 0.7265.
As it is expected, since SegNet and U-Net is based on FCN and improved, they have higher efficiency. SegNet and U-Net get better results than FCN. The structure of SegNet and U-Net is similar. The main differences are when it decodes, SegNet decode with uppooling but U-Net concatenates corresponding layers, and SegNet has Batch Normalization after every convolution layer. Batch Normalization allows to use higher learning rates and be less careful about initialization. Hence it might be the reason of approximate 0.1 gap of dice coefficient on both of training and test set between SegNet and U-Net. SegNet and U-Net sufficiently detected where the tumour is. However, in this experiment, we use data from only 40 patients, which contains 720 images having a liver tumour in total. It is few training datasets for deep learning, therefore for further improvement, an approach such as adding some noise or making some transformations would be needed to increase the number of training set.
