# Pocket OMR

A phone photo of an exam sheet becomes a graded answer sheet. Pocket OMR replaces a
dedicated OMR scanner with the camera most schools already have: the sheet is rectified,
its regions are segmented, the bubbles are read and the handwritten name and identifier are
transcribed.

Pluridisciplinary project, ESI-SBA, 2025/2026. Supervisor: Pr. Oussama Serhane.

## Components

| Repository | Role |
| --- | --- |
| [mobile](https://github.com/pocket-omr/mobile) | Flutter client: capture and result |
| [frontend](https://github.com/pocket-omr/frontend) | Web client: upload, review, per-field metrics |
| [backend](https://github.com/pocket-omr/backend) | API, storage and pipeline orchestration |
| `cv-model` (private) | Rectification, segmentation, bubble reading, character recognition |

Status: the component repositories currently contain the project scaffold and the
configuration contract, not the implementation.

## Pipeline

1. **Normalisation.** Detect the printed markers, warp the sheet to 1240x1754, correct the
   QR orientation, remove shadows and binarise with CLAHE and Otsu.
2. **Segmentation.** Eight form regions are located with YOLOv8m-seg.
3. **Bubble reading.** A classifier reads which bubble is filled per question. Detection
   and classification stay separate: an end-to-end detector was tried and rejected because
   finding a bubble is not the same as reading it.
4. **Character recognition.** Printed and handwritten fields are read by a three-model
   ensemble (EfficientNetV2-S, SimpleCNN_SE, MobileNetV3-S) pre-trained on EMNIST and
   combined by soft voting.
5. **Serving.** FastAPI service, React and Flutter clients, containerised with Docker.

## Stack

Python, PyTorch, YOLOv8m-seg, OpenCV, FastAPI, React, Flutter, Docker.

## Notes

The dataset was collected and annotated by the team, so the capture conditions in the
reported metrics are the same ones the pipeline was tuned on. Per-field metrics, not
per-component metrics, are the number that decides whether the system works.

## Team

Medjahri Mohammed Safouane, Ben Terki Abdel-Ouakil, Fezazi Khadidja Amina, Soummar Inas,
Khelladi Alaa, Aini Ines.
