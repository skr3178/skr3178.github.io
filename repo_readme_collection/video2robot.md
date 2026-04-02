# video2robot

End-to-end pipeline: Video (or Prompt) → Human Pose Extraction → Robot Motion Conversion

## Demo

<p align="center">
<video src="https://github.com/user-attachments/assets/a0f1bfb1-7e06-4672-8f6a-320ab60b0bfe" width="800" controls></video>
</p>
<p align="center"><b>Demo Video</b></p>

<table>
<tr>
<td align="center" width="50%">
<video src="https://github.com/user-attachments/assets/1d58bac8-173c-499d-b245-65013371d50f" width="400" controls></video>
<br><b>Backflip</b>
</td>
<td align="center" width="50%">
<video src="https://github.com/user-attachments/assets/94e6d12d-afae-4300-8c5c-c244ad208bdb" width="400" controls></video>
<br><b>Dance Motion</b>
</td>
</tr>
</table>

## Pipeline

```
[Prompt] → Veo → [Video] → PromptHMR → [SMPL-X] → GMR → [Robot Motion]
```

## Project Structure

```
video2robot/
├── video2robot/            # Main package
│   ├── config.py           # Configuration management
│   ├── pipeline.py         # (Optional) Python API pipeline
│   ├── cli.py              # Console entrypoint for installation
│   ├── video/              # Video generation/processing
│   │   └── veo_client.py   # Google Veo API
│   ├── pose/               # Pose extraction (PromptHMR wrapper)
│   │   └── extractor.py
│   └── robot/              # Robot conversion (GMR wrapper)
│       └── retargeter.py
│
├── scripts/                # CLI scripts
│   ├── run_pipeline.py     # Full pipeline
│   ├── generate_video.py   # Veo video generation
│   ├── extract_pose.py     # Pose extraction
│   └── convert_to_robot.py # Robot conversion
│   └── visualize.py        # Result visualization
│
├── configs/                # Configuration files
├── data/                   # Data (gitignored)
│
└── third_party/            # External dependencies (submodules)
    ├── PromptHMR/          # Pose extraction model
    └── GMR/                # Motion retargeting
```

## Installation

This project requires **two conda environments**: `gmr` and `phmr`.

```bash
# Clone repo (with submodules)
git clone --recursive https://github.com/AIM-Intelligence/video2robot.git
cd video2robot

# Or initialize submodules after cloning
git submodule update --init --recursive
```

### 1. GMR Environment (Robot Retargeting)

```bash
conda create -n gmr python=3.10 -y
conda activate gmr
pip install -e .
```

For details, see [GMR README](third_party/GMR/README.md).

### 2. PromptHMR Environment (Pose Extraction)

**For other GPUs (Ampere, Hopper, etc.):**
```bash
conda create -n phmr python=3.10 -y
conda activate phmr
cd third_party/PromptHMR
pip install -e .
```

For details, see [PromptHMR README](third_party/PromptHMR/README.md).

## Data Download and Setup

After installing the environments, you need to download the required model files and data.

### Download PromptHMR Data

From the `third_party/PromptHMR` directory:

```bash
cd third_party/PromptHMR
bash scripts/fetch_data.sh
```

This script downloads:
- PromptHMR checkpoints
- Third-party checkpoints (camcalib, droidcalib, vitpose, SAM, SAM2, YOLO, Detectron2)
- Dataset annotations (for evaluation)
- Example images

**Note:** The script automatically organizes downloaded files into the correct directory structure:
- Checkpoints → `data/pretrain/`
- Annotations → `data/annotations/`
- Examples → `data/examples/`

### Download SMPL-X and SMPL Body Models

From the `third_party/PromptHMR` directory:

```bash
cd third_party/PromptHMR
bash scripts/fetch_smplx.sh
```

