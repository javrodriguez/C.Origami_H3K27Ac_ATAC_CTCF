# C.Origami with H3K27Ac

This is a modified version of C.Origami that includes H3K27Ac as an additional genomic feature for improved chromatin architecture prediction. This is NOT the official C.Origami implementation.

For the original C.Origami implementation, please visit: [https://github.com/tanjimin/C.Origami](https://github.com/tanjimin/C.Origami)

## Modifications

This version adds H3K27Ac ChIP-seq data as an additional input feature alongside the original CTCF ChIP-seq and ATAC-seq data. The model architecture has been modified to incorporate this new feature, which can provide additional information about active regulatory regions.

## Dependencies and Installation

# Download C.Origami repo
module load git
git clone https://github.com/javrodriguez/C.Origami_vPlusH3K27Ac.git
cd C.Origami

# Create environment
conda create -n corigami_dev python==3.9 pytorch==1.12.0 torchvision==0.13.0 pytorch-cuda=11.8 pandas==1.3.0 matplotlib==3.3.2 pybigwig==0.3.18 omegaconf==2.1.1 tqdm==4.64.0 pytorch-lightning=1.9 scikit-image lightning-bolts mkl==2024.0 -c pytorch -c nvidia

conda activate corigami_vPlusH3K27Ac

# Install C.Origami
pip install -e .

## Data Requirements

The data directory should be structured as follows:

```
root
└── hg38
    ├── centrotelo.bed
    ├── dna_sequence
    │   ├── chr1.fa.gz
    │   ├── chr2.fa.gz
    │   └── ...
    └── CELL_TYPE
        ├── genomic_features
        │   ├── atac.bw
        │   ├── ctcf_log2fc.bw
        │   └── h3k27ac.bw
        └── hic_matrix
            ├── chr1.npz
            ├── chr2.npz
            └── ...
```

## Training

To train the model with H3K27Ac data:

```bash
python -m corigami.train \
    --save_path path/to/save/model \
    --data-root path/to/data \
    --assembly hg38 \
    --celltype CELL_TYPE \
    --max-epochs 200 \
    --batch-size 4 \
    --num-gpu 4
```

## Inference

### Prediction

```bash
python -m corigami.inference.prediction \
    --out outputs \
    --celltype CELL_TYPE \
    --chr chr1 \
    --start 1000000 \
    --model path/to/model/checkpoint.pt \
    --seq path/to/sequence/files \
    --ctcf path/to/ctcf/files \
    --atac path/to/atac/files \
    --hkac path/to/h3k27ac/files
```

### Screening

```bash
python -m corigami.inference.screening \
    --out outputs \
    --celltype CELL_TYPE \
    --chr chr1 \
    --screen-start 1000000 \
    --screen-end 2000000 \
    --perturb-width 1000 \
    --step-size 1000 \
    --model path/to/model/checkpoint.pt \
    --seq path/to/sequence/files \
    --ctcf path/to/ctcf/files \
    --atac path/to/atac/files \
    --hkac path/to/h3k27ac/files
```

### Editing/Perturbation

```bash
python -m corigami.inference.editing \
    --out outputs \
    --celltype CELL_TYPE \
    --chr chr1 \
    --start 1000000 \
    --del-start 1500000 \
    --del-width 1000 \
    --model path/to/model/checkpoint.pt \
    --seq path/to/sequence/files \
    --ctcf path/to/ctcf/files \
    --atac path/to/atac/files \
    --hkac path/to/h3k27ac/files
```

## Citation

If you use this modified version of C.Origami in your project, please cite both:

1. The original C.Origami paper:
```BibTeX
@article{tan2023nbt,
    title = {Cell-type-specific prediction of 3D chromatin organization enables high-throughput in silico genetic screening},
    author = {Tan, Jimin and Shenker-Tauris, Nina and Rodriguez-Hernaez, Javier and Wang, Eric and Sakellaropoulos, Theodore and Boccalatte, Francesco and Thandapani, Palaniraja and Skok, Jane and Aifantis, Iannis and Feny{\"o}, David and Xia, Bo and Tsirigos, Aristotelis},
    journal = {Nature Biotechnology},
    date = {2023/01/09},
    doi = {10.1038/s41587-022-01612-8},
    id = {Tan2023},
    isbn = {1546-1696},
    url = {https://doi.org/10.1038/s41587-022-01612-8},
    year = {2023},
    publisher = {Nature Publishing Group}}
```

2. This modified version:
```BibTeX
@software{corigami_h3k27ac,
    author = {Rodriguez, Javier},
    title = {C.Origami with H3K27Ac},
    year = {2024},
    url = {https://github.com/javrodriguez/C.Origami_vPlusH3K27Ac}
}
```
