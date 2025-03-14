# RGB-D Facial Reconstruction under Occlusion

This project addresses the challenge of reconstructing occluded facial regions using RGB-depth images. A non-occluded reference image is used for registration and provides the visual form of the occluded region. This method is particularly relevant for applications with access to extensive databases, such as hospital systems for recognizing masked employees in secure areas.

### Tested Occlusions:

- Blue medical face mask
- Black sunglasses
- Pink sunglasses
- Scarf
- Hands shielding the face

The primary focus is on face masks and black sunglasses as they occlude distinct facial regions without deforming the face and present distinct colors against other facial features. We are primarily focused on classical computer vision techniques to improve theoretical understanding, but we have included some DL/ML methods in certain pipelines as alternatives to our naive techniques, like DBSCAN.

## Pipeline Workflow

1) Data Collection
    Occluded and non-occluded images are captured using a PrimeSense Carmine 1.09 RGB-depth camera.

2) Data Conversion
    Video streams from the depth and color images are converted into numpy arrays.
    Four frames are collected for each scenario: color and depth frames for both occluded and non-occluded images.
    Depth and colour images are are either considered as distinct 2D images for the same frame, or as one 3D point cloud. 

4) Image Registration
    Images are registered using depth information via the Iterative Closest Point (ICP) algorithm or similar techniques.
    This transforms the non-occluded image to align with the occluded image's spatial reference.

5) Occlusion Segmentation
    Identify regions where RGB values differ between the transformed non-occluded and occluded images.
    Extract the occluded region using these differences.

6) Mask Generation
    Generate a binary mask to represent the occluded area.

7) Facial Reconstruction
    Project the occlusion mask onto the non-occluded image to extract the visible region.
    Combine the non-occluded mask with the occluded image to reconstruct the face.


## Experimental Setup

The project is divided into four pipelines using different combinations of techniques to improve performance:

1) Naive Pipeline
    Minimal movement (rotation and translation) for simple reconstruction.
    Uses 2D images for depth and colour (not point cloud), ORB, basic affine transformation for registration, K-means clustering, and morphological thresholding

2) ICP Pipeline
   Uses 2D images for depth and colour (not point cloud), ORB, ICP for registration, K-means clustering

3) DBSCAN/ Haar Cascade Pipeline
   Uses 2D images for depth and colour (not point cloud), ORB, ICP for registration, DBSCAN clustering, and Haar Cascade
   DBSCAN and Haar Cascade are pre-trained, deep learning approaches, respectively for clustering and identifying occlusions over the face.

4) ICP - Point Cloud Pipeline
   Uses Point Cloud for 3D reconstruction, ORB, ICP for registration, DBSCAN clustering 



