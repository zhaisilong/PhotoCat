# PhotoCat

## Installation

```bash
mamba create -npcreactor python=3.8
mamba activate pcreactor
pip install -r requirements.txt -i https://pypi.tuna.tsinghua.edu.cn/simple
```

## TMAP Visualization

[TMAP](work/cluster_tmap/TMAP.ipynb)

## Acknowledgements

- This is work is based on the [Biocatalysis Model]() published on [Nature Communications]()

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

- 实验步骤:
  - 创建目录 `raw` 并放入 5 个 `xlsx` 文件
  - `data/biocatalysis/pre.ipynb`
    - 数据文件格式转化: `*.xlsx` -> `data.csv`
  - `data/biocatalysis/get_data.ipynb`
    - 反应格式转化, 得到 `data_biocatalysis.csv`
  - Rule Filter: Copy `molecules.txt` and `patterns.txt` to `data/biocatalysisV4`
  - Encode SMILES and Build Vocabulary:
    - `bash cv.sh`: 得到 5 折 CV 的反应文本数据
    - `pre.sh i j`: i level, j fold
    - Output Directory: `preprocessing` with suffix(EC level and fold)
  - 训练: `train.sh i j`
    - Evaluation and Get Results:
      - `after.sh i j`
      - 输出目录: out
  - 结果分析: `plot_results.ipynb`
    - 图片输出在: `imgs`
