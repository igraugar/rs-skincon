# Concept inconsistency in the SkinCon dataset

This repository holds the code and data of the article *A mathematical analysis of concept inconsistency in the SkinCon dataset* by Gonzalo Nápoles and Isel Grau. The analysis uses rough set theory to show how far the SkinCon concepts determine the malignant versus non-malignant decision. It also trains concept bottleneck models on a clean, leakage-free version of the dataset.

## Contents

| Item | Description |
| --- | --- |
| `cbm_colab.ipynb` | Google Colab notebook that cleans the images, tests the image embeddings and trains the hard, soft and hybrid concept bottleneck models |
| `inconsistency_analysis.ipynb` | Rough-set analysis, Shapley attribution, forced misses, filtering experiment and evaluation of the trained models |
| `tables_and_figures.ipynb` | The ten tables and eight figures of the article |
| `data/` | Inputs and results as CSV files |
| `README.md` | This file |

## Data

| Folder | Content |
| --- | --- |
| `data/colab` | The CSV files written by `cbm_colab.ipynb` in the run reported in the article |
| `data/analysis` | The CSV files written by `inconsistency_analysis.ipynb` |

The file `data/analysis/clean_dataset.csv` lists the 3,833 images of the clean dataset with their split, near-copy group and label. The file `data/analysis/reported_values.csv` holds every number quoted in the text of the article.

The repository contains neither images nor SkinCon annotations. The notebooks download the two annotation files from the [SkinCon website](https://skincon-dataset.github.io) to `data/skincon` when they are missing. The Fitzpatrick17k images are available through the [Fitzpatrick17k repository](https://github.com/mattgroh/fitzpatrick17k). The DDI images are available through the [DDI website](https://ddi-dataset.github.io) under a research use agreement of the Stanford AIMI Center, which does not permit redistribution.

## Reproducing the results

The two local notebooks run with Python 3.13 on a CPU in a few minutes. Start Jupyter in the root of the repository, since every path is relative to it.

```bash
pip install numpy==2.5.3 pandas==3.0.5 scipy==1.18.1 scikit-learn==1.9.1 matplotlib==3.11.2 seaborn==0.13.2 pillow==12.3.0 jupyter
```

1. `inconsistency_analysis.ipynb` downloads the SkinCon annotations, reads `data/colab`, then writes every result to `data/analysis`.
2. `tables_and_figures.ipynb` reads `data/skincon`, `data/colab` and `data/analysis`, then writes the tables to `output/tables` and the figures to `output/figures`.

With the package versions above, both notebooks reproduce the published values, tables and figures exactly. The figures use the font Liberation Sans, which the Debian and Ubuntu package `fonts-liberation` provides.

### Training the models on Google Colab

The notebook `cbm_colab.ipynb` needs a GPU runtime. It reads its inputs from a Google Drive folder, `MyDrive/skincon` by default, organized as follows.

```
skincon/
  skincon/annotations_fitzpatrick17k.csv      downloaded by the notebook when missing
  skincon/annotations_ddi.csv                 downloaded by the notebook when missing
  fitzpatrick17k/fitzpatrick17k.csv           from the Fitzpatrick17k repository
  fitzpatrick17k/images/                      Fitzpatrick17k images, named <md5hash>.jpg
  ddi/ddi_metadata.csv                        from the DDI release
  ddi/                                        DDI images
  clean_dataset.csv                           from data/analysis, optional
```

With `clean_dataset.csv` in place, the notebook trains on exactly the images and splits of the article. Without it, the notebook rebuilds the clean dataset from the images. The rebuilt dataset may then differ slightly, because the embeddings vary across GPUs and library versions. Training on a GPU is not deterministic either, so retrained models match the published ones only up to run-to-run variation. The notebook writes its CSV files to `MyDrive/skincon/results`, which replaces `data/colab` for a new analysis.

## Citation

```bibtex
@article{napoles2026skincon,
  title   = {A mathematical analysis of concept inconsistency in the {SkinCon} dataset},
  author  = {N{\'a}poles, Gonzalo and Grau, Isel},
  journal = {Scientific Data},
  note    = {Under review},
  year    = {2026}
}
```

Please also cite SkinCon (Daneshjou et al., NeurIPS 2022), Fitzpatrick17k (Groh et al., CVPR Workshops 2021) and DDI (Daneshjou et al., Science Advances 2022) when you use their data.

## License

The code in this repository, including the notebooks, is licensed under the MIT License; see [`LICENSE`](LICENSE). This license does not cover the data files or third-party material. The SkinCon annotations and source images remain subject to the terms of their original releases.
