# GS-AgriField: Monocular Visual Odometry in Agricultural Environments

> **Status:** in preparation. The dataset links, paper reference and DOI will
> be added at publication.

This repository accompanies the **GS-AgriField** dataset and the paper *[PAPER TITLE]* ([VENUE], [YEAR]).
It contains documentation and utilities to:

1. **download** the raw ROS 2 bags (MCAP) from the MARV Robotics server, and
2. **convert** them into a KITTI-like monocular sequence format.

The datasets themselves are **not** stored in this repository.

---

## Download Dataset

**➡️ [Download the dataset on MARV Robotics]([MARV_DATASET_URL])**

The data is hosted on a MARV Robotics server in Clermont-Ferrand, France.

| Sequence | Environment | MARV link | Size |
|----------|-------------|-----------|------|
| [SEQUENCE_NAME] | [ENVIRONMENT] | [MARV_LINK] | [SIZE] |

To fetch a sequence from the command line, see [docs/download.md](docs/download.md).

---

## Dataset Overview

> **TODO:** describe the environments, crops and growth stages, robot platform,
> sensors, acquisition dates, number of sequences, total duration/length, and
> ground-truth source and accuracy. Only report values from the paper.

Each sequence is a ROS 2 bag (MCAP storage). The topics used by the KITTI-like
converter are:

| Role       | Topic                            | Message type                 |
|------------|----------------------------------|------------------------------|
| Camera     | `/cam_nav1/hd/image_color_rect`  | `sensor_msgs/msg/Image`      |
| Intrinsics | `/cam_nav1/hd/camera_info`       | `sensor_msgs/msg/CameraInfo` |
| Odometry   | `/odom/sbg`                      | `nav_msgs/msg/Odometry`      |
| Transforms | `/tf_static`, `/tf`              | `tf2_msgs/msg/TFMessage`     |

## Repository Structure

```text
.
├── README.md
├── LICENSE                 BSD 3-Clause (code)
├── LICENSE-DATA            CC BY-NC-SA 4.0 (dataset)
├── CITATION.cff
├── requirements.txt        Python dependencies of the downloader
├── scripts/
│   ├── download_marv.py                 download a ROS 2 bag from MARV
│   └── mono_camera_kitti_converter.py   ROS 2 bag -> KITTI-like sequence
└── docs/
    ├── download.md         downloading the ROS 2 bags
    └── conversion.md       KITTI-like conversion, file formats and conventions
```

## Quick Start

### 1. Download a ROS 2 bag

```bash
pip install -r requirements.txt
export MARV_USER=<user>
python3 scripts/download_marv.py <dataset_hash> --output-dir data/bags
```

### 2. Convert it to a KITTI-like sequence

Requires ROS 2 Humble with the MCAP storage plugin (see
[docs/conversion.md](docs/conversion.md), including a Docker option).

```bash
source /opt/ros/humble/setup.bash
python3 scripts/mono_camera_kitti_converter.py \
  --bag data/bags/<bag_name> \
  --out data/kitti \
  --seq <bag_name>
```

### KITTI-like output

```text
data/kitti/sequences/<sequence_name>/
├── image_/      PNG frames 000000.png, 000001.png, ...
├── calib.txt    P2: 3x4 projection matrix from CameraInfo
├── poses.txt    3x4 pose per frame (KITTI format), nearest /odom/sbg message
├── times.txt    timestamp per frame (s)
└── info.txt     conversion metadata
```

The single camera goes to `image_/`, not `image_2/`. The converter also writes
`nav_camera_to_imu_link.txt` (camera ↔ `imu_link` extrinsics from TF), a
per-sequence `README.md`, and an optional `topics/` text export. See
[docs/conversion.md](docs/conversion.md) for every file format and frame
convention.

## License

- **Code** (`scripts/`): [BSD 3-Clause License](LICENSE).
- **Dataset** (ROS 2 bags hosted on MARV, and the data derived from them):
  [Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International
  (CC BY-NC-SA 4.0)](LICENSE-DATA).

## Citation

If you use this dataset, please cite:

```bibtex
@article{[CITATION_KEY],
  title   = {[PAPER TITLE]},
  author  = {[AUTHORS]},
  journal = {[VENUE]},
  year    = {[YEAR]},
  doi     = {[DOI]}
}
```

See also [CITATION.cff](CITATION.cff).

## Affiliations and Acknowledgements

> **TODO:** list the author affiliations, funding sources and project
> acknowledgements.

Data hosting: MARV Robotics server, Clermont-Ferrand, France.

## Contact

[CONTACT NAME / EMAIL]
