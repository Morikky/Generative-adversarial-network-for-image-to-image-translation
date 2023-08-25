# Generative-adversarial-network-for-image-to-image-translation

The translation is from real images into segmented images that use color encoding to separate different objects in the real images (like: cars, buildings, trees, etc.). A Generative Adversarial Network (GAN) model is trained to perform this image-to-image translation.

![img1](https://github.com/Morikky/Generative-adversarial-network-for-image-to-image-translation/blob/main/Plots/Example_img_from_dataset.png)

The real and segmented images (also called source and target images) are from the Cityscapes dataset (https://www.cityscapes-dataset.com/). The dataset is not shared here. 300 pairs of images with resolution of 256x256 are used for training. Additional images from the validation dataset are used to evaluate the model. 

The code presented here is based on the source https://machinelearningmastery.com/how-to-develop-a-pix2pix-gan-for-image-to-image-translation/ . 

It is a typical GAN implementation that includes a model for each of the so-called generator and discriminator. The two are then combined in a higher model that is called the GAN model. This combined model allows the generator to get better in producing "fake" data that resembles the real data (from the training set), while the discriminator learns to distinguish between real and fake data. 

Typically, the generator creates fake data out of random data (like in the known MNIST digits dataset) and the discriminator is input with real and fake data that are labeled. Here, however, since it is an image-to-image translation task, the source and target images are input to the generator, and it produces a fake target image out of the source image. The fake target image is improved by a comparison to the real target image (comparison in the sense of L1 loss minimization). The discriminator, in the image-to-image case, takes a source image and a real and a fake target image as inputs, and learns to classify a target image as real/fake for a given source image. The discriminator learns through the minimization of a binary cross entropy loss term. 

Worth mentioning that the discriminator is trained on its own, and the generator is trained as part of the combined GAN model. Moreover, in the GAN model, a weighting of the two loss contributions in favor of the L1 loss over the binary cross entropy loss (100-fold) is added in order for the model to produce fake target images that resembles the real target images and not just fake target images that would pass the discriminator.

Additionally, the generator model is based on the U-Net architecture; a convolution neural network (CNN) that gradually reduces the size of the array, which is the output of applying the convolution filters, and then increases it back to the original image size. These changes in the array size are done through the convolution filters that their parameters are the ones that are trained. The discriminator model also uses CNN to reduces the original image size down to a final patch of size 16x16 (the source mentions a patch size of 70x70, however here a patch size of 16x16 is seen). The loss function of the discriminator is set with a weight of 0.5 to slow down its training relative to the training rate of the generator. This, reported by the source, have a positive effect on the training. 

To show the results of training the model, 3 instances are randomly selected from the validation dataset (each in one row of the image below). The source and target images are shown in the extreme left and extreme right, respectively, and between them the model-generated target images after 10, 40 and 70 training epochs are shown. This presents the evolution of the model performance along the training. 

![image grid](https://github.com/Morikky/Generative-adversarial-network-for-image-to-image-translation/blob/main/Plots/image_grid.png)

For the first image, capturing of the sidewalk (in pink on the left) and of people (red in the center) is seen to improve as the training advances. On the negative side, capturing of poles (in gray) and traffic signs (in yellow) is unsuccessful, that is after 70 training iterations. 

In the second image, cars, buildings and trees are segmented clearly. The coloring of the road (in purple) after 10 epochs is a bit noisy, but this noise vanishes with a higher number of epochs. As in the first image, traffic sign capturing is unsuccessful. However, the poles on the right side might start to appear after the maximal number of epochs used here. Moreover, bright buildings in the back of the image might have been mistaken for skies, as they were colored  in light blue. 

In the third image, the separation between the overlapping car (in dark blue) and sidewalk (in pink) on the left is clearly seen to improve as the model is further trained.  




