# <img src="assets/logo.png" width="35" style="vertical-align: middle"> WorldCam: Interactive Autoregressive 3D Gaming Worlds with Camera Pose as a Unifying Geometric Representation

<div align="center">
  <a href="https://arxiv.org/abs/2603.16871"><img src="https://img.shields.io/badge/ArXiv-2603.16871-red"></a>
  <a href="https://cvlab-kaist.github.io/WorldCam/"><img src="https://img.shields.io/static/v1?label=Project%20Page&message=Web&color=green"></a>
  <a href="https://huggingface.co/worldcam/worldcam"><img src="https://img.shields.io/static/v1?label=HuggingFace&message=Weights&color=yellow"></a>
  <a href="https://huggingface.co/datasets/worldcam/worldcam-dataset"><img src="https://img.shields.io/static/v1?label=HuggingFace&message=Dataset&color=yellow"></a>
</div>

---

## Installation

Tested with **Python 3.10**, **PyTorch 2.9**, and a single **NVIDIA H100 (80 GB)** GPU.

```bash
git clone https://github.com/cvlab-kaist/WorldCam.git
cd WorldCam

conda create -n worldcam python=3.10 -y
conda activate worldcam

pip install torch torchvision --index-url https://download.pytorch.org/whl/cu121
pip install -r requirements.txt
```

---

## Download Pretrained Weights

```bash
pip install huggingface_hub
hf download worldcam/worldcam --local-dir weights/ --include "*.safetensors" --repo-type model
```

This downloads the fine-tuned WorldCam DiT (~3 GB) into `weights/`. The model was trained on CS:GO gameplay.

The base Wan2.1-T2V-1.3B weights (text encoder, VAE, base DiT, tokenizer) are pulled automatically the first time you run `inference.py`.

---

## Quick Start

```bash
python inference.py
```

Inference settings can be adjusted in the configuration block at the top of `inference.py`.

---

## Download Dataset

We release gameplay recordings from two open-source games for research use:

| Game | License | Folder |
| --- | --- | --- |
| [Xonotic](https://xonotic.org) | GPL v3 | `data_1/` |
| [Unvanquished](https://unvanquished.net) | CC BY-SA 2.5 | `data_2/` |

```bash
# Download all
hf download worldcam/worldcam-dataset --local-dir data/ --repo-type dataset

# Download only Xonotic (data_1)
hf download worldcam/worldcam-dataset --local-dir data/ --repo-type dataset --include "data_1/*"
```

Each recording consists of:
- **`video_*.mp4`** — raw gameplay footage
- **`input_*.txt`** — recorded player actions: keyboard (`W`, `A`, `S`, `D`, `Shift`, `Space` as booleans) and mouse movement (`dx`, `dy` as relative pixel deltas)

> **Note:** These are raw recorded actions, not used in the paper. You can use these raw recorded actions for your own research. Camera poses and captions are not included — you can extract them using off-the-shelf models such as [ViPE](https://github.com/nv-tlabs/vipe) / [DA3](https://github.com/ByteDance-Seed/depth-anything-3) for camera poses and [Qwen2.5-VL-7B](https://huggingface.co/Qwen/Qwen2.5-VL-7B-Instruct) for captions.

### Action Overlay Visualization

To visualize the recorded actions overlaid on video:

```bash
python overlay_actions.py \
    --video data/data_1/batch_001/video_20250926_193542.mp4 \
    --log data/data_1/batch_001/input_20250926_193542.txt \
    --output overlay_demo.mp4
```

---

## Citation

```bibtex
@article{nam2026worldcam,
  title={WorldCam: Interactive Autoregressive 3D Gaming Worlds with Camera Pose as a Unifying Geometric Representation},
  author={Nam, Jisu and Hong, Yicong and Huang, Chun-Hao Paul and Liu, Feng and Lee, JoungBin and Kim, Jiyoung and Jin, Siyoon and Lee, Yunsung and Jung, Jaeyoon and Choi, Suhwan and others},
  journal={arXiv preprint arXiv:2603.16871},
  year={2026}
}
```

---

## Acknowledgements

- [Wan2.1-T2V-1.3B](https://github.com/Wan-Video/Wan2.1) (base video backbone)
- [DiffSynth-Studio](https://github.com/modelscope/DiffSynth-Studio) (pipeline framework)
- Game data from [Xonotic](https://xonotic.org) (GPL v3) and [Unvanquished](https://unvanquished.net) (CC BY-SA 2.5)
