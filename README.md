# codebrim_challenge
This repository was made to attend to the CODEBRIM challenge hosted on "https://dacl.ai/". We highly encourage the readers to visit "https://github.com/phiyodr/building-inspection-toolkit" for more information on automatic infrastructure inspection. 

# Dataset
Codebrim is a multi-classes multi-labels dataset of common infrastructure defects. There are 6 classes ['Background', 'Crack', 'Spallation', 
'Efflorescence' , 'ExposedBars', 'CorrosionStain'] and 7729 images split in train (6481 imgs) / test (632 imgs) / val (616 imgs). 
A balanced version of the dataset is available with 9209 images in the train dataset. 
Name      | Type        | Unique images | train/val/test split
----------|-------------|---------------|-------------
CODEBRIM [[Paper]](https://openaccess.thecvf.com/content_CVPR_2019/html/Mundt_Meta-Learning_Convolutional_Neural_Architectures_for_Multi-Target_Concrete_Defect_Classification_With_CVPR_2019_paper.html) [[Data]](https://zenodo.org/record/2620293#.YO8rj3UzZH4) | 6-class multi-target Clf  | 7,730 | original-version

# Model
Our model is a Vision Transformer with 12 transformer encoder layers, 6 heads, an embedding dimension of 384 and a patchsize of 8. 
A class token is concatenated to the input patch sequence. The concatenated class tokens of the 4 last layers are used as features for the classification task.
The classifier is a simple linear layer. A sigmoid activation function convert the logits into probabilities and a 0,5 threshold convert the probabilities into 
prediction.

# Training
To deal with class imbalance, we apply multiple class-balancing tricks regarding the loss, the weight regularization constraints and the parameters freezing. 
We don't use any over-sampling by data augmentation.

# Usage
To load the model
```python
model = build_model(pretrained_weights='./vit/weights/models.pth', img_size=224, num_cls=6)
```

To make predictions:
```python
labels_list =  ['NoDamage' , 'Crack', 'Spalling', 'Efflorescence', 'BarsExposed', 'Rust']
make_predictions(model, img_path, labels= labels_list)
```
![My Image](codebrim.png)

```python
NoDamage       ........................................  0.07% 
Crack          +++.....................................  9.97% 
Spalling       +++++++++++++++++++++++++++++++++++++++. 99.23% 
Efflorescence  ........................................  0.82% 
BarsExposed    +++++++++++++++++++++++++++++++++++++++. 98.82% 
Rust           ++++++++++++++++++++++.................. 55.86% 
inference time = 253.40 ms
```
  

