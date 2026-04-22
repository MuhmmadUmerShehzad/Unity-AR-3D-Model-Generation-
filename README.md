# Unity AR 3D Model Generation Demo

This project demonstrates an Augmented Reality (AR) pipeline for generating and loading 3D models from captured images dynamically within Unity. 

##  Important Note
**The pipeline is originally defined and intended for mobile devices, but we are using a standard camera from Unity for project demonstration purposes only.**

## Pipeline Overview

The project follows a streamlined architecture for transforming a 2D image into a runtime 3D model by utilizing cloud-based processing:

1. **Photo Capture**: A snapshot is captured from the Unity camera (acting as the device camera).
2. **Image Upload & API Routing (Railway)**: The captured image is sent as a multipart form-data payload to our custom intermediary API hosted on **Railway** (`web-production-8092e.up.railway.app`). We use this Railway app to regulate and manage incoming requests from our own server before forwarding them for heavy processing.
3. **Model Generation (Google Colab)**: The core 3D model generation requires significant computational power. To avoid the need for the end-user to have a high-end device, the heavy lifting is offloaded to a **Google Colab** environment. The Railway server routes the image here to be converted into a 3D mesh.
4. **Processing & Polling**: The Railway server returns a `job_id` back to the Unity client. The Unity application periodically polls the Railway server's status endpoint to check if the Colab backend has finished generating the model.
5. **Mesh Download**: Once the status returns as "done", the generated 3D mesh (`.obj` file) is downloaded via the Railway API to the device's local persistent storage.
6. **Runtime Loading**: An OBJ Loader dynamically reads the downloaded mesh file and instantiates the 3D model directly into the scene at a designated spawn point.

## Core Scripts

- `PhotoCapture.cs`: Handles capturing the screen/camera view, communicating with the backend API to upload the image, polling for job completion, and downloading the finalized mesh.
- `ModelLoader.cs`: Subscribes to the model ready event, handles the runtime importing of the downloaded `.obj` file using a runtime OBJ importer, and configures the model's materials, scale, and placement within the scene.

## Project Link

You can view the project demo/files here: [Google Drive Link](https://drive.google.com/file/d/18kMDEZAx1Snnf576x37kZHxuFtxbv8Yb/view?usp=sharing)
