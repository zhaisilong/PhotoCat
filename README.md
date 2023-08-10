# PhotoCat

## Installation

```bash
mamba create -npcreactor python=3.8
mamba activate pcreactor
pip install -r requirements.txt -i https://pypi.tuna.tsinghua.edu.cn/simple
```

## Usage

### Data Preprocessing

- Here we set catalysts type as the same to Enzyme Encoding
  - For Example: `catalyst: fac-Ir(ppy)3` -> `cat_class: 1` -> `encoding: 1.0.0.0` -> `level 1: [v1]`

- And we washed our PC-Dataset with the following steps
  - 1 Preprocess: `data/light/pre.ipynb`
    - 1.1 Data Format: `xlsx` -> `csv`
    - 1.2 Numerical catalysts
    - 1.3 Canonization
    - 1.4 Show data distributions
    - 1.5 Output: `data.csv`

- Environment: `conda activate rxn-biocatalysis-tools`
- Format Adapted: `data/biocatalysis/get_data.ipynb`
- Rule Filter: Copy `molecules.txt` and `patterns.txt` to `data/light/biocatalysis`
- Preprocess With Smiles: `preprocess.sh`
  - Set **level to 1**, because we have only catalysts class
  - Set **output directory** as data directory

### Forward with catalysts classes

- Build Vocabulary: `build-forwards.sh`
  - Save in directory `preprocessing` with **suffix**
- Train: `train.sh`
- Evaluation: `eval.sh` -> `tgt-pred.txt`

### Backward

- Build Vocabulary: `build-backwards.sh`
- Train: `train-backwards.sh`
- Evaluation: `eval-backwards.sh` -> `src-pred.txt`

### Roundtrip Prediction

- After Training on Both Forward and Backward
- Run `eval-rtrp.sh` -> `tgt-pred-rtrp.txt`

### Show Results

- `rbt-evaluate.sh` -> `all_results_top*.csv`
  - Forward
  - Backward
  - Roundtrip
  - EC type

### Detailed Steps

Steps

1. `data/biocatalysis/pre.ipynb`: Tranform data file format from `*.xlsx` to `data.csv`
2. `data/biocatalysis/get_data.ipynb`: Transform reaction format and get `data_biocatalysis.csv`
3. Rule Filter: Copy `molecules.txt` and `patterns.txt` to `data/biocatalysisV4`
4. Encode SMILES and Build Vocabulary:
  - `bash cv.sh`: Prepare CV datasets
  - `pre.sh i j`: i level, j fold
  - Output Directory: `preprocessing` with suffix(EC level and fold)
5. Training: `train.sh i j`
6. Evaluation and Get Results: `after.sh i j` -> out/
7. Analyze results: `plot_results.ipynb` -> `imgs`

## TMAP Visualization

[TMAP](work/cluster_tmap/TMAP.ipynb)

## Acknowledgements

- This is work is based on the [Biocatalysis Model]() published on [Nature Communications]()