# Introduction
- If you are interested in building a dataset for training an image segmentation model, then you have to come up against polygon labeling. Polygons are more precise than bounding boxes but, take more time to apply labels to numerous instances in a large number of images.
- Now that, Polygon Auto-Annotate Tool is a simple-UI application, in which you can apply a polygon annotation to objects in an image with as few as a drag-and-drop. It takes advantage of **an instance segmentation model** to automatically segment items and create polygon masks or boundaries.

## Major features
### 1. The basics
- **Drag-and-drop a region** containing the entirety of the object that you would like to segment. Make sure that there is a reasonable padding surrouding the object, since the model does not work well when the selected region is too tight to the object.
- A cropped image will be shown up in a modal. You have to click on the object to specify the segmented target.

### 2. Remove a mask
- Whenever you feel disappointed with a mask, you certainly can remove that mask from annotation. **Ctrl + Click** on the mask and the image will be updated. If there is no mask in that place, no action will be executed.

### 3. Include or exclude regions
- **Shift + Click** on a point, then a small reasonable region will be added to the current mask. A unit added is a superpixel that this point belongs to.
- We apply **the Sklearn's SLIC implementation** to segment the image into clusters. The Include-click handler adds the corresponding cluster to the mask.
- Notice that the cluster is added to the newest mask, not the nearest mask. We will amend this limit in the future.
- Contrastively, **Alt + Click** on a point will erase a cluster from the mask. If there is no mask in that place, no action will be taken.

### 4. Export
- Click on the **Export** button to download the Json file of annotations. For the sake of simplicty, we only maintain a simple schema as follow:
```javascript
{
    "filename": "sample_image.jpg",
    "annotations": [
        {
            "name": "Object_1",
            "boundary": []
        },
        {
            "name": "Object_2",
            "boundary": []
        },
        ...
    ]
}
```
- TODO: Allows output to standard formats such as: COCO and Pascal VOC.

## Demo
👉 Check out this website: [polygon-auto-annotate-tool.fly.dev](https://polygon-auto-annotate-tool.fly.dev/)
- Because of the limitation of hardware resources, it may take time to run the website (~5-7 seconds for an inference). We recommend to install and run the application locally.

👉 Youtube video for how to install and demo: 

# Installation
After the successful installation, the application will be available at http://localhost:8080
## 1. Build with Docker

Easy install with Docker.
### Build Dockerfile
```bash
# Find Dockerfile and build an image.
$ docker build -t "polygon-auto-annotate-tool" .

# Run the container
$ docker run --name "polygon-auto-annotate-tool" -p 8080:8080  "polygon-auto-annotate-tool"
```

### Pre-built Docker Image
Dockerhub pre-buiilt image: [Dockerhub-link](https://hub.docker.com/r/nxquang2002/polygon-auto-annotate-tool)
```bash
$ docker pull nxquang2002/polygon-auto-annotate-tool

# Run
$ docker run --name "polygon-auto-annotate-tool" -p 8080:8080  "nxquang2002/polygon-auto-annotate-tool"
```

## 2. Build from source
Or you can follow these steps to install:

```bash
$ conda create -n polygon-annotate python=3.9 -y
$ conda activate polygon-annotate

# Install packages
$ pip install -r requirements.txt

# Clone and build detectron2 from source. Since it is modify 
# to adapt to this app, we have forked and modified the source code.
$ pip install -e git+https://github.com/nxquang-al/detectron2.git#egg=detectron2

# Download model weights from Google Drive.
$ gdown https://drive.google.com/uc?id=1qhoiYU92BvJuum74rY6LpiG7-FoEqoO9 -O ./models/
```

# User Guides
- Allowed operations:
    - *Drag-and-drop*: select a region that contains the object
    - *Ctrl + click*: remove the mask, if there is no mask, nothing is taken.
    - *Shift + click*: Include click, add a reasonable region to the current mask
    - *Alt + click*: Exclude click, erase a region from the mask.
    - Click on **Export** to download Json file of annotations.

# Acknowledgement
- This application is inspired by the Roboflow Smart Polygon Labeling, but worse. Instead of labeling in one click, our app requires more operations but, provide more flexibility in selecting objects.
