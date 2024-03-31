### The purpose of this model is to classify whether the image of a human's face has a filter or not.

We ended up building multiple binary classifiers for a variety of filters that will hopefully be able to generalize across all filters

# Abstract

We achieved 83% accuracy on a binary classification problem of a test set including 54 data points of unfiltered and filtered images, with various iPhone camera filters applied. The final binary classification models for the ensemble model was trained on a dataset of a total of 173,488 training images and 21,908 validation images, gathered from the FairFace dataset and applying iPhone filters.

# Use Case

Download the checkpoints from this the FINAL Checkpoints folder in this __drive:https://drive.google.com/drive/folders/1av6IIJIUVnEDiEEZSur6huc_NYrP8xKx?usp=drive_link__ and move them to the **checkpoints** folder.

We test and create the final ensemble model and isntructions to use it are in the **src** folder in the file **FINAL_camera_ENSEMBLE_Binary.ipynb**.


# Dataset

For our final model, we used the Fair Face dataset consisting of ~87k training images and ~11k validation images. These served as our unfiltered images. Throughout the project, we experimented with many filters but ultimately ended up using these 8 filters:

 - exposure
 - cool
 - cool_hue
 - warmth
 - black_and_white
 - blur
 - contrast
 - sepia

These filters were applied to every unfiltered image (Fair Face dataset) to build our filtered images. Our dataset folder ended up looking like this:

 - dataset
  - unfiltered
     - train
     - val
   
  - camera_filtered
     - exposure
        - train
        - val
     - ...

  - filtered [this was for the instagram filters used in past trials]

Our methods of applying the filters and the past filters and methods can be found in **dataset_filter_applier.ipynb**.

# Training

Training files can be found in the **experiments** folder.

We trained a binary classifier for each filter in **camera_CNN_Binary.ipynb**. Based on previous trials, we found that simple CNN or simple two layer CNNs worked best, so these were the two models used. Converged very quickly, within 2 epochs generally got 80+% or 90+%.

# Past Trials

Past training files can also be found in the **experiments** folder.

We first started with a multiclass CNN for 6 camera filters and our dataset was a class directory of photos of men, which was around 4k in size. This was not very helpful. 

Then moved to the Fair Face dataset and used the instagram filters dataset that already existed and was made by someone else. This was a balanced dataset of around of 87k filtered images (10k per filter) and 87k unfiltered images. We still used a multiclass CNN for this, experimenting with different models. We then tried adding additional 6-8 camera fitlers expanding the classes to 14-17, and the total training images to around 450k - 600k. Most of the model experimenting was done here, and this can be found in _**camera_and_instagram_CNN_Multiclass.ipynb**_ and **instgaram_filters_CNN_Multiclass.ipynb**. We also tried a binary classifier with all of these filters used as the filter images, which can be found in **camera_and_instagram_CNN_Binary.ipynb**. The **image_classifier_instructions.ipynb** shows use cases for the multiclass model, which did poorly on test sets, so the checkpoints were not uploaded. Details can be found in those files, especially in the _**camera_and_instagram_CNN_Multiclass.ipynb**_.

# More Information

We built this model to detect images that we cannot work with. Thus, it is more important to not detect false positives and let small to mild filters go than to try and detect every filter at the risk of detecting images with no filter and thus impacting the user experience.

 - exposure
   - essentially brightness
 - cool
   - large variation of decrease in saturation, very mild variation of cold (g/b) tint applied
 - cool_hue
   - slight variation of decrease in saturation, variations of cold tints applied (r/g/b, but more b)
 - warmth
   - mild variations of warm tints applied (r/g)
 - black_and_white
   - black and white filter applied with slight variations towards the extremes
 - blur
   - blur radius from 2 to 6, so tracks more extreme blurs. Can change the cutoff if you want to detect less blurry images
 - contrast
   - typical contrast, also accounts for variations of increase in saturation
 - sepia
   - this one was very specific and tended to overfit. Most of the time, contrast or warmth was able to capture images with sepia like filters
