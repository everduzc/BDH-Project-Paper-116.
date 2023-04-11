# CSE 6250 - Recreation of: Adapting Pre-trained Vision Transformers from 2D to 3D through Weight Inflation Improves Medical Image Segmentation

## Getting Started

### Installation

Please install dependencies by

```bash
conda env create -f environment.yml
```

### Dataset and Model

- The datasets used are **[BCV](https://www.synapse.org/\#!Synapse:syn3193805/wiki/217789)**, **[ACDC](https://www.creatis.insa-lyon.fr/Challenge/acdc/)**, **[MSD](https://drive.google.com/file/d/1jzeNU1EKnK81PyTsrx0ujfNl-t0Jo8uE/view?usp=sharing)** Register and download these datasets.
- Decompress each dataset and move the decompressed files to the corresponding`src/data/` folder (e.g., move **BCV** to `src/data/bcv/`)
- Run the pre-processing script *split_data_to_slices_nii.py* in each folder to generate processed data. (e.g., **BCV** will be processed to `src/data/bcv/processed/`).
- Run the weight downloading script *download_weights.sh* in the `src/backbones/encoders/pretrained_models/`folder.

### Training

1. Run the following line to train the best segmentation model with both transfer learning and depth information:

```bash
cd src/
bash scripts/train_[dataset].sh
```

All the hardware requirements for training such as number of GPUs, CPUs, RAMs are listed in each script.

To change the different encoder, set `--encoder swint/videoswint/dino`.

To adjust the number of training steps, modify `--max_steps`.

2. To train the segmentation model with only transfer learning and without depth information (i.e., **Ours w/o D** in the paper), simply add `--force_2d 1` and run:

```bash
cd src/
# add `--force_2d 1` at the end of the script
bash scripts/train_[dataset].sh
```

3. To train the segmentation model without transfer learning or depth information (i.e., **Ours w/o T&D** in the paper), simply add `--use_pretrained 0` and run:

```bash
cd src/
# add both `--force_2d 1` and `--use_pretrained 0` at the end of the script
bash scripts/train_[dataset].sh
```

Results are displayed at the end of training and can also be found at `wandb/` (open with `wandb` web or local client).

Model files are saved in `MedicalSegmentation/` folder. 

```
### Evaluation

To evaluate any trained segmentation model, simply add `--evaluation 1` and `--model_path <path/to/checkpoint>` and run:

```bash
cd src/
# add `--evaluation 1` and `--model_path <path/to/checkpoint>` at the end of the script
bash scripts/train_[dataset].sh
```

Results are displayed at the end of evaluation and can also be found at `wandb/` (open with `wandb` web or local client).

The predictions are saved in `dumps/` (open with `pickle.Unpickler`) .

```
