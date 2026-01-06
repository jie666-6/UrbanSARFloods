# UrbanSARFloods

This repo contains the dataset, explanation, and some corrections of the paper:  
[UrbanSARFloods: Sentinel-1 SLC-Based Benchmark Dataset for Urban and Open-Area Flood Mapping](https://openaccess.thecvf.com/content/CVPR2024W/EarthVision/papers/Zhao_UrbanSARFloods_Sentinel-1_SLC-Based_Benchmark_Dataset_for_Urban_and_Open-Area_Flood_CVPRW_2024_paper.pdf)  
by Jie Zhao, Zhitong Xiong, and Xiao Xiang Zhu.

This dataset release focuses on data availability. All baseline models were trained using standard segmentation pipelines implemented in the segmentation_models_pytorch (SMP) library.
Since no customized code was developed, training scripts are not included.


👉 Check out the [UrbanSARFloods](https://huggingface.co/datasets/S1Floodbenchmark/UrbanSARFloods_v1) for details.

## Introduction
UrbanSARFloods is a large-scale, SAR-based flood mapping dataset processed from **Sentinel-1 Single Look Complex (SLC) data**, designed to address the lack of urban flood data in deep learning research. While SAR is widely used for flood detection due to its all-weather capability, most existing datasets focus on open-area floods while neglecting urban environments.

UrbanSARFloods features **Sentinel-1 intensity** and **interferometric coherence** image chips (**512 × 512**), covering 807,500 km² across 5 continents, 20 land cover classes, and 18 flood events. We benchmarked state-of-the-art CNN models and found that imbalanced data and limited training samples remain major challenges, particularly for urban flood detection.

Expanding this dataset and exploring transfer learning and data balancing strategies could further improve SAR-based flood mapping. To ensure accurate geospatial analysis, all data is provided in **GeoTIFF** format, preserving both geolocation and projection information.


## Usage
1️⃣ **Download** the training and validation dataset (**urban_sar_floods.tar.gz**) from [here](https://huggingface.co/datasets/S1Floodbenchmark/UrbanSARFloods_v1), then **extract** it to `./urban_sar_floods`.  
2️⃣ The extracted dataset will be organized as follows:

```plaintext
urban_sar_floods
├── 01_NF
│   ├── GT       # Ground truth TIFF files
│   │   ├── file_1.tif
│   │   ├── file_2.tif
│   │   └── ...
│   ├── SAR      # SAR TIFF files
│   │   ├── file_1.tif
│   │   ├── file_2.tif
│   │   └── ...
├── 02_FO
│   ├── GT
│   ├── SAR
├── 03_FU
│   ├── GT
│   ├── SAR
├── Train_dataset.txt   # List of training samples
├── Valid_dataset.txt   # List of validation samples
```
3️⃣ Download the testing dataset (testing_case_256 /testing_case_orig ) from  [here](https://huggingface.co/datasets/S1Floodbenchmark/UrbanSARFloods_v1) as follows:
```plaintext
testing_case_256  # Testing dataset (preprocessed SAR into 256×256 patches)
├── Event1
│   ├── SAR_patch_001.tif
│   ├── SAR_patch_002.tif
│   ├── ...
├── Event2
│   ├── SAR_patch_001.tif
│   ├── SAR_patch_002.tif
│   ├── ...
├── Event3
│   ├── SAR_patch_001.tif
│   ├── SAR_patch_002.tif
│   ├── ...

testing_case_orig  # Testing dataset (original full-size SAR and GT files)
├── Event1
│   ├── SAR.tif  # Full-size SAR image
│   ├── GT.tif   # Full-size ground truth
├── Event2
│   ├── SAR.tif
│   ├── GT.tif
├── Event3
│   ├── SAR.tif
│   ├── GT.tif
```
4️⃣ The original Sentinel-1 Single Look Complex (SLC) data used in the UrbanSARFloods dataset are provided  [here](https://huggingface.co/datasets/S1Floodbenchmark/UrbanSARFloods_v1/tree/main/Sentinel-1_SLC_data).
```
Sentinel-1_SLC_data  # Original Sentinel-1 SLC data for UrbanSARFloods
├── Event1
│   ├── S1A_*.zip  # Pre-flood SLC acquisition
│   ├── S1A_*.zip  # Pre-flood SLC acquisition
│   ├── S1A_*.zip  # Post-flood SLC acquisition
├── Event2
│   ├── S1A_*.zip
│   ├── S1A_*.zip
│   ├── S1A_*.zip
├── Event3
│   ├── S1A_*.zip
│   ├── S1A_*.zip
│   ├── S1A_*.zip
├── ...
```
For each flood event, the repository includes:
1) Two pre-flood Sentinel-1 SLC acquisitions, and
2) One post-flood Sentinel-1 SLC acquisition.
All files are stored using their original Sentinel-1 product filenames.
Users can directly retrieve detailed acquisition metadata, including exact sensing time, absolute orbit number, relative orbit (path), and track information, from the official ESA Sentinel-1 data hub by referencing these filenames.



### 🛠 Note: Cropping GeoTIFF Data  
If you need to crop images in **testing_case_orig**  to a specific size or align it with another geotif file, you can use **GDAL's `gdalwarp` tool**.  

For more details, check out the [GDAL Warp documentation](https://gdal.org/programs/gdalwarp.html).  


## Correction
📢 Note: This table is the corrected version of Table 2 from the CVPRW UrbanSARFloods paper, with missing information supplemented. Please refer to this version for accurate data.

|   Continent   |             Location             |   Event Date   |          Image Size          | Absolute Orbit |   Path  | Number of NF tiles  | Number of FO tiles | Number of FU tiles |
|--------------|----------------------------------|--------------|----------------------------|---------------|--------|------------------|------------------|------------------|
| **North America** | Houston, US | 19 April 2016 | 17766 × 13306 | 10890  | 143 | 175 | 209 | 154 |
|              | Houston, US | 30 August 2017 | 14918 × 12981 | 7169 | 143 | 274 | 323 | 129 |
|              | Lumberton, US | 11 Oct 2016 | 9931 × 6465 | 13449 | 77 | 147 | 201 | 2 |
|              | Sainte-Marthe-sur-le-Lac, Canada | 02 May 2019 | 21638 × 11184 | 27055 | 33 | 279 | 431 | 2 |
| **Africa**   | Beledweyne, Somalia | 08 May 2018 | 14293 × 11656 | 21807 | 35 | 495 | 73 | 4 |
|              | Beira, Mozambique | 20 March 2019 | 14904 × 12608 | 15432 | 6 | 133 | 89 | 14 |
|              | Beledweyne, Somalia | 14 Nov 2023 | 15457 × 12634 | 51207 | 35 | 455 | 56 | 20 |
|              | Jubba, Somalia* | 01 Dec 2023 | 15548 × 13078 | 50580 | 108 | 400 | 59 | 13 |
|              | | | 15454 × 12710 | 51455 | 108 | 460 | 65 | 10 |
|              | Lokoja, Niger | 13 Oct 2022 | 15500 × 12587 | 44902 | 30 | 427 | 107 | 1 |
| **Asia**     | Iwaki/Koriyama, Japan | 12 Oct 2019 | 11751 × 10096 | 18447 | 46 | 353 | 158 | 19 |
|              | Weihui, China* | 27 July 2021 | 18927 × 12245 | 38962 | 40 | 293 | 221 | 135 |
|              | Aqqala, Iran | 29 March 2021 | 19549 × 12580 | 26554 | 57 | 333 | 351 | 25 |
|              | Zhuozhou, China | 05 August 2023 | 19906 × 12207 | 49739 | 142 | 332 | 204 | 137 |
|              | Langfang, China | 05 August 2023 | 19458 × 12220 | 49739 | 142 | 279 | 269 | 117 |
| **Oceania**  | Coraki, Australia | 2 March 2022 | 18160 × 13358 | 42146 | 74 | 105 | 54 | 11 |
|              | Sydney, Australia | 24 March 2021 | 19495 × 13582 | 37144 | 147 | 468 | 95 | 6 |
|              | Sydney, Australia | 5 July 2022 | 19498 × 13584 | 43969 | 147 | 391 | 155 | 23 |
|              | Port Macquarie, Australia | 19 March 2021 | 18774 × 13460 | 37071 | 74 | 124 | 89 | 26 |
| **Europe**   | NovaKakhovka, Ukraine* | 09 June 2023 | 22596 × 12226 | 48911 | 14 | 103 | 612 | 37 |




## License
The dataset is are released under the CC-BY-4.0 license.

## Citation
If you find this repository useful, please consider citing the following paper:

```bibtex
@INPROCEEDINGS{10678367,  
  author={Zhao, Jie and Xiong, Zhitong and Zhu, Xiao Xiang},  
  booktitle={2024 IEEE/CVF Conference on Computer Vision and Pattern Recognition Workshops (CVPRW)},  
  title={UrbanSARFloods: Sentinel-1 SLC-Based Benchmark Dataset for Urban and Open-Area Flood Mapping},  
  year={2024},  
  pages={419-429},  
  keywords={Training;Satellites;Urban areas;Transfer learning;Sentinel-1;Land surface;Benchmark testing;Sentinel-1;flood mapping;benchmark dataset;urban flood},  
  doi={10.1109/CVPRW63382.2024.00047}  
}
```
## Further Information  

For a comprehensive review of urban flood mapping with satellite SAR data, we invite you to explore our recent related paper:

🔗 **[Urban Flood Mapping Using Satellite Synthetic Aperture Radar Data: A review of characteristics, approaches, and datasets](https://ieeexplore.ieee.org/document/10795465)**  

This paper provides an in-depth discussion on challenges, methodologies, and future directions in urban flood detection, complementing the UrbanSARFloods dataset. 

```bibtex
@ARTICLE{10795465,
  author={Zhao, Jie and Li, Ming and Li, Yu and Matgen, Patrick and Chini, Marco},
  journal={IEEE Geoscience and Remote Sensing Magazine}, 
  title={Urban Flood Mapping Using Satellite Synthetic Aperture Radar Data: A review of characteristics, approaches, and datasets}, 
  year={2024},
  volume={},
  number={},
  pages={2-34},
  keywords={Floods;Buildings;Urban areas;Sensors;Synthetic aperture radar;Reviews;Backscatter;Sensor phenomena and characterization;Spatial resolution;Spaceborne radar},
  doi={10.1109/MGRS.2024.3496075}}
```
