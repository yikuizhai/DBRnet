# DAR

DAR is a PyTorch-based implementation for industrial image anomaly detection and segmentation. It supports the HD, MVTec AD, and VisA datasets.

The model is named `DAR` and consists of the following three modules:

- `MDAR`: Multi-Level Dual Anomaly Representation.
- `LGFB`: Local-Global Feature Blending.
- `LDRD`: Lightweight Detail Recovery Decoder.

The model entry is the `DAR` class implemented in `model/DAR.py`.

## Project Structure

```text
.
├── README.md
├── requirements.txt
├── pyproject.toml
├── train_model.py
├── test_model.py
├── constant.py
├── paths.py
├── reproducibility.py
├── data/
└── model/
