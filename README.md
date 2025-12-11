# SLAM-Notes
This is a repository for recoding the knowledge during learning SLAM.

## Category

Based on the different standards, we can classify the SLAM into different categories. For example:

Based on the **Frontend** data process method, there are two categories:

1. ***Indirect / Feature-based SLAM*** extract the keypoint from the image, and then utilize the feature matching and tracking to establish the geometric correspondences for pose estimation.
2. ***Direct SLAM*** adopt the intensity or gradient information from image. Specifically, based on the assumption of adjacent frames brightness constancy to estimate the pose by minimizing the photometric error.

Based on the **Sensor** type, we can classify the SLAM into two categories:

1. ***Visual SLAM*** can further subdivide according to different sensors, including ***Monocular***, ***Stereo*** and ***RGB-D***. 
2. ***LiDAR SLAM*** utilize the *LiDAR* sensor to support the precise depth information.

Particularly, Monocular SLAM is struggle to the **scale ambiguity** problem because lacking of depth, while RGB-D sensor can supprot the depth directly and Stereo sensor can calculate the depth by **triangulation** indirectly.
## Knowledge
Clarify the definition of technical terms:

***Point Cloud*** is a set of points that represents the 3D shape or object. Each point is associated with 3D position (***x, y, z***) and sometimes with additional information such as color (***R, G, B***), intensity, or normal vector.

***Camera Pose*** generally represents the camera position and orientation relative to the world coordinate system.

***T_wc***(*world-to-camera*) indicates transform point in the world coordinate system into the camera coordiante system, which is used for projecting the world points into the camera image such as **rendering** and **reprojection**.

***T_cw***(*camera-to-world*) depicts transform point in the camera coordinate system into the world coordiante system, which is used for describing the camera position in the world such as camera trajectory **evalutaion** and **visualization**.

## Perception
There are multiple visual SLAM with different perception frontiers, such as **Monocular**, **Stereo**, **RGB-D**, **LiDAR** etc. The purpose of perception is extract the image feature, estimate the camera pose and reconstruct the 3D environment.

### 1. Feature Extraction(Sparse)
This conventional feature extraction phase is consist of feature detection and matching. 

First, we need to adopt the `detector` to gain the keypoint positions from image and utilize the `matcher` to generate descriptors that comprise of local information for each keypoint. 

Then, based on calculating the distance between two feature descriptors, we can regard the lowest distance as the highest similarity to establish the potential correspoence.

 Furthermore, there are still exist the inaccurate matching results among the initial matches. We can use `filter` to remove incorrect sample and deliver the final matches.

Traditional hand-crafted feature extraction method:
<div align="center">

