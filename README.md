# Generative-adversarial-network-for-image-to-image-translation

The translation is from real images into segmented images that use color encoding to separate different objects in the real images (like: cars, buildings, trees, etc.).

![img1](https://github.com/Morikky/Generative-adversarial-network-for-image-to-image-translation/blob/main/Plots/Example_img_from_dataset.png)

The real and segmented images (also called source and target images) are from the Cityscapes dataset (https://www.cityscapes-dataset.com/). The dataset is not shared here. 300 pairs of images with resolution of 256x256 are used for training. Additional images from the validation dataset are used to evaluate the model. 

The code presented here is based on the source https://machinelearningmastery.com/how-to-develop-a-pix2pix-gan-for-image-to-image-translation/ . 

It is a typical GAN implementation that includes a model for each of the so-called generator and discriminator. The two are then combined in a higher model that is called the GAN model. This combined model allows the generator to get better in producing data that resembles the real data, while the discriminator learns to distinguish between the real and fake data. 

Typically, the generator creates fake data out of random data (like in the known MNIST digits dataset) and the discriminator is input with real and fake data that are labeled. Here, however, since it is an image-to-image translation task, the source and target images are input to the generator, and it produces a fake target image out of the real source image. The fake target image is improved by a comaprison to the real target image (comparison in the sense of L1 loss minimization). The discriminator, in the image-to-image case, takes a source image and a real and a fake target image as inputs, and learns to classify a target image as real/fake for a given source image. The discriminator learns through the minimization of a binary cross entropy loss term. 

Worth mentioning that the discriminator is trained on its own, and the generator is trained as part of the GAN model. Moreover, in the GAN model, a weighting of the two loss contributions in favor of the L1 loss over the binary cross entropy loss (100-fold) is added in order for the model to produce fake target images that resembles the real target images and not just fake target images that would pass the discriminator.

Additionally, the generator model is based on the U-Net architecture; a convolution neural network (CNN) that gradually reduces the size of the array, which is the output of the convolution filters, and then increases it back to the original image size. These changes in the array size are done through the convolution filters that their parameters are the ones that are trained. The discriminator model also uses CNN to reduces the original image size down to a final patch of size 16x16 (the source mentions a patch size of 70x70, however here a patch size of 16x16 is seen). The loss function of the discriminator is set with a weight of 0.5 to slow down its training relative to the training rate of the generator. This, reported by the source, have a positive effect on the training. 

To show the results, three instances are randomly selected from the validation dataset. The source and target images of each instance are shown in one row (in the extreme left and extreme right, respectively) and between them the model-generated target images after 10, 40 and 70 training epochs are shown. This presents the evolution of the model performance along the training. 

![image grid](https://github.com/Morikky/Generative-adversarial-network-for-image-to-image-translation/blob/main/Plots/image_grid.png)

first image:  negative: capturing of poles is unsuccessfull even after the maximal number of traing iterations. Positive: successfuully captures the road and sidwalks.  Overall, a difficult image beacuse of the many overlapping objects im the image center (padestrains in the vorground of street signs, etc.)

second image, negaitive:  coloring of the road (in purple) and of the buliding on the right is noisy. . But, the noise level is reduced as training advances. Positive: capturing of vegetation is succesfull with only 10 epochs.  Moreover the blue buliding might have been mistaken as the sky (and colored in blue).

third image, positive: with more the epochs sepeartion of overlapping objects (car and pedestrian) is seen to be imporved. Negative: The capturing of sidewalks (pink surface in background) is unscessfull.



The typical use of a validation dataset is not a part of the model training, since the GAN model does not converge.