This script requires registration at:
- [SMPL-X](https://smpl-x.is.tue.mpg.de) - for SMPL-X models
- [SMPL](https://smpl.is.tue.mpg.de) - for SMPL models

**Reorganization Process:**

The `fetch_smplx.sh` script automatically handles extraction and reorganization:

1. **SMPL-X Models:**
   - Downloads `models_smplx_v1_1.zip`
   - Extracts to `data/body_models/smplx/`
   - Moves files from nested `models/smplx/` directory to `data/body_models/smplx/`
   - Removes temporary extraction directories and zip file
   - Final location: `third_party/PromptHMR/data/body_models/smplx/`

2. **SMPL Models:**
   - Downloads `SMPL_python_v.1.1.0.zip`
   - Extracts to `data/body_models/smpl/smpl/`
   - Moves and renames model files:
     - `basicmodel_neutral_lbs_10_207_0_v1.1.0.pkl` → `SMPL_NEUTRAL.pkl`
     - `basicmodel_f_lbs_10_207_0_v1.1.0.pkl` → `SMPL_FEMALE.pkl`
     - `basicmodel_m_lbs_10_207_0_v1.1.0.pkl` → `SMPL_MALE.pkl`
   - Removes temporary extraction directories and zip file
   - Final location: `third_party/PromptHMR/data/body_models/smpl/`

3. **Supplementary Files:**
   - Downloads accessory files (J_regressor, smplx2smpl mappings, etc.)
   - Places them in `data/body_models/accessory/` and `data/body_models/smplx/`

**Note:** If the automated download script fails or you need to download files manually, you may need to:
- Manually download `SMPL_python_v.1.1.0.zip` from the SMPL website
- Place it in the project root or extract it manually to the appropriate location
- Some files may be missing from the automated download and need to be obtained separately

**Expected Final Structure:**

```
third_party/PromptHMR/data/
├── body_models/
│   ├── smplx/
│   │   ├── SMPLX_NEUTRAL.npz
│   │   ├── SMPLX_MALE.npz
│   │   ├── SMPLX_FEMALE.npz
│   │   ├── SMPLX_NEUTRAL.pkl
│   │   ├── SMPLX_MALE.pkl
│   │   ├── SMPLX_FEMALE.pkl
│   │   └── version.txt
│   ├── smpl/
│   │   ├── SMPL_NEUTRAL.pkl
│   │   ├── SMPL_MALE.pkl
│   │   └── SMPL_FEMALE.pkl
│   └── accessory/
│       ├── J_regressor_h36m.npy
│       ├── smpl_mean_params.npz
│       ├── smplx2smpl_joints.npy
│       └── smplx2smpl.pkl
├── pretrain/
│   ├── phmr/
│   ├── phmr_vid/
│   ├── camcalib_sa_biased_l2.ckpt
│   ├── droidcalib.pth
│   ├── vitpose-h-coco_25.pth
│   ├── sam_vit_h_4b8939.pth
│   └── ...
└── annotations/
```

### Convert SMPL-X Models to Float32 (Required)

**Important:** The downloaded SMPL-X models use `float64` (double precision) which can cause compatibility issues with PyTorch and other frameworks. **You must convert them to `float32` (single precision) after downloading.** This also reduces file size by ~50%.

**Convert downloaded models to float32:**

```bash
# From the project root directory
python scripts/convert_smplx_to_float32.py \
    --input_dir third_party/PromptHMR/data/body_models/smplx \
    --output_dir third_party/PromptHMR/data/body_models/smplx
```

This script:
- Converts all `.npz` files in the input directory from `float64` to `float32`
- Converts `int64` to `int32` and `uint64` to `uint32` for consistency
- Handles object arrays and nested structures
- Creates compressed output files
- Preserves original files (backup recommended)

**Note:** If you're converting in-place (overwriting originals), consider backing up first:
```bash
# Backup before converting
cp -r third_party/PromptHMR/data/body_models/smplx third_party/PromptHMR/data/body_models/smplx_backup

# Convert in-place
python scripts/convert_smplx_to_float32.py \
    --input_dir third_party/PromptHMR/data/body_models/smplx \
    --in_place
```

### Download Locked-Head SMPL-X Models (Optional)

If you need locked-head SMPL-X models (for head-constrained motion), you'll need to download them separately:

1. **Download the locked-head models:**
   - Register and download `smplx_lockedhead_20230207.zip` from [SMPL-X website](https://smpl-x.is.tue.mpg.de)
   - Place the zip file in the project root directory

2. **Extract the models:**
   ```bash
   # From the project root directory
   unzip smplx_lockedhead_20230207.zip -d smplx_lockedhead_extracted
   ```

3. **Create slim float32 versions:**
   ```bash
   # Update the base_dir path in create_slim_models.py if your project is in a different location
   python create_slim_models.py
   ```

   This creates optimized f32 slim versions of the SMPL-X models in `third_party/PromptHMR/data/body_models/smplx/`:
   - `SMPLX_neutral_array_f32_slim.npz`
   - `SMPLX_male_array_f32_slim.npz`
   - `SMPLX_female_array_f32_slim.npz`

   The script automatically:
   - Reads from `smplx_lockedhead_extracted/models_lockedhead/smplx/`
   - Converts from `float64` to `float32`
   - Removes unnecessary data to create slim versions
   - Saves to the PromptHMR body models directory

### Troubleshooting: Manual Downloads and Missing Files

If the automated download scripts fail or you encounter missing files, you may need to download files manually:

1. **Manual SMPL Download:**
   - If `SMPL_python_v.1.1.0.zip` is not downloaded automatically:
     - Download from [SMPL website](https://smpl.is.tue.mpg.de) after registration
     - Place in project root or extract manually
     - Extract to `third_party/PromptHMR/data/body_models/smpl/smpl/`
     - Rename files as described in the automated process above

2. **Root-Level Body Models Directory:**
   - You may find a `body_models/` directory in the project root
   - This is typically a copy or alternative location for body models
   - The primary location used by the code is `third_party/PromptHMR/data/body_models/`
   - If files are missing from the primary location, you can copy them from the root `body_models/` directory:
     ```bash
     # Copy SMPL-X models if needed
     cp -r body_models/smplx/* third_party/PromptHMR/data/body_models/smplx/
     
     # Copy accessory files if needed
     cp -r body_models/accessory/* third_party/PromptHMR/data/body_models/accessory/
     ```

3. **Missing Files:**
   - Some files may not be included in the automated download scripts
   - Check the expected directory structure above and verify all required files are present
   - If files are missing, download them manually from the respective websites:
     - SMPL-X: https://smpl-x.is.tue.mpg.de
     - SMPL: https://smpl.is.tue.mpg.de
   - After manual downloads, ensure you convert `.npz` files to `float32` format as described above

4. **File Format Issues:**
   - If you encounter errors related to data types, ensure all `.npz` files have been converted to `float32`
   - The conversion script handles nested structures and object arrays
   - Always backup original files before conversion

## Usage

> **Note**: Scripts automatically switch to the appropriate conda environment (`gmr` or `phmr`) as needed. Just ensure both environments are installed - no need to manually activate them.

```bash
# Full pipeline (action → robot motion) - BASE_PROMPT auto-applied
python scripts/run_pipeline.py --action "Action sequence:
The subject walks forward with four steps."

# Use Sora
python scripts/run_pipeline.py --action "..." --provider sora

# Start from existing video (video.mp4 → robot motion)
python scripts/run_pipeline.py --video /path/to/video.mp4

# Resume from existing project
python scripts/run_pipeline.py --project data/video_001

# Run individual steps
python scripts/generate_video.py --action "Action sequence: The subject walks forward."
python scripts/extract_pose.py --project data/video_001
python scripts/convert_to_robot.py --project data/video_001

# Visualization (auto env switching)
python scripts/visualize.py --project data/video_001
python scripts/visualize.py --project data/video_001 --pose
python scripts/visualize.py --project data/video_001 --robot
```

## Web UI

```bash
# Run server (from video2robot root)
# Use python -m uvicorn to ensure conda environment packages are used
python -m uvicorn web.app:app --host 0.0.0.0 --port 8000

# Access in browser
# http://localhost:8000
```

Features:
- Prompt input → Video generation → Pose extraction → Robot conversion automatic pipeline
- Video upload support
- Veo/Sora model selection
- 3D visualization (viser)
- Video-3D synchronized playback

## Environment Setup

```bash
# Create .env file
cp .env.example .env

# Set API key
echo "GOOGLE_API_KEY=your-api-key" >> .env
```

## Supported Robots

| Robot | ID | DOF |
|-------|-----|-----|
| Unitree G1 | `unitree_g1` | 29 |
| Unitree H1 | `unitree_h1` | 19 |
| Booster T1 | `booster_t1` | 23 |

See [GMR README](third_party/GMR/README.md) for full list

## Output Format

```python
# robot_motion.pkl
{
    "fps": 30.0,
    "robot_type": "unitree_g1",
    "num_frames": 240,
    "root_pos": np.ndarray,    # (N, 3)
    "root_rot": np.ndarray,    # (N, 4) quaternion xyzw
    "dof_pos": np.ndarray,     # (N, DOF)
}
```


## TODO

- [ ] **`lastFrame` (Start/End Frame Interpolation)** - Veo 3.1 only
  - Start image + End image → Generate video smoothly connecting the two
  - Useful for "Pose A → Pose B" robot motion videos

- [ ] **`referenceImages` (Reference Images)** - Veo 3.1 only
  - Up to 3 reference images to maintain character/style
  - Generate videos with specific character performing actions

## Acknowledgements

This project builds upon the following excellent open source projects:

- [PromptHMR](https://github.com/yufu-wang/PromptHMR): 3D human mesh recovery from video
- [GMR](https://github.com/YanjieZe/GMR): general motion retargeting framework

## License

This project depends on third-party libraries with their own licenses:

- **[GMR](third_party/GMR/LICENSE)**: MIT License
- **[PromptHMR](third_party/PromptHMR/LICENSE)**: Non-Commercial Scientific Research Use Only

Please review both licenses before use.

> The core video2robot code is MIT-licensed, but using this
> repository end-to-end (including PromptHMR) inherits PromptHMR's
> **Non-Commercial Scientific Research Only** restriction. Commercial use
> requires obtaining appropriate permission from the PromptHMR authors.
