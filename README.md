# LAC-ST-TransUNet

Official model-code repository accompanying the manuscript:

**Land-atmosphere causal learning enables process-aware flash-drought forecasting**

This repository provides the deep-learning model implementations used in the study. The public release is intentionally limited to model architecture and training code. Data-processing scripts, causal-analysis workflows, datasets, trained model weights, prediction outputs, evaluation results, and manuscript figures are not included.

## Repository scope

The notebooks retain their original filenames used during model development. Their correspondence to the model names used in the manuscript is listed below.

| File | Corresponding implementation |
| --- | --- |
| `code/骤旱预测-ST-TransUNet.ipynb` | Main ST-TransUNet implementation. The same notebook contains the causality-guided LAC-ST-TransUNet configuration through the PCMCI-prior options. With `pcmci_mode='none'`, causal-prior embedding is disabled and the model corresponds to ST-TransUNet. The causal-prior modes implement the LAC-ST-TransUNet extension described in the manuscript. |
| `code/骤旱预测-TransUNet.ipynb` | TransUNet benchmark implementation. |
| `code/骤旱预测-CNN-LSTM&CNN-Transformer.ipynb` | CNN-LSTM and CNN-Transformer benchmark implementations. |
| `code/骤旱预测-UNet.ipynb` | U-Net implementation retained as an additional model reference. |

The original notebook filenames are preserved to maintain consistency with the authors' development environment.

## Main LAC-ST-TransUNet implementation

The main model is implemented in:

`code/骤旱预测-ST-TransUNet.ipynb`

The principal function is:

```python
Auto_ST_TransUnet_pytorch(...)
```

The implementation supports standard ST-TransUNet and causality-guided configurations through the `pcmci_mode` argument. In particular:

- `pcmci_mode='none'`: ST-TransUNet without PCMCI-derived causal-prior embedding.
- `pcmci_mode='m1'`: fixed causal-prior addition used as an auxiliary implementation option.
- `pcmci_mode='m4'` and compatible local causal-prior modes: gated local cross-attention fusion of PCMCI-derived causal information, corresponding to the causality-guided extension of ST-TransUNet.

For the causal-prior configuration, the implementation accepts PCMCI-derived MCI values together with optional q-values and significance masks. The model checks the correspondence among predictor channels, temporal lags, and spatial grid dimensions before causal-prior fusion.

The main input conventions used by this notebook are:

```text
vx:    (time, latitude, longitude, predictor)
vy:    (time, latitude, longitude)
       or (time, latitude, longitude, output_channel)
PCMCI: (event_type, predictor, lag, latitude, longitude)
```

Users should ensure that predictor ordering, lag ordering, and spatial dimensions are consistent between the prepared hydroclimatic input and the PCMCI causal-prior tensors.

## Baseline models

The repository also contains the deep-learning baselines used for model comparison in the study:

- ST-TransUNet
- TransUNet
- CNN-Transformer
- CNN-LSTM

An additional U-Net implementation is also retained in the repository.

## Installation

The notebooks are implemented in Python with PyTorch. Install the main Python dependencies with:

```bash
pip install -r requirements.txt
```

PyTorch/CUDA compatibility depends on the local hardware and CUDA environment. Users running the models on GPU should install a PyTorch build compatible with their CUDA setup.

## Usage

The notebooks provide model-definition and training functions rather than an end-to-end data-processing pipeline. A typical workflow is:

1. Prepare input and target arrays outside this repository.
2. If using LAC-ST-TransUNet, prepare the PCMCI-derived causal-prior tensors required by the model.
3. Open the corresponding notebook and execute the model-definition cell.
4. Call the model function with the prepared arrays and the desired hyperparameters.

The detailed preprocessing, causal-analysis, event-identification, and result-generation workflows used in the study are outside the scope of this public model-code release.

## Data and results

No research data are redistributed through this repository. The repository also does not contain trained checkpoints, prediction fields, evaluation tables, or manuscript figures.

The observational/reanalysis and operational forecast datasets used in the study are described in the manuscript and its Supplementary Information.

## Citation

If you use these model implementations, please cite the accompanying manuscript:

**Land-atmosphere causal learning enables process-aware flash-drought forecasting**

Formal journal citation information will be added after publication.

## License

This repository is released under the Apache License 2.0. See `LICENSE` for details.

## Contact

For questions specifically related to the released model code, please open an issue in this repository.
