% DAR README (English LaTeX version)
\section*{DAR}

DAR is a PyTorch-based implementation for industrial image anomaly detection and segmentation. It supports the HD, MVTec AD, and VisA datasets. The model is named \texttt{DAR} and consists of the following three modules:

\begin{itemize}
    \item \texttt{MDAR}: Multi-Level Dual Anomaly Representation.
    \item \texttt{LGFB}: Local-Global Feature Blending.
    \item \texttt{LDRD}: Lightweight Detail Recovery Decoder.
\end{itemize}

The model entry is the \texttt{DAR} class implemented in \texttt{model/DAR.py}.

\section*{Project Structure}

\begin{verbatim}
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
\end{verbatim}

\section*{Installation}

The current experimental environment is configured as follows:

\begin{itemize}
    \item Python 3.13.5
    \item PyTorch 2.6.0
    \item torchvision 0.21.0
    \item CUDA 12.4
\end{itemize}

Install the required dependencies with:

\begin{verbatim}
python -m pip install -r requirements.txt
\end{verbatim}

CUDA is required for training, while evaluation can also be performed on CPU. The teacher network uses a pretrained ResNet18. During the first run, the corresponding pretrained weights must either be available in the local cache or be downloaded through an Internet connection.

\section*{Datasets}

The datasets need to be prepared manually. The default directory structure is shown below, although custom paths can also be specified through command-line arguments:

\begin{verbatim}
datasets/
├── HD_data/
├── mvtec/
├── VisA/
└── dtd/images/<texture_category>/*.jpg
\end{verbatim}

For HD and MVTec AD, each category is expected to follow the directory structure below:

\begin{verbatim}
<dataset>/<category>/
├── train/good/*.png
├── test/good/*.png
├── test/<defect_type>/*.png
└── ground_truth/<defect_type>/*_mask.png
\end{verbatim}

For VisA, the loader supports the following structure:

\begin{verbatim}
<category>/Data/Images/Normal/
<category>/Data/Images/Anomaly/
<category>/Data/Masks/Anomaly/
\end{verbatim}

The current VisA loader uses all normal images for training and uses both normal and anomalous images for testing. The official CSV split is not used. DTD textures are employed for synthetic anomaly augmentation.

\section*{Training}

Run the following command in the project root directory:

\begin{verbatim}
python train_model.py --dataset hd \
  --hd_path /path/to/HD_data \
  --dtd_path /path/to/dtd/images \
  --gpu_id 0 --num_workers 4
\end{verbatim}

The default random seed is \texttt{43}, and the default number of training steps is \texttt{5000}. Evaluation is automatically performed after training. Use \texttt{--no_auto_test} to disable automatic evaluation.

To train only a specified category, use:

\begin{verbatim}
python train_model.py --dataset hd \
  --hd_path /path/to/HD_data --dtd_path /path/to/dtd/images \
  --custom_training_category --no_rotation_category 06_front_negative
\end{verbatim}

For MVTec AD, use:

\begin{verbatim}
--dataset mvtec --mvtec_path /path/to/mvtec
\end{verbatim}

For VisA, use:

\begin{verbatim}
--dataset visa --visa_path /path/to/VisA
\end{verbatim}

\section*{Evaluation}

Run:

\begin{verbatim}
python test_model.py --dataset hd \
  --hd_path /path/to/HD_data \
  --checkpoint_path outputs/hd/checkpoints \
  --base_model_name DAR_hd_5000_
\end{verbatim}

Use \texttt{--category 06_front_negative} to evaluate a specified category. By default, all categories are evaluated. Categories with missing checkpoints are skipped, and the reported mean values are computed only over successfully evaluated categories.

The checkpoint parameters must exactly match the current model architecture and parameter names because strict loading is used during evaluation.

The generated checkpoints, logs, and results are stored in:

\begin{verbatim}
outputs/<dataset>/checkpoints/
logs/
results/
\end{verbatim}

These files are excluded from the repository. Checkpoints are named according to the following format:

\begin{verbatim}
<run_name_head>_<steps>_<category>.pckl
\end{verbatim}

\section*{Command-Line Options}

For the complete list of available arguments, run:

\begin{verbatim}
python train_model.py --help
python test_model.py --help
\end{verbatim}
