# Dataset Preparation

This directory documents the data acquisition and preprocessing workflow used to build the datasets for the Generative AI for Geospatial Design project.

The datasets combine satellite and aerial imagery with multiple geospatial conditioning layers and textual descriptions. Two spatial scales were developed: district-level and block-level, each with different image resolutions and tile sizes.

## 1. Data Sources

### 1.1 Satellite Imagery – District Level

Satellite imagery covering Lebanon was acquired from **Google Earth Engine (GEE)** using Sentinel-2 imagery.

- Spatial resolution: 10 meters per pixel.
- Geographic coverage: Lebanon.
- Temporal coverage: Multiple seasons and dates.
- Preprocessing: Cloud masking was applied to remove cloudy pixels and improve image quality.
- Imagery was prepared for RGB visualization and aligned with the corresponding geospatial conditioning data.

Multiple seasons were included to expose the model to different environmental and visual conditions.

### 1.2 High-Resolution Imagery – Block Level

Very high-resolution imagery was obtained through **Esri** for different areas across Lebanon.

- Spatial resolution: 0.3 meters per pixel.
- Geographic coverage: Selected portions of Lebanon.
- Temporal coverage: Multiple acquisition dates.

This imagery was used to develop the block-level dataset, allowing the model to learn finer spatial details and urban structures.

## 2. Geospatial Conditioning Data

For each imagery acquisition date, corresponding geospatial data was collected and prepared to guide the image generation process.

The conditioning layers included:

| Conditioning layer | Description |
|---|---|
| DEM | Digital Elevation Model representing terrain elevation. |
| Slope | Terrain slope derived from the DEM. |
| Aspect | Terrain orientation derived from the DEM. |
| Roads | Road network representing transportation infrastructure. |
| Rivers | River network representing hydrographic features. |
| LULC | Land Use and Land Cover information. |
| Buildings | Building footprints used in the block-level model. |

The conditioning data was selected to provide complementary information about terrain, land cover, and human-made structures.

## 3. Geospatial Preprocessing

All datasets were prepared using **ArcGIS Pro** geoprocessing tools.

The preprocessing workflow included:

1. Aligning the imagery and conditioning layers spatially.
2. Normalizing the conditioning data to ensure consistent value ranges.
3. Rasterizing vector-based layers, including roads, rivers, land use and land cover, and building footprints where applicable.
4. Ensuring that the conditioning layers corresponded to the same geographic areas and acquisition dates as the reference imagery.
5. Combining the prepared layers into aligned conditioning datasets.

This process ensured that each training sample contained a reference image and its corresponding geospatial conditions.

## 4. Image Tiling

The aligned imagery and conditioning datasets were divided into smaller tiles to create individual training samples.

Two tile sizes were used:

| Dataset | Image resolution | Tile size |
|---|---|---|
| District level | 10 m/pixel | 256 × 256 pixels |
| Block level | 0.3 m/pixel | 512 × 512 pixels |

Each tile was paired with its corresponding conditioning data to maintain spatial consistency between the reference imagery and model inputs.

## 5. Textual Prompt Generation

In addition to the geospatial conditioning layers, textual descriptions were generated for each sample using a locally running language model through **Ollama**.

The prompts incorporated information such as:

- The geographic location of the tile, including its Qada (district) name.
- The corresponding season.
- Relevant landscape and geographic characteristics.

The purpose was to introduce geographic and seasonal context into the generation process, allowing the model to learn associations between textual descriptions, spatial conditions, and satellite imagery.

## 6. Data Augmentation and Dataset Splitting

Data augmentation was applied to the district-level training dataset to increase sample diversity and improve model generalization.

The final dataset sizes were:

### District-Level Dataset

| Split | Number of samples |
|---|---:|
| Training | 9,350 |
| Validation | 1,169 |
| Testing | 1,169 |
| **Total** | **11,688** |

### Block-Level Dataset

| Split | Number of samples |
|---|---:|
| Training | 2,388 |
| Validation | 300 |
| Testing | 289 |
| **Total** | **2,977** |

The resulting datasets provide paired reference imagery, geospatial conditioning layers, and textual descriptions for training and evaluating the conditional diffusion models.

## 7. Directory Contents

- `samples/district_level/`: Representative district-level imagery and conditioning examples.
- `samples/block_level/`: Representative block-level imagery and conditioning examples.

**Note:** The complete datasets are not included in this repository due to their size and data availability restrictions. This documentation describes the preparation methodology and dataset organization.
