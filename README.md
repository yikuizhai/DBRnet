# DBRNet

DBRNet is a PyTorch-based framework for industrial image anomaly detection and segmentation. It supports the HD, MVTec AD, and VisA datasets.

DBRNet consists of the following three modules:

- `DBRF`: Dual-Branch Representation Fusion.
- `LGFB`: Local-Global Feature Blending.
- `LDRD`: Lightweight Detail Recovery Decoder.

The main model is implemented as the `DBRNet` class in `model/DBRNet.py`.

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
