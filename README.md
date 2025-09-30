# Gen-AnimalClassification
Classification of an [animal dataset](https://www.kaggle.com/datasets/miguelxp/animalprediction) for a Kaggle competition


## Models tried

### Big model
I created a big model after trying smaller models before. This was the first to deliver good results.
| Layer (type)              | Output Shape         | Param #     |
|--------------------------|----------------------|-------------|
| InputLayer               | (None, 128, 128, 3)  | 0           |
| Conv2D                   | (None, 126, 126, 32) | 896         |
| Conv2D_1                 | (None, 124, 124, 64) | 18,496      |
| BatchNormalization       | (None, 124, 124, 64) | 256         |
| MaxPooling2D             | (None, 62, 62, 64)   | 0           |
| Conv2D_2                 | (None, 60, 60, 128)  | 73,856      |
| Conv2D_3                 | (None, 58, 58, 256)  | 295,168     |
| BatchNormalization_1     | (None, 58, 58, 256)  | 1,024       |
| MaxPooling2D_1           | (None, 29, 29, 256)  | 0           |
| Conv2D_4                 | (None, 27, 27, 256)  | 590,080     |
| Conv2D_5                 | (None, 25, 25, 512)  | 1,180,160   |
| BatchNormalization_2     | (None, 25, 25, 512)  | 2,048       |
| MaxPooling2D_2           | (None, 12, 12, 512)  | 0           |
| Conv2D_6                 | (None, 10, 10, 512)  | 2,359,808   |
| Conv2D_7                 | (None, 8, 8, 1024)   | 4,719,616   |
| BatchNormalization_3     | (None, 8, 8, 1024)   | 4,096       |
| MaxPooling2D_3           | (None, 4, 4, 1024)   | 0           |
| Flatten                  | (None, 16384)        | 0           |
| Dense                    | (None, 512)          | 8,389,120   |
| Dense_1                  | (None, 256)          | 131,328     |
| Dropout                  | (None, 256)          | 0           |
| Dense_2                  | (None, 64)           | 16,448      |
| Dense_3                  | (None, 10)           | 650         |
Total params: 17,783,050 (67.84 MB)
#### Metrics
| Epoch | Train Accuracy | Validation Accuracy | Train Loss | Validation Loss |
|-------|----------------|---------------------|------------|------------------|
| 10    | 0.4297         | 0.1825              | 1.6925       | 2.2247             |
| 30    | 0.5859         | 0.5233              | 1.2179       | 1.4879             |
| 50    | 0.6875         | 0.6773              | 0.9265       | 1.1082             |
| 70    | 0.7812         | 0.7505              | 0.6469       | 0.7938             |
| 90    | 0.9297         | 0.8030              | 0.2579       | 0.6533             |
| 110   | 0.8984         | 0.8193              | 0.4315       | 0.6422             |

### Oversampling
This model is the same as the one before, but taking into account the number of images of each class.

Unluckily it didn´t got any real gain.

### Transfer learning
After only achieving a 84% accuracy in test, we tried aplying transfer learning, using VGG and the Imagenet weights, and freezing most of their layers, excluding the last convolutional block.

The learning rate was much smaller (1e-4), for it to work properly.

| Layer (type)              | Output Shape         | Param #     |
|--------------------------|----------------------|-------------|
| vgg16 (Functional)       | (None, 7, 7, 512)     | 14,714,688  |
| Flatten                  | (None, 25088)         | 0           |
| Dense                    | (None, 512)           | 12,845,568  |
| Dense_1                  | (None, 256)           | 131,328     |
| Dropout                  | (None, 256)           | 0           |
| Dense_2                  | (None, 128)           | 32,896      |
| Dense_3                  | (None, 10)            | 1,290       |
Total params: 27,725,770 (105.77 MB)
#### Metrics
| Epoch | Train Accuracy | Validation Accuracy | Train Loss | Validation Loss |
|-------|----------------|---------------------|------------|------------------|
| 10    | 0.9457         | 0.9178              | 0.1566     | 0.2671           |
| 20    | 0.9660         | 0.9433              | 0.1017     | 0.1941           |
