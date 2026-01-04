# Pyramid Blending & AI Face Swap

Multi-scale image blending using Laplacian pyramids with AI-powered automatic face swapping capabilities.

## Overview

This project implements advanced image compositing techniques combining traditional computer vision (pyramid blending) with modern AI (MediaPipe face detection) for seamless face swapping and image blending.

## Features

### Core Functionality
- **Laplacian Pyramid Blending** - Multi-scale image composition for seamless edge transitions
- **Gaussian/Laplacian Pyramid Generation** - Custom implementation with configurable levels
- **Interactive Mask Creation GUI** - Rectangle/ellipse selection tools
- **AI Face Detection** - MediaPipe-based 468-point facial landmark detection
- **Automatic Face Alignment** - Feature-based scale and rotation matching
- **Smart Color Matching** - LAB color space histogram matching

### Advanced Optimizations
- **Ultra-smooth edge transitions** with multi-pass Gaussian blur
- **Distance transform-based mask feathering**
- **Progressive pyramid-level blending**
- **Edge-aware bilateral filtering** for seamless composites
- **Automatic model download** for MediaPipe

## Project Structure

```
pyramid_blending/
├── Proj_opt1.ipynb                          # Basic pyramid blending
├── Proj_ML_AutoMask.ipynb                   # AI-powered face swap
├── Proj_ML_AutoMask_v2.ipynb                # AI-powered face swap with optimized edge smooth
├── test_img/                                # Sample images
└── README.md                                # This file
```

## Requirements

### Python Packages
```bash
pip install numpy opencv-python matplotlib pillow mediapipe
```

### Optional (for Jupyter/Colab)
```bash
pip install ipywidgets jupyter
```

## Usage

### 1. Basic Pyramid Blending

Open `Proj_opt1.ipynb` in Jupyter:

```python
# Load images
source_image = load_image("source.jpg")
target_image = load_image("target.jpg")
mask = create_mask(source_image)  # Interactive GUI

# Blend with pyramids
result = blend_with_pyramids(target_image, source_image, mask, levels=6)
```

### 2. AI-Powered Face Swap

Open `Proj_ML_AutoMask.ipynb` or use the upload version for Google Colab:

```python
# Automatic face detection and swapping
result = auto_face_swap(
    source_image=source_face,    # Face to swap IN
    target_image=target_photo,   # Face to swap ONTO
    visualize=True,
    pyramid_levels=6
)
```

The AI pipeline automatically:
1. Detects faces using MediaPipe (468 landmarks)
2. Aligns facial features (eyes, nose, mouth)
3. Generates smooth face mask from contours
4. Matches colors in LAB space
5. Blends using optimized multi-scale algorithm

### 3. Google Colab

Upload `Proj_ML_AutoMask_Upload.ipynb` to Google Colab:
- Interactive file upload widgets
- Auto-installs compatible MediaPipe version
- Works with both new and legacy MediaPipe APIs

## Technical Details

### Pyramid Blending Algorithm

1. **Build Pyramids**
   - Gaussian pyramid: Progressive smoothing + downsampling
   - Laplacian pyramid: Difference between levels
   - Mask pyramid: Smooth transitions at all scales

2. **Multi-scale Blending**
   ```
   For each level i:
       blended[i] = mask[i] * source[i] + (1 - mask[i]) * target[i]
   ```

3. **Reconstruction**
   - Upsample and add Laplacian levels
   - Progressive reconstruction from coarse to fine

### Edge Smoothness Optimizations

- **Multi-pass Gaussian blur** (4 passes with increasing kernel sizes)
- **Distance transform** for natural falloff from mask center
- **Progressive feathering** across pyramid levels
- **Bilateral filtering** in edge regions only

### Color Matching

LAB color space histogram matching:
- L channel: Luminance matching
- A channel: Green-red axis
- B channel: Blue-yellow axis

Better perceptual accuracy than RGB matching.

## Performance

- **Small images** (500×500): ~100ms
- **Medium images** (1000×1000): ~500ms
- **Large images** (2000×2000): ~2s

Using 6 pyramid levels with optimized algorithms.

## Examples

### Face Swap Results
- `test_img/profile.jpg` → `test_img/monalisa.jpg`
- Automatic alignment, masking, and seamless blending
- Invisible seam lines with smooth color transitions

### Creative Blending
- Portrait compositing
- Object insertion with natural shadows
- Artistic image merging

## MediaPipe API Compatibility

The code automatically detects and uses the correct MediaPipe API:

**New API** (mediapipe >= 0.10.8):
```python
from mediapipe.tasks.python import vision
landmarker = vision.FaceLandmarker.create_from_options(options)
```

**Legacy API** (mediapipe < 0.10.8):
```python
import mediapipe as mp
face_mesh = mp.solutions.face_mesh.FaceMesh(...)
```

## Academic Context

**Course**: ECE558 - Digital Image Processing
**Project**: Project03-Final - Option 1 (Pyramid Blending)
**Requirements**:
- Custom convolution implementation (no high-level libraries)
- Nearest-neighbor interpolation for pyramids
- Interactive mask creation GUI
- Multiple image pair demonstrations

## Limitations

- Requires clear facial features for AI detection
- Best results with similar lighting conditions
- Large scale differences may need manual adjustment
- Processing time increases with image size and pyramid levels

## Future Enhancements

- [ ] Real-time video face swapping
- [ ] Multi-face detection and swapping
- [ ] GPU acceleration with CUDA
- [ ] Deep learning-based refinement
- [ ] 3D face model integration

## License

Academic project - Educational use only

## Author

ECE558 Course Project - 2026

---

**Note**: This implementation prioritizes smoothness and seamless blending. For best results, use images with similar lighting and adjust `pyramid_levels` (6-8 recommended) based on image size and detail preservation needs.
