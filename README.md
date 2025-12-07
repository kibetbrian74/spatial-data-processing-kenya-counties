# 📍 Spatial Data Analysis for Kenya Counties
This repository contains a simple but complete geospatial workflow for processing and visualizing Kenya county boundaries using GeoPandas, Matplotlib, and supporting Python libraries.

The notebook demonstrates how to:

- Load and inspect spatial data
- Clean county names
- Plot the national counties map
- Generate individual county maps
- Produce a population density visualization
- Save outputs to organized folders

This makes the project suitable for GIS beginners, data analysts, and researchers working with spatial data in Python.

## 🚀 Features

- Read spatial vector datasets using GeoPandas
- Clean attribute fields (i.e., county names)
- Visualize spatial geometries
- Export maps as PNG files
- Generate both national-level and county-level maps
- Compute and plot population density
- Organized input/output workflow

## 📂 Project Structure
```
/
├── spatial data analysis.ipynb # Main analysis notebook
├── inputs/
│   └── Counties.shp            # Kenya county boundary shapefile
├── outputs/
│   ├── counties/               # Individual county maps
└──  kenya_pop.png           # Population density map
```
In your case, you will need to create inputs/ and outputs/ folders manually.

## 🧰 Requirements
Install the necessary dependencies:
```
pip install geopandas matplotlib pandas
```
Ensure that your environment supports Fiona/GDAL (GeoPandas dependency).

## 🗺️ Workflow Overview
1. Import Libraries
```
import geopandas as gpd
import pandas as pd
import matplotlib.pyplot as plt
```
2. Load County Shapefile
```
counties = gpd.read_file("inputs/Counties.shp")
counties.head()
```
3. Clean Attribute Names
County names may include slashes, spaces, or inconsistent formatting.

A typical cleaning operation (reconstructed for correctness) was:
```
counties = counties.copy()
counties["NAME"] = (counties["NAME"].str.strip().str.replace("/", "_").str.replace(" ", "_"))
counties.NAME.unique()
```
4. Plot the Kenya Counties Map
```
fig = counties.plot(color="grey", figsize=(6, 8), edgecolor="green")
plt.title("Kenya Counties", fontweight="bold")
fig.set_axis_off()
```
5. Generate and Save Individual County Maps
```
import os
os.makedirs("outputs/counties", exist_ok=True)

for _, row in counties.iterrows():
    fig, ax = plt.subplots(figsize=(6, 6))
    gpd.GeoDataFrame([row], crs=counties.crs).plot(ax=ax, color="lightgrey", edgecolor="black")

    x, y = row.geometry.centroid.coords[0]
    ax.text(x, y, row["NAME"], ha='center', fontsize=10)

    ax.set_axis_off()
    plt.tight_layout()
    plt.savefig(f"outputs/counties/{row['NAME'].lower()}.png", dpi=300)
    plt.close(fig)
```
6. Population Density Visualization

Population column existed in the data:
```
fig, ax = plt.subplots(figsize=(10, 10))

counties.plot(column="POP_DENSITY", ax=ax, legend=True, edgecolor="black")

for _, row in counties.iterrows():
    x, y = row.geometry.centroid.coords[0]
    ax.text(x, y, row["NAME"], fontsize=6)
plt.title("Kenya Population Density")
plt.savefig("outputs/kenya_pop.png", dpi=300)
plt.show()
plt.close(fig)
```
## 📊 Outputs
- National county map
- Individual county maps (PNG files)
- Population density visualization

These outputs were automatically saved inside the outputs/ directory.

## 🧪 How to Run the Notebook

Clone the repository:
```
git clone https://github.com/yourusername/kenya-spatial-analysis.git
cd kenya-spatial-analysis
```
Install the required libraries.

Add the county shapefile to the inputs/ directory.

Open the notebook:
```
jupyter notebook "spatial data analysis.ipynb"
```
Run all cells.
