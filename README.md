# Masked-Face-Recognition-Challenge
This Repo was created for the Webface260M Masked Face Recognition Challenge project.
In this session, I used the Retinaface-Resnet50 model for face detection task and a pretrained weights of the model Resnet34 using Arcface loss function and Local Binary Pattern (LBP) for recognition task.

The Pipeline of algorithm:
1.  Using the pre-trained Resnet50.onnx model to detect and extract cropped face and also Bounding Boxes. The cropped face will be fed in Arcface model. The bounding boxes will be used in LBP task.

2.  Preprocessing face input (target_size = (112,112)) and normalize the face images to range [0,1].

After that, Create Model Arcface (backbone is Resnet34) with pretrained weight, which output the face embedding.
![image](https://user-images.githubusercontent.com/92146886/175832147-82cba1e1-3a91-44aa-be86-c4adbd1f8e88.png)
![image](https://user-images.githubusercontent.com/92146886/175832881-975cd5f4-7359-4f22-b419-ab7e8b40aa1c.png)

At this task, I omitted the last layer (fully connected layer of resnet34, which can be used for classifying task) and adding a Batch Norm. layer to take the face encode vector for step 3.

3. Calculate Cosine similarity between vector embeddings

  I follow the paper to choose this distance threshold:
- If distance < 0.35: This is the same person
- If 0.35 <= distance < 0.7: Using LBP method to verify if this is the same person or not
- Else: the face is unknown, this is not the same person

4. The faces which have distaces between 0.35 and 0.7 will be stored in an array and using Dlib to extract ROI of the eyebrow and eye region and then store in an array. Using LBP method to compare if this is the same person. (not done yet)

References:
- Paper: https://rdcu.be/cQqTn.

- The model Arcface is based on the implementation of Insightface with Keras version of PeteryuX and Deepface: 
https://github.com/peteryuX/arcface-tf2 and https://github.com/serengil/deepface/blob/master/deepface/basemodels/ArcFace.py

- The model Resnet34 was built based on Insightface team: https://github.com/deepinsight/insightface/tree/master/recognition. 

- The link to download pretrained model: https://github.com/leondgarse/Keras_insightface
