# Introduction
In this project, we explore the advancements in Generative Adversarial Networks (GANs) for high-fidelity image editing by re-implementing the paper titled \textit{Image2StyleGAN++: How to Edit the Embedded Images?}. The authors, Rameen Abdal, Yipeng Qin, and Peter Wonka, expand upon their previous work, Image2StyleGAN, by introducing several significant enhancements. It was presented at CVPR 2020.

The core method discussed in the paper involves embedding images into the latent space of a pre-trained StyleGAN. This is achieved through an embedding algorithm that not only optimizes the latent space but also manipulates the noise and activation tensors to produce localized and high-fidelity edits on images.

The main contributions of the paper are threefold: Firstly, the introduction of noise optimization to enhance the reconstruction of high-frequency features in images, significantly improving the quality of the output. Secondly, the extension of the global W+ latent space embedding allows for local modifications and more controlled edits. Thirdly, the combination of latent space embedding with direct manipulation of activation tensors enables both global and local edits, providing a powerful framework for various image editing applications such as image inpainting, style transfer, and feature modification.

# Chosen Result
We aimed to reproduce two specific results the first is the merging of two halves of an image as seen below. The significance of this result is that it is equally as good as copying and pasting the two halves of the image and producing a high-quality image with no splits.  This  could reduce the human work and shows that deep learning networks can be do just as much as manually cropping
<img width="1225" alt="Screen Shot 2024-05-16 at 10 36 34 PM" src="https://github.com/jacksoncamp42/Image2StyleGANPlusPlus-Re-Implementation/assets/37753577/994903a5-386d-4bf6-8ff2-11d538fb0c24">
The second which is making the PSNR ratio up from around 19 to 22 to 39 to 45 on recreated images.  The significance is that we can recreate the image with a higher quality than before as it has a higher precision-to-noise ratio.
<img width="409" alt="Screen Shot 2024-05-16 at 10 37 15 PM" src="https://github.com/jacksoncamp42/Image2StyleGANPlusPlus-Re-Implementation/assets/37753577/2b9726e7-bf90-4a96-a54b-5b100df04ea3">

# Reimplementation Details
During this re-implementation, we started from the image2styleGAN paper and the pre-trained model, which we loaded via a pickle file. The main change in the model architecture was via the loss function and adding the noise optimization. When changing the loss function we changed it by adding multiple masks on the four different loss compared to the MSE loss and the perceptual loss in the original paper. In this loss function, we added two new parts and added masks primarily so that there could be style transfer from two images. We added the loss called the paper as the style loss which was taken from the third layer of the VGG16 pre-trained ImageNet model and the image not used in the style loss. The next thing we had to implement was masks which was a convolutional filter that specified which part of the image we took from image one and which part of the image one we took from image two. There had to be three separate filters because they were different parts of the loss and had different losses. However, overall they are supposed to roughly represent the same portion of our image. While doing this for the perceptual loss it was tough just because there were parts from different sizes so we couldn't use the same matrix. As a result we had a few matrices representing the same part of the image. These matrices would be hyperparameters to the loss functions as well as the images. In addition to blend two image, such as the image with a scribble result we would put in some type of blur matrix. Some of the shapes in the image were complicated to make as filters so we took the one that combined two images vertically.

For running our code we did it on a Jupiter notebook so one could simply run the Jupiter notebook. We used pytorch, numpy and pickle to load the pretrained styleGan network

We were able to reproduce results by running a V100 GPU for 20-30 minutes

# Results and Analysis

<img width="924" alt="Screen Shot 2024-05-16 at 10 39 10 PM" src="https://github.com/jacksoncamp42/Image2StyleGANPlusPlus-Re-Implementation/assets/37753577/46e27815-9b9f-4483-ab26-61ef3aea934b">
<img width="1225" alt="Screen Shot 2024-05-16 at 10 36 34 PM" src="https://github.com/jacksoncamp42/Image2StyleGANPlusPlus-Re-Implementation/assets/37753577/89f84c11-58fc-433b-8782-bd06db494aa1">
Clearly the results of the combined image are very similar to the results as it looks the same as the two images halves combined but not the actual image.  That is because the dataset of images was very large and finding the right image was like finding a needle in a haystack.  As to avoid combing through the actual dataset with millions of images we chose two random images and used the same filters.  As we were able to reimplement this this shows that this is able to be reimplemented and that the paper contributed very well.  I am not as good in Photoshop so I recreated the pictures combined manually as best I could there was a little bit of whitespace but for the purposes of comparing each half with the half of the recreated image I believe it shows that it does a decent job. 
<img width="1110" alt="Noise Optimization Results 2" src="https://github.com/jacksoncamp42/Image2StyleGANPlusPlus-Re-Implementation/assets/37753577/33096eea-c4ee-4b9f-a051-40a129c9c3f9">
<img width="586" alt="Noise Optimization Results" src="https://github.com/jacksoncamp42/Image2StyleGANPlusPlus-Re-Implementation/assets/37753577/4507241c-0fac-48b4-86be-bd02bb77aac5">
<img alt="PSNR graph (optimizng embedding function noise + latent space)" src="https://github.com/jacksoncamp42/Image2StyleGANPlusPlus-Re-Implementation/assets/37753577/c96fdddf-8200-4c26-9a66-53c93234437b">
<img width="586" alt="dave" src="https://github.com/jacksoncamp42/Image2StyleGANPlusPlus-Re-Implementation/assets/37753577/28eed2da-7163-415f-935a-14ea78e66244">
<img width="586" alt="psnr-dave" src="https://github.com/jacksoncamp42/Image2StyleGANPlusPlus-Re-Implementation/assets/37753577/62967d03-a577-40d8-9a13-1a010d9d0331">



# Conclusion and Future Work
I think the that one key takeaway from our re-implementation effort was that using pre-trained models and improving upon them can be really powerful and provide innovative results.  While we didn’t have to train an entirely new style and just fine-tuned on two images, it produced incredible results.   
Another lesson that we learned is just how confusing and the amount of detail that you have to include several details to make one's results reproducible.  The paper left out several details that they would have used especially in terms of hyperparameters apart from masks that were all ones or all zeros, and what exact mask matrices they use to produce a result.  Even when they provided an image of it, it seemed very hard to reproduce the result due to the odd shape and converting that roughly into a matrix of 1 and 0 of which one to take from each image.  In other times they didn’t give us other hyperparameters and how this picture or any arbitrary picture would translate to other masks.  This made reimplementing the results a hassle and required a lot of guessing.  I think that some future directions would be seeing how changing the mask would affect the image and which masks we can use to garner different types of resutsl  

# References
https://arxiv.org/abs/1904.03189
https://arxiv.org/abs/1911.11544
https://oscar-guarnizo.medium.com/review-image2stylegan-embedding-an-image-into-stylegan-c7989e345271
