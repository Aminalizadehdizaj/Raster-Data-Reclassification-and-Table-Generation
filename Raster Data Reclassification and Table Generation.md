# Raster Data Reclassification and Table Generation

*_This Python script enables the reclassification of raster values into groups and generates statistical tables for geospatial analysis._*


```python
import os
import rasterio
import numpy as np
import pandas as pd

# Function to reclassify raster values into 8 groups and save the table
def reclassify_and_save_table(raster_path, output_path):
    with rasterio.open(raster_path) as src:
        # Read raster data
        raster_data = src.read(1, masked=True)  # Assuming a single band raster
        
        # Get transform data to calculate area covered by a single pixel
        transform = src.transform
        pixel_area = abs(transform[0] * transform[4])  # Width * Height
        
        # Flatten the data to work with numpy operations
        flat_data = raster_data.flatten()
        
        # Reclassify into 8 groups
        grouped_values = np.linspace(flat_data.min(), flat_data.max(), num=9)
        grouped_raster = np.digitize(flat_data, grouped_values)
        
        # Calculate frequency counts for each group
        unique, counts = np.unique(grouped_raster, return_counts=True)
        
        
        # Calculate group ranges without decimals
        group_ranges = [
            f"{int(grouped_values[i]):d} - {int(grouped_values[i + 1]):d}" for i in range(len(grouped_values) - 1)
        ]
        
        # Calculate area covered by each group in square kilometers
        areas_covered = counts * pixel_area * 1e-6  # Convert to square kilometers
        
        # Calculate cumulative areas
        cumulative_areas = np.cumsum(areas_covered)
              
        # Ensure all arrays have the same length
        min_length = min(len(unique), len(counts), len(group_ranges), len(areas_covered), len(cumulative_areas))
        unique = unique[:min_length]
        counts = counts[:min_length]
        group_ranges = group_ranges[:min_length]
        areas_covered = areas_covered[:min_length]
        cumulative_areas = cumulative_areas[:min_length]
        
        # Create a table with group, frequency counts, ranges, area covered, and cumulative areas
        table_data = {
            'Group': unique,
            'Range': group_ranges,
            'Frequency': counts,
            'Area_covered_sqkm': areas_covered,
            'Cumulative_area_sqkm': cumulative_areas,
        }
        table = pd.DataFrame(table_data)
        
        # Save the table to the specified output path
        output_filename = os.path.splitext(os.path.basename(raster_path))[0] + "_reclassified_table.csv"
        output_file = os.path.join(output_path, output_filename)
        table.to_csv(output_file, index=False)
        
        return output_file  # Return the path where the table is saved

# Directory containing raster files
directory = r'G:\python\python (1)\python\raster_files'
output_directory = rf'G:\python\python (1)\python\output'

# Iterate through all raster files in the directory
for filename in os.listdir(directory):
    if filename.endswith('.tif'):  # Adjust the extension if your files have a different format
        raster_file = os.path.join(directory, filename)
        
        # Generate table for each raster file and save it to the output directory
        saved_table_path = reclassify_and_save_table(raster_file, output_directory)
        
        # Display the path where the table is saved for each raster file
        print(f"Table for {filename} saved at: {saved_table_path}")
```
