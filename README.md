# CREODIAS Notebook Samples

Jupyter notebooks showing how to work with Earth observation data on [CREODIAS](https://creodias.eu), the EO data and cloud platform by [CloudFerro](https://cloudferro.com).

The notebooks read data **directly from the `/eodata` repository**, the read-only archive mounted on every CREODIAS virtual machine. Nothing needs to be downloaded in advance: open a notebook, run the cells, and the data is already there.

> **Heads-up:** these notebooks are written to run **on a CREODIAS VM with `/eodata` mounted**. Outside of CREODIAS the data paths do not exist and the notebooks will not be able to load anything.

## What's inside

| Folder | Notebook | What you will learn |
|---|---|---|
| [`geoAI-embeddings/`](geoAI-embeddings) | [`01_tessera_v10_change.ipynb`](geoAI-embeddings/01_tessera_v10_change.ipynb) | Explore **Tessera** 128-band land embeddings for a point in Europe: PCA composites per year, change detection between years, KMeans clustering. |
| | [`02_esd_embeddings.ipynb`](geoAI-embeddings/02_esd_embeddings.ipynb) | Explore global **ESD** (Embeddings Seamless Data) embeddings at 30 m, 2000-2024: tile lookup, windowed reads, PCA, change hotspots, clustering and cosine similarity ("where else looks like here?"). |
| [`geoAI-training/`](geoAI-training) | [`01_eurosat_pixels_to_predictions.ipynb`](geoAI-training/01_eurosat_pixels_to_predictions.ipynb) | A hands-on tour of **EuroSAT** (27,000 Sentinel-2 patches, 10 land cover classes). Builds Random Forest, SVM and CNN classifiers step by step and compares them. |
| | [`02_forest_height_from_space.ipynb`](geoAI-training/02_forest_height_from_space.ipynb) | Predict **canopy height** from a year of Sentinel-2 imagery using the **OLMOEarth** dataset: Random Forest vs U-Net, and why spatial leakage fools your accuracy score. |

### Data used

| Notebook | Path on `/eodata` |
|---|---|
| Tessera embeddings | `/eodata/geoAI-embeddings/Tessera/Tessera-Embeddings-v1.0` |
| ESD embeddings | `/eodata/geoAI-embeddings/Embeddings-Seamless-Data/ESD-Embeddings/SDC30_EBD_V001` |
| EuroSAT | `/eodata/geoAI-training/EuroSAT` |
| OLMOEarth | `/eodata/geoAI-training/OLMOEarth` |

The `geoAI-training` collection also hosts other ready-to-use datasets (BigEarthNet, SSL4EO, MMEarth, MajorTOM, CloudSEN12) that you can use as a starting point for your own experiments.

## Requirements

- A **CREODIAS virtual machine** with `/eodata` mounted (check with `ls /eodata`).
- Python 3 with Jupyter (JupyterLab or Notebook).
- Python packages, depending on the notebook:
  - all: `numpy`, `matplotlib`, `rasterio`, `scikit-learn`
  - embeddings: `affine`
  - training: `torch`, `tqdm`, `pandas`, `pyproj`, `folium`
- A **GPU is optional**. The training notebooks detect the hardware and automatically switch to lighter settings on CPU-only machines (EuroSAT finishes in under ten minutes on a 4-core VM). The forest-height notebook needs about 6 GB of RAM.

## Getting started

1. Log in to your CREODIAS VM and clone this repository:

   ```bash
   git clone <repository-url>
   cd notebook-samples
   ```

2. Install the dependencies (adjust to the notebooks you want to run):

   ```bash
   pip install numpy matplotlib rasterio scikit-learn affine pandas pyproj folium tqdm jupyterlab
   # PyTorch (training notebooks) - pick the build matching your hardware, e.g. for CUDA 12.8:
   pip install torch --index-url https://download.pytorch.org/whl/cu128
   ```

3. Start Jupyter and open a notebook:

   ```bash
   jupyter lab
   ```

4. For the embedding notebooks, set `AOI_LAT`, `AOI_LON` and `YEARS` in the configuration cell, then run all cells.

## Good to know

- **`/eodata` is backed by object storage** and can occasionally return I/O errors or briefly stop responding. The notebooks retry reads where it matters; if a run fails with an `/eodata` error, wait a moment and re-run the cell.
- **Training notebooks copy a small sample** of the data (a few hundred MB) into a local `geoai_data` folder next to the notebook, because opening thousands of small files on `/eodata` is slow. Later runs reuse the copy. The folder is safe to delete.
- Some visualisations (e.g. the interactive map in the forest-height notebook) load OpenStreetMap tiles in your browser, so they need internet access on the machine where you view the notebook.

## About CREODIAS

[CREODIAS](https://creodias.eu) provides cloud computing next to a petabyte-scale archive of Earth observation data (Copernicus Sentinel missions and more), so you can process data where it is stored instead of moving it around. Learn more at [creodias.eu](https://creodias.eu) and [cloudferro.com](https://cloudferro.com).

## License

Released under the [MIT License](LICENSE).
