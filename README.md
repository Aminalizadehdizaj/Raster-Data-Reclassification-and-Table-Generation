# Raster-Data-Reclassification-and-Table-Generation
**The project serves as both a tool for daily workflow enhancement and a personal development endeavor.**
## Overview
The script simplifies the process of creating statistical tables for multiple raster files by performing the following:

* **Reclassification:** Values are grouped into 8 predefined categories (modifiable) based on specified ranges (equal and constant intervals).
* **Statistics Generation:** Frequency counts, area coverage and cumulative areas are calculated for each group.
* **CSV Export:** Tables are automatically generated and saved as CSV files for each raster file in the specified output directory.
## Advantage
This script offers a significant advantage over manual methods:
* **Workflow Enhancement:** Prior to this script, creating tables for each raster file (especially from multiple files) involved manual processes using ArcGIS/QGIS and Excel.
* **Automated Statistics:** The script streamlines this process, automatically creating tables including grouped ranges, frequency counts, area coverage in each range, and cumulative areas for efficient and consistent analysis.
## Usage
### Requirements
* Python 3.x  
* Libraries: **rasterio**, **numpy**, **pandas**
### Steps  
**1. Folder Structure:**  
- Place raster files in the directory specified by directory.  
- Set the output folder (output_directory) to store the generated CSV files.
  
**2. Running the Script:**  
- Execute the script.  
- Statistics tables will be created for each raster file and saved as CSV files in the output directory.
