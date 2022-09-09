# Multitask Learning for Improving Models Robustness

During my master's project, which was the diagnosis of a type of blood cancer called leukemia, we faced the problem of white blood cell classification. On the available dataset, we trained common neural networks such as CNN and Vision Transformers for this purpose. The model performed very well on the test set, but when we indirectly tasted those models (because we do not access the labels exactly) on an out-of-distribution test set, we discovered that the model's performance had fallen significantly.


<div align="center">
<img src="https://user-images.githubusercontent.com/53098142/189374377-c0d05112-ff67-4fb0-b244-83d69fe334c1.png" alt="Multi task network" width="500"/>
</div>


We proposed forcing the network to embed the cell morphological data on its produced feature vector to solve this problem. To accomplish this, we add a segmentation head to the previous network, implying that the network should perform classification and segmentation tasks concurrently. The main issue in that situation was that we didn't have a dataset of that size with both class labels and segmentation masks for each image. So we used a dataset 14.5 times smaller than the previous dataset and achieved nearly the same accuracy. The main accomplishment was that we discovered these new models perform significantly better on the out-of-distribution test set.

Because our model predicts a segmentation mask for each input image in addition to the class label, we decided to put it to the test on the cell segmentation problem. The model's goal was to segment the nucleus and cytoplasm in each image of a white blood cell. As a starting point, we used the U-Net architecture. We discovered that in the case of small training sets, U-Net makes decisions based on relative intensity, which causes it to perform very poorly on cell classes that lack the nucleus but have cytoplasm that is very similar in color to the nucleus of other classes. In this case, our model performs much better and does not have the mentioned issue. In the table below, for example, U Net made a mistake for Basophil cell types, but the proposed method performs very well.

<div align="center">

|  | U-Net | Proposed Idea |
| :---:         |     :---:      |          :---: |
| Eosinophil| <img src="https://user-images.githubusercontent.com/53098142/189376313-f915a856-8814-4f85-b555-34c5aa229f5e.png" alt="U Net - Eosinophil" width="200"/>    | <img src="https://user-images.githubusercontent.com/53098142/189376321-cc67a579-0c3c-4b2a-8cec-9e3e571140e3.png" alt="two tasks_Eosinophil" width="200"/>  |
| Lymphocyte| <img src="https://user-images.githubusercontent.com/53098142/189376317-60c91c03-60a5-4690-8883-6c13bcbee075.png" alt="U Net - Lymphocyte" width="200"/>    | <img src="https://user-images.githubusercontent.com/53098142/189376308-8d87df9c-bdcf-4910-8ecc-753c5c2d9956.png" alt="two tasks_Lymphocyte" width="200"/>  |
| Basophil| <img src="https://user-images.githubusercontent.com/53098142/189376310-efa17672-6782-4dc4-91e1-dc68eefbf6eb.png" alt="U Net - Basophil" width="200"/>    | <img src="https://user-images.githubusercontent.com/53098142/189376318-844657aa-e729-495a-9544-c77338e54089.png" alt="two tasks_Basophil" width="200"/>  |

</div>


As you can see, the preliminary tests of the proposed idea are very promising, and I believe that this idea can be developed and tested using adversarial methods. According to my intuition, this model will perform very well on adversarial attacks because it embeds morphological information in its proposed feature vector.


