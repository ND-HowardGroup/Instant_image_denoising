# Author: Varun Mannam, The Department of Electrical Engineering, The University of Notre Dame, South Bend, Indiana (IN), USA. Zip: 46556, Email: vmannam@nd.edu

# High-level description of the plugin
This is the plugin used to denoise any fluorescence microscopy image that contains mixed Poisson-Gaussian noise from the fluorescence microscopy (Wide-field, Confocal and two-photon microscopy). This algorithm is developed by training the noisy microscopic images using the FMD dataset with the convolutional neural networks using the Noise2Noise/DnCNN architectures.

# Steps to get a denoised image:
1a. Open Fiji/ImageJ
1b. ImageJ -> Edit -> Options -> Tensorflow -> Choose the Tensorflow TF version based on the user system requirements (like: CPU or GPU with proper CUDA drivers)
2. Select an image in ImageJ (use open image function: File ->open)
3. Run the image-denoising plugin (Plugins -> Noise2Noise denoising or Plugins -> DnCNN denoising) then denoised image will pop-up with the proper title.  (by default denoised image is 32-bit float type and use ImageJ to combine the color images or other functions) and convert to the 8-bit/16-bit images and use the auto-scale function.
4. Use the console to check for the test time.

# Implementation details of the plugins: 
In addition to the existing functions of ImageJ-tensorflow library provided by Rueden et.at, we have added the pre-processing (normalization) and post-processing (restoring original scaling) steps and support our plugin for any datatype. For example, our ImageJ plugin works for 8-bit depth data to 16-bit and 32-bit depth images and performs the image denoising using the pre-trained ML models and provides the result in 32-bit format. Additionally, the neural network expects the input image to be normalized (to a range between -0.5 to 0.5) which plug-in performs prior to model inference. Conversely, the denoised inference is subsequently rescaled by the same offset and scaling factor to produce the final output image.

# Current Limitations:
1. Plugin is  limiteed in the number of images at a time to denoise (limitation on the TensorFlow and computer GPU memory)
2. Noise2Noise uses the max-pool layer, so if the image size is not multiple of 32x32, image will be adjusted using linear interpolation (however, the padding is the prefered method which is under current development) to the nearest multiple of 32x32 and perform the denoising and finally restores back to the original image dimensions. 

##### Note #####
Noise2Noise ML model uses 5 max-pooling layers of kernel size 2x2 (in total it is 32x32), so if the image is not evenly divisible by 32, interpolation is performed. Alternatively, padding can be used for (most likely) further performance improvements. That feature is in development. However, since DnCNN does not have any max-pooling layers, resizing is not performed and therefore not an issue when using that model.”

##### End of Note #####

3. Speed is better in presence of GPU machine compared to the CPU version.
4. 4D images support is not added yet this stage but can use hyper-stack to stack conversion and perform the image denoising on the 3D stacks.
5. GPU common errors are linking the CUDA drivers using symbolic names.

## **Copyright**

© 2019 Varun Mannam, University of Notre Dame  
