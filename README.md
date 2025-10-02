# Gen-AnimalClassification
Classification of an [animal dataset](https://www.kaggle.com/datasets/miguelxp/animalprediction) for a Kaggle competition.

This is the [best model](https://huggingface.co/MiguelGP-13/AnimalClassifier)

## Analysis
We had to classify an animal dataset with 10 classes. The first step was to analyze the number of classes, the number of samples in each class, and their shapes.

Interestingly, the image shapes were variable, which could complicate the process.

We started by creating a model from scratch, but due to the complexity of the problem, it had to be large and required many epochs to learn.

Then we tried transfer learning with VGG, which delivered better results thanks to its pretrained convolutional layers.

## Models Tried

### Big Model
We created a large model after experimenting with smaller ones. This was the first to deliver good results.

| Layer (type)              | Output Shape         | Param #     |
|---------------------------|----------------------|-------------|
| InputLayer                | (None, 128, 128, 3)  | 0           |
| Conv2D                    | (None, 126, 126, 32) | 896         |
| Conv2D_1                  | (None, 124, 124, 64) | 18,496      |
| BatchNormalization        | (None, 124, 124, 64) | 256         |
| MaxPooling2D              | (None, 62, 62, 64)   | 0           |
| Conv2D_2                  | (None, 60, 60, 128)  | 73,856      |
| Conv2D_3                  | (None, 58, 58, 256)  | 295,168     |
| BatchNormalization_1      | (None, 58, 58, 256)  | 1,024       |
| MaxPooling2D_1            | (None, 29, 29, 256)  | 0           |
| Conv2D_4                  | (None, 27, 27, 256)  | 590,080     |
| Conv2D_5                  | (None, 25, 25, 512)  | 1,180,160   |
| BatchNormalization_2      | (None, 25, 25, 512)  | 2,048       |
| MaxPooling2D_2            | (None, 12, 12, 512)  | 0           |
| Conv2D_6                  | (None, 10, 10, 512)  | 2,359,808   |
| Conv2D_7                  | (None, 8, 8, 1024)   | 4,719,616   |
| BatchNormalization_3      | (None, 8, 8, 1024)   | 4,096       |
| MaxPooling2D_3            | (None, 4, 4, 1024)   | 0           |
| Flatten                   | (None, 16384)        | 0           |
| Dense                     | (None, 512)          | 8,389,120   |
| Dense_1                   | (None, 256)          | 131,328     |
| Dropout                   | (None, 256)          | 0           |
| Dense_2                   | (None, 64)           | 16,448      |
| Dense_3                   | (None, 10)           | 650         |

**Total params:** 17,783,050 (67.84 MB)

#### Metrics
| Epoch | Train Accuracy | Validation Accuracy | Train Loss | Validation Loss |
|-------|----------------|---------------------|------------|-----------------|
| 10    | 0.4297         | 0.1825              | 1.6925     | 2.2247          |
| 30    | 0.5859         | 0.5233              | 1.2179     | 1.4879          |
| 50    | 0.6875         | 0.6773              | 0.9265     | 1.1082          |
| 70    | 0.7812         | 0.7505              | 0.6469     | 0.7938          |
| 90    | 0.9297         | 0.8030              | 0.2579     | 0.6533          |
| 110   | 0.9902         | 0.9655              | 0.0230     | 0.1832          |

### Oversampling
This model was the same as the one above but trained with class balancing. Unfortunately, it did not provide any real improvement.

### Transfer Learning
After only achieving 84% accuracy on the test set, we applied transfer learning using VGG with ImageNet weights, freezing most layers except the last convolutional block.

The learning rate was reduced to 1e-4 for stability.

| Layer (type)              | Output Shape         | Param #     |
|---------------------------|----------------------|-------------|
| vgg16 (Functional)        | (None, 7, 7, 512)    | 14,714,688  |
| Flatten                   | (None, 25088)        | 0           |
| Dense                     | (None, 512)          | 12,845,568  |
| Dense_1                   | (None, 256)          | 131,328     |
| Dropout                   | (None, 256)          | 0           |
| Dense_2                   | (None, 128)          | 32,896      |
| Dense_3                   | (None, 10)           | 1,290       |

**Total params:** 27,725,770 (105.77 MB)

#### Metrics
| Epoch | Train Accuracy | Validation Accuracy | Train Loss | Validation Loss |
|-------|----------------|---------------------|------------|-----------------|
| 10    | 0.9457         | 0.9178              | 0.1566     | 0.2671          |
| 20    | 0.9660         | 0.9433              | 0.1017     | 0.1941          |

With validation size reduced to 10% and a smaller learning rate (1e-5):

| Epoch | Train Accuracy | Validation Accuracy | Train Loss | Validation Loss |
|-------|----------------|---------------------|------------|-----------------|
| 25    | 0.9810         | 0.9575              | 0.1566     | 0.2671          |

### Ensemble
We combined the two transfer learning models with the Big Classifier into a voting classifier.

Unfortunately, this did not outperform the second VGG-based model. Nevertheless, it ended up being our second-best submission.
