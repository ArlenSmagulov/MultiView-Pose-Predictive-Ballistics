# Architecture

MultiView-Pose-Predictive-Ballistics combines four software layers:

1. Perception: camera capture, object detection, pose estimation, and geometric projection.
2. Analytics: 3D joint normalization, rep segmentation, movement-quality metrics, confidence scoring, and report generation.
3. Closed-loop control: event logging, target confidence gates, camera-count gates, and launcher command constraints.
4. Operator outputs: live trainer overlays, coach-facing reports, C3D export, and camera-coverage visualizations.

The public repository focuses on layers 2-4 and includes standalone geometry tools from layer 1. The heavy live inference stack is intentionally not bundled with the portfolio repo because it depends on local cameras, model weights, TensorRT engines, and private calibration recordings.

## Data Flow

```text
Multi-camera runtime
    -> COCO-17 3D joint stream
    -> assessment metrics and live trainer state
    -> JSON/HTML/C3D reports

Camera/projector calibration
    -> wall coordinates and target grid
    -> per-camera wall votes
    -> temporal target consensus

Live targeting loop
    -> predicted joint target
    -> confidence and camera-count gates
    -> structured event log
    -> launcher command
```

## Review Focus

For software review, inspect `src/project_cam/assessment`, `src/project_cam/closed_loop`, `src/project_cam/projector`, and `tests`. For CV geometry review, inspect `scripts/visualize_camera_coverage.py` and `scripts/render_coverage_heatmap.py`.