|Method|[Harris](https://www.bmva-archive.org.uk/bmvc/1988/avc-88-023.pdf)|[SIFT](https://www.cs.ubc.ca/~lsigal/425_2024W1/ijcv04.pdf)|[SURF](https://people.ee.ethz.ch/~surf/eccv06.pdf)|[FAST](https://www.edwardrosten.com/work/rosten_2006_machine.pdf)|[BRIEF](https://www.cs.ubc.ca/~lowe/525/papers/calonder_eccv10.pdf)|[ORB](https://par.cse.nsysu.edu.tw/resource/paper/2016/161129/ORB-an%20efficient%20alternative%20to%20SIFT%20or%20SURF.pdf)|
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| *detector* | ✓ | ✓ | ✓ | ✓ | – | ✓ |
| *matcher* | – | ✓ | ✓ | – | ✓ | ✓ |

</div>

With the development of deep learning, many works start to combine the ***Convolutional Neural Network*** or ***Transformer*** architecture to accompolish the feature extraction task. Subsequently, producing two different research direction, detector-based and detetor-free matching paradigm. 

Learning-based feature extraction method with or without detector:
<div align="center">

|Method|[SP](https://openaccess.thecvf.com/content_cvpr_2018_workshops/papers/w9/DeTone_SuperPoint_Self-Supervised_Interest_CVPR_2018_paper.pdf)+[SG](https://openaccess.thecvf.com/content_CVPR_2020/papers/Sarlin_SuperGlue_Learning_Feature_Matching_With_Graph_Neural_Networks_CVPR_2020_paper.pdf)|[R2D2](https://proceedings.neurips.cc/paper/2019/file/3198dfd0aef271d22f7bcddd6f12f5cb-Paper.pdf)|[LoFTR](https://openaccess.thecvf.com/content/CVPR2021/papers/Sun_LoFTR_Detector-Free_Local_Feature_Matching_With_Transformers_CVPR_2021_paper.pdf)|[SiLK](https://openaccess.thecvf.com/content/ICCV2023/papers/Gleize_SiLK_Simple_Learned_Keypoints_ICCV_2023_paper.pdf)|[LightGlue](https://openaccess.thecvf.com/content/ICCV2023/papers/Lindenberger_LightGlue_Local_Feature_Matching_at_Light_Speed_ICCV_2023_paper.pdf)|[ELoFTR](https://openaccess.thecvf.com/content/CVPR2024/papers/Wang_Efficient_LoFTR_Semi-Dense_Local_Feature_Matching_with_Sparse-Like_Speed_CVPR_2024_paper.pdf)|[EDM](https://arxiv.org/pdf/2503.05122)|
|---|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| *w*. *detector* | ✓ | ✓ | – | ✓ | ✓ | – | – |
| *w*.*o*. *detector* | – | – | ✓ | – | – | ✓ | ✓ |

</div>

### 2. Depth Estimation(Dense)
Predicting the pixel depth value from a single RGB image.

<div align="center">

|Method|[Monodepth](https://openaccess.thecvf.com/content_cvpr_2017/papers/Godard_Unsupervised_Monocular_Depth_CVPR_2017_paper.pdf)|[Monodepth2](https://openaccess.thecvf.com/content_ICCV_2019/papers/Godard_Digging_Into_Self-Supervised_Monocular_Depth_Estimation_ICCV_2019_paper.pdf)|[DA](https://openaccess.thecvf.com/content/CVPR2024/papers/Yang_Depth_Anything_Unleashing_the_Power_of_Large-Scale_Unlabeled_Data_CVPR_2024_paper.pdf)|[DA2](https://proceedings.neurips.cc/paper_files/paper/2024/file/26cfdcd8fe6fd75cc53e92963a656c58-Paper-Conference.pdf)|[Marigold](https://openaccess.thecvf.com/content/CVPR2024/papers/Ke_Repurposing_Diffusion-Based_Image_Generators_for_Monocular_Depth_Estimation_CVPR_2024_paper.pdf)|[Depth Pro](https://proceedings.iclr.cc/paper_files/paper/2025/file/bc8b2058fd96978a4146f18298cb2d39-Paper-Conference.pdf)| [VDA](https://openaccess.thecvf.com/content/CVPR2025/papers/Chen_Video_Depth_Anything_Consistent_Depth_Estimation_for_Super-Long_Videos_CVPR_2025_paper.pdf)|[Marigold-DC](https://openaccess.thecvf.com/content/ICCV2025/papers/Viola_Marigold-DC_Zero-Shot_Monocular_Depth_Completion_with_Guided_Diffusion_ICCV_2025_paper.pdf)|
|---|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| *backbone* | *CNN* |  *CNN* | *ViT* | *ViT* | *Diffusion* | *Transformer* | *ViT* | *Diffusion* |

</div>

We usually use the raw depth map as the SLAM input for pose optimization and map reconstruction, which indicate the real space depth. On the other hand, colorful result of depth map in thesis is convert the depth map type from gracyscale into rgb, which only use for direct visualization.


## Evaluation
We using the prevalent package as [evo](https://github.com/MichaelGrupp/evo) for evaluation. There are two common evaluation metrics for camera trajectory, Detailed description can be found in [here](https://github.com/MichaelGrupp/evo/wiki/Metrics).

1. ***APE***(*Absolute Pose Error*) serves as a metric for evaluating the global consistency of a trajectory, which quantifies the disparity between the estimated poses provided by the SLAM system and the ground truth poses.
```
evo_ape FORMAT GT_POSE ESTIMATED_POSE -va --plot --plot_mode xyz --correct_scale
```
2. ***RPE***(*Relative Pose Error*) is a widely used metric for assessing the local consistency of a trajectory, which compares the relative transformations between successive poses in the estimated trajectory against the corresponding transformations in the ground truth trajectory.
```
evo_rpe FORMAT GT_POSE ESTIMATED_POSE -va --plot --plot_mode xyz --correct_scale
```
`FORMAT` is the specific format of the saved trajectory file, such as `tum`, `kitti`, `rosbag` etc, elaborate format information refer to [here](https://github.com/MichaelGrupp/evo/wiki/Formats). `GT_POSE` and `ESTIMATED_POSE` are the paths to the ground truth and the estimated pose file respectively. 

**Tips**: When you evaluate the trajectory estimated by different methods, the final poses may have different length because of the initialization in SLAM system. 

1. You need to add the `-va` option to print the compelete statistical information and align the estimated trajectory with the ground truth trajectory.

2. You need to add the `--correct_scale` option to prevent the **scale drift** problem of the estimated trajectory in the **Monocular** sensor scenario. In other sensor cases, **do not** use this option for a fair comparison.