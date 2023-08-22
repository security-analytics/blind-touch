Load the modules for training.

# Blind-Touch: Homomorphic Encryption-Based Distributed Neural Network Inference for Privacy-Preserving Fingerprint Authentication

- This repository provides source code to try using homomorphic encryption-based fingerprint authentication.
- The authentication server is a cluster structure designed to allow authentication of 5000 fingerprint data in approximately 650 ms. (Based on Current Model)
- Currently, we don't use various preprocessing techniques separately, but we use the API of the simple opencv library and imgaug library. The preprocessing process will be updated later through continuous research.
- CNN models will also be updated in the study. When the update proceeds, we will update it to a new branch or directory that is different from the current version.

## 1. Server Setting
- In this scenario, needs 5 servers.
  - One client, one main server, and three cluster servers.
- We use NAVER Cloud server (https://www.ncloud.com/product/compute/server)
  - All server spec is Standard-g2 Server.
  - CentOS 7.8.64
  - Two cores (Intel(R) Xeon(R) Gold 5220 CPU @ 2.20GHz) with 8GB DRAM
- We use NAS storage (https://www.ncloud.com/product/storage/nas) 
  - To mount four servers: one main server, and three cluster servers.
  - The NAS is used for transferring ciphertexts.

## 2. Installation
- CUDA 11.4
- Python 3.8.1
- SEAL-Python library (https://github.com/Huelse/SEAL-Python)
- Python packages are detailed separately in ```requirements.txt```

## 3. Setting
### 3.1 Dataset download
- PolyU Cross Sensor Fingerprint Database (PolyU)
  - It can be obtained to contact the Hong Kong Polytechnic University.
  - In this scenario, we use the ```processed_contactless_2d_fingerprint_images``` dataset.
  - Each measuring 350 X 225 pixels.
- SOKOTO Coventry Dataset (SOKOTO)
  - The SOKOTO dataset can download from the Kaggle site(https://www.kaggle.com/datasets/ruizgara/socofing)
  - In our scenario, we use the real dataset (It consists of 6,000 fingerprint images)
  - Each measuring 96 X 103 pixels.

### 3.2 Data Preprocessing
- The fingerprint data need data preprocessing.
- PolyU Dataset case
  - We use the following preprocessing methods.
  - resize to 224 X 224
  - dilate in cv2 library (kernel=None)
  - erode in cv2 library (kernel=None)
  - Sobel in cv2 library (ddepth=cv2.CV_64F, dx=0, dy=1, ksize=9)
  - The cropping and other minutiae extraction methods are not used in this experiment. If you want to use some methods, use them.

- SOKOTO Dataset case
  - resize to 224 X 224
  - The cropping and other minutiae extraction methods are not used in this experiment. If you want to use some methods, use them.

### 3.2 Model Training
- PolyU training sample
  - Use the notebook ```Blind-Touch-Training-SampleNotebook(PolyU).ipynb```
  - Load the Pre-processed dataset in 3.2

- SOKOTO training sample
  - Use the notebook ```Blind-Touch-Training-SampleNotebook(SOKOTO).ipynb```
  - Load the Pre-processed dataset in 3.2
  - Save the test set and two trained models (feature_model, model)
  - This is used in this experiment. 

### 3.3 Client Setting
- Firstly, set the ```Blind-Touch-Client.ipynb``` in the client server.
- Load the test set and feature_model, and the model which is obtained in the training.
- Generate key sets and move the public key, galois key, and relinearization key to the server
- Set the path and parameter
- Move the generated ciphertext (ctxt1, ctxt2, ctxt3) which is used as the registered ciphertext in the server. Send the ciphertexts to Clusert1, Cluster2, and Cluster3 each other.

### 3.4 Main Server Setting
- Firstly, set the code in the main server
- Copy and store the key set in the path (Define the path)

### 3.5 Cluster Server Setting
- Firstly, set the code in each cluster servers
- Copy and store the key set and the ciphertext in the path (Define the path)
- All setting is defined in the conf.py

### 4. Run codes
- Run the Main server and three cluster servers
- Run the Client code.
 
