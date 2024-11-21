
DeepFace library which based on cutting edge Face Regconition technologies VGG-Face (by default), can be used to verify 2 images   
- Default detector opencv, distance default cosine, threshold tuned for each model 
-> use retinaface + facenet 512 as it yields the highest accuracy result with alignment on. 
- Benchmark: https://dergipark.org.tr/en/pub/gazibtd/issue/84331/1399077 shows that, top models have over 96% accuracy (Facenet512 is the highest being 98.4%)

Flow
<2 diagrams> 

Portal infos
- Users infos (passport, text fields, face biometric)   
- portal: only visible to bank clerks: submit/reject. on mobile app, there should be a interface to show status, status is taken directly from database. 
- 

### problems 
- Record of user biometric: how to collect data at once and send, is the data realtime? 
- Passport metadatas: front only
- Blockchain ?  



# State-of-the-Art Models for Face Recognition:

Face recognition technology has witnessed significant advancements in recent years, driven by breakthroughs in deep learning and the availability of large-scale annotated datasets. State-of-the-art models have pushed the boundaries of face recognition accuracy, robustness, and efficiency. Here, we discuss the theoretical foundations and advancements in some of the notable state-of-the-art models for face recognition.

**— FaceNet:** FaceNet, proposed by Schroff. in 2015, introduced the concept of triplet loss for learning face embeddings. By training a deep convolutional neural network (CNN) to map face images into a high-dimensional feature space, FaceNet achieved impressive results in face recognition. It learns to encode faces in such a way that similar faces are closer together in the embedding space, facilitating efficient face matching.

**— DeepFace:** DeepFace, presented by Taigman et al. in 2014, introduced a deep learning model based on a multi-layer CNN architecture. It achieved remarkable performance by training on a large-scale dataset and employing a vast number of labeled identities. DeepFace integrates both identification and verification tasks, making it suitable for various face recognition applications.

**— ArcFace:** ArcFace, proposed by Deng et al. in 2019, introduced the additive angular margin loss to improve face recognition accuracy and discrimination. By incorporating angular margin-based supervision into the CNN training process, ArcFace effectively enhances the inter-class separability of face embeddings. It achieves state-of-the-art performance on benchmark face recognition datasets.

**— VGGFace and VGGFace2:** VGGFace and VGGFace2 are deep CNN models proposed by Parkhi et al. and Cao et al., respectively. These models employ deep convolutional networks with multiple layers to extract discriminative features from face images. VGGFace and VGGFace2 have demonstrated impressive performance in face recognition, particularly when trained on large-scale datasets.

**— DeepID:** DeepID, introduced by Sun et al. in 2014, focuses on learning identity-specific features using multiple deep neural networks. It incorporates both verification and identification tasks to improve discrimination between faces. DeepID has shown strong performance in face recognition, particularly in scenarios with significant variations in pose, illumination, and expression.

These state-of-the-art models are based on deep learning architectures, such as convolutional neural networks (CNNs), and employ innovative loss functions and training techniques to learn powerful face representations. They often leverage large-scale labeled datasets, such as MS-Celeb-1M and CASIA-WebFace, to achieve remarkable face recognition performance.









# face regconition A.I
--- 
# Refererences 




2024 08 10 12:26
#literature  [[Python]] [[machine learning]]  