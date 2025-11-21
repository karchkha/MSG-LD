# Simultaneous Music Separation and Generation Using Multi-Track Latent Diffusion Models

This is the official repository for: [Simultaneous Music Separation and Generation Using Multi-Track Latent Diffusion Models](https://arxiv.org/pdf/2409.12346).

The paper is published at **ICASSP 2025**.

Diffusion models have recently shown strong potential in both music generation and music source separation tasks. Although in early stages, a trend is emerging towards integrating these tasks into a single framework, as both involve generating musically aligned parts and can be seen as facets of the same generative process. In this work, we introduce a latent diffusion-based multi-track generation model capable of both source separation and multi-track music synthesis by learning the joint probability distribution of tracks sharing a musical context. Our model also enables arrangement generation by creating any subset of tracks given the others. We trained our model on the Slakh2100 dataset, compared it with an existing simultaneous generation and separation model, and observed significant improvements across objective metrics for source separation, music, and arrangement generation tasks. 

Sound examples are available at https://msg-ld.github.io/

# Installation

To install MSG-LD, follow these steps:

Clone the repository to your local machine
```bash
$ git clone https://github.com/karchkha/MSG-LD
```

To run the code in this repository, you will need python 3.9 

Navigate to the project directory and install the required dependencies

If you already installed conda before, skip this step, otherwise:
```
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
bash Miniconda3-latest-Linux-x86_64.sh
```
Then, after installing you should make sure your conda environment is running on your bash


```
conda env create -f musicldm_env.yml
``` 


then 
```
conda activate musicldm_env
```


# Data

In this project, the Slakh2100 data is used.

Please follow the instructions for data download and set up given here:

https://github.com/gladia-research-group/multi-source-diffusion-models/blob/main/data/README.md

# Pre-trained Components

After data and conda environment are installed properly, you will need to download components of MusicLDM that are used for MSG-LD. Create the directory for checkpoints and download the required files:

```bash
# Create directory for MusicLDM checkpoints
mkdir -p lightning_logs/musicldm_checkpoints

# Download HiFiGAN vocoder checkpoint
wget -O lightning_logs/musicldm_checkpoints/hifigan-ckpt.ckpt https://zenodo.org/record/10643148/files/hifigan-ckpt.ckpt

# Download VAE checkpoint
wget -O lightning_logs/musicldm_checkpoints/vae-ckpt.ckpt https://zenodo.org/record/10643148/files/vae-ckpt.ckpt
```

# Training MSG-LD

After placing the checkpoints in the correct directory, you can start training MSG-LD:

```bash
python train_musicldm.py --config config/MSG-LD/multichannel_musicldm_slakh_3d_train.yaml
```

# Pre-trained Model Checkpoints

We provide two pre-trained model checkpoints on Zenodo:

## Standard Model (for Separation and Total Generation)
- **Model**: 128 channels, trained for ~72k steps
- **Zenodo**: https://zenodo.org/records/15123184
- **File**: `2024-05-23T09-28-56_3_D_4_stems_slakh_mix_cond_sumch_3e-05_zero_unconditional_checkpoint.pt`
- **Used with**: `config/MSG-LD/multichannel_musicldm_slakh_3d_eval.yaml`

```bash
# Download standard model
wget https://zenodo.org/records/15123184/files/2024-05-23T09-28-56_3_D_4_stems_slakh_mix_cond_sumch_3e-05_zero_unconditional_checkpoint.pt

```

## Inpainting Model (for Arrangement Generation)
- **Model**: 192 channels, trained for ~657k steps
- **Zenodo**: https://zenodo.org/records/15123213
- **File**: `2024-05-30T21-00-48_3_D_4_stems_slakh_mix_cond_sumch_ch_192_3e-05_zero_unconditional_checkpoint.pt`
- **Used with**: `config/MSG-LD/multichannel_musicldm_slakh_3d_eval_inpaint.yaml`

```bash
# Download inpainting model
wget https://zenodo.org/records/15123213/files/2024-05-30T21-00-48_3_D_4_stems_slakh_mix_cond_sumch_ch_192_3e-05_zero_unconditional_checkpoint.pt

```

# Inference

## Separation and Total Generation

For **separation** and **total generation** tasks, use the standard model:

```bash
python train_musicldm.py --config config/MSG-LD/multichannel_musicldm_slakh_3d_eval.yaml
```

Adjust the `unconditional_guidance_scale` parameter in the config file:
- Set `unconditional_guidance_scale: 0` for **total generation** (unconditional mode)
- Set `unconditional_guidance_scale: 1` or `2` for **separation** (conditional generation)

## Arrangement Generation

For **arrangement generation** (inpainting specific instruments), use the inpainting model:

```bash
python train_musicldm.py --config config/MSG-LD/multichannel_musicldm_slakh_3d_eval_inpaint.yaml
```

Specify the instrument(s) you want to generate in the `stems_to_inpaint` parameter in the config file. Available stems: `bass`, `drums`, `guitar`, `piano`.

## Output Location

Generated audio files will be saved in the lightning logs directory under your experiment folder. Look for outputs in:
```
lightning_logs/<project_name>/<timestamp>_<experiment_name>/
```

# Citation

If you use this code in your research, please cite our paper:

```bibtex
@inproceedings{karchkhadze2025simultaneous,
  title={Simultaneous Music Separation and Generation Using Multi-Track Latent Diffusion Models},
  author={Karchkhadze, Tornike and Beguš, Gašper},
  booktitle={ICASSP 2025-2025 IEEE International Conference on Acoustics, Speech and Signal Processing (ICASSP)},
  year={2025},
  organization={IEEE}
}
```

# Acknowledgments

This work builds upon [MusicLDM](https://github.com/RetroCirce/MusicLDM) and uses pre-trained VAE and HiFiGAN components from the MusicLDM project.

# License

Please refer to the LICENSE file for details on the terms of use.

# Contact

For questions or issues, please open an issue on the [GitHub repository](https://github.com/karchkha/MSG-LD) or contact the authors.

