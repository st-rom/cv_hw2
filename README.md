#### cv_hw2

# HomeWork 2 - Computer Vision


## Task 1. Multiclass Semantic Segmentation

### Problem description
We have a number of images of people, and we have to distinguish them from everything else in the picture. This can be hard since sometimes the clothes on the person can be of the same color as background. By applying semantic segmentation using ResNet34 we can get pretty good results. 

### Challenges
Our model has to learn to find edges of objects by applying different layers like convolution and pooling and by doing so the size of the initial image decreases. But since we have to classify pixels of the initial image we should do one of the upsampling techniques to get back to initial resolution. In ResNet  (ResNet is a combination of U-Net and LinkNet) this is done by deconvolution (transposed convolution). This way we can achieve binary classification of all pixels.

### Model description
We do the reduction of dimensionality 5 times to extract the most important features for us. Five is an optimal number which in our case works great, because less than five would is used for simpler images, and more than five would be too much. Each time we apply convolutions2D and sigmoid activation since this is binary segmentation problem. After that we do upsampling 5 times. Each time we use transposed convolutions to get the most precise results in the end.

### Training/validation strategy
We apply horizontal flip augmentation to our train dataset. After shuffling the data we train our model on it with batch size equal to 4. We use every tenth picture(10%) as part of our validation set. Augmentation isn't applied to it. Also we use Adam optimizer and Binary Cross Entropy with logits as our loss function. Also we use Dice and Jaccard loss which will give us a lot better understanding of how accurate our model is.

### Experiment log
Firstly, I tried other models like UNet16 and UNet11 but ResNet32 was the best in this case.<br>
I didn't use any other types of augmentation on our images since they would make our model worse by learning it on features that aren't usual(in transposed or vertically flipped images) also scaling isn't an option because it can crop important parts of an image.<br>
With lesser learning rate our epochs training time is bigger and the result isn't better and when I took bigger learning rate dice and jaccard loss gave pretty bad results, so the optimal learning rate in my case was equal to 0.001. <br>
I also experimented with batch sizes, other loss functions, decoder blocks etc.

### Discussion (what worked, what didn't work, how to improve)
After trying different parameters on our model which I already mentioned in Experiment log, I chose the ones that left in the file. Due to the problem that training takes a lot of time and computational power, the model isn't the best it could be, but at least I know in which direction I should be moving further to improve it(experimenting more with model layers, trying more hyperparameters values, etc).
![alt text](https://github.com/st-rom/cv_hw2/blob/master/imgs/poker.jpg)
Після 20 епох: val_loss: 0.123, val_dice: 0.938, val_jaccard: 0.891


## Task 2. Image Matting
