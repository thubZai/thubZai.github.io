---
title: "Markerless 3D Pose from Monocular Video"
date: 2025-06-15
domain: "3D Vision & Sports Biomechanics"
tags: ["3D Reconstruction", "Pose Estimation", "Computer Vision", "PyTorch"]
author: ["Yash Thube"]
description: "A minimal pipeline that recovers 3D human pose from a single video camera using pose estimation, camera intrinsics, and triangulation — no markers, no suit, no special hardware."
summary: "A minimal pipeline that recovers 3D human pose from a single video camera using pose estimation, camera intrinsics, and triangulation — no markers, no suit, no special hardware."
cover:
    image: "pipeline.png"
    alt: "3D pose estimation pipeline"
    relative: true
---

##### Overview

Recovering 3D human pose from a single camera is one of those problems that sounds trivial until you actually try it — then you realize how much geometry, calibration, and error modeling goes into getting something that looks right.

This project is a minimal, end-to-end pipeline that takes a monocular video and outputs 3D joint positions in camera coordinates. No motion capture suit. No marker set. No multi-camera rig. Just a phone camera, an open-source 2D pose estimator, and some careful geometry.

##### Pipeline

1. **2D pose estimation** — Run a pretrained pose model (HRNet, OpenPose, or MediaPipe) on every frame to get 2D joint detections with confidence scores.
2. **Camera calibration** — Estimate intrinsic parameters (focal length, principal point) from a checkerboard or using the known phone camera profile. Extrinsics are tracked frame-by-frame using feature-based PnP.
3. **Depth recovery** — Lift 2D joints to 3D using a combination of anatomical constraints (fixed bone lengths) and optimization over the reprojection error.
4. **Temporal smoothing** — Apply a Kalman filter or simple spline smoothing to reduce jitter across frames.

##### Why this matters

Most sports biomechanics systems require expensive marker-based capture (Vicon, OptiTrack) or specialized suits (Xsens). A markerless approach that works with an ordinary phone camera dramatically lowers the barrier to entry — especially in resource-constrained settings like community sports programs in India.

The tradeoff is accuracy: markerless systems typically sit in the 5–15 cm error range versus 1–2 mm for optical capture. But for many coaching applications — technique analysis, movement pattern classification, progress tracking — that level of precision is more than sufficient.

##### Limitations

- Single-camera depth is inherently ambiguous without anatomical priors
- Occlusions (self-occlusion, clothing, background clutter) break pose estimation
- No metric scale without a known reference object in the scene
- Accuracy degrades with motion blur and low lighting

These are all solvable problems, but they require more data, more calibration, and more engineering — which is exactly what the next iteration of this system will address.
