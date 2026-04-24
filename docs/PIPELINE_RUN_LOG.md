# MRI SR Pipeline - Preprocessing Run Log

**Date:** 2026-03-01  
**Subject:** `100206_T2w.nii.gz`  
**Status:** SUCCESS - Pipeline complete. Data ready for WGAN training.

---

## Full Run Log

```
(venv) PS D:\Campus\FYP\General\MRI\mri_sr_pipeline> python main.py
2026-03-01 00:32:16,781 - INFO - Loading Template: ./data/templates/mni152_template.nii.gz
100%|████████████████████████████████████████████████████| 109M/109M [01:00<00:00, 1.89MB/s]
perform_everything_on_device=True is only supported for cuda devices! Setting this to False
2026-03-01 00:33:30,427 - INFO - Found 1 files to process.
2026-03-01 00:33:30,458 - INFO - Starting subject: 100206_T2w.nii.gz
2026-03-01 00:33:30,913 - INFO - Extracting brain using HD-BET...
There are 1 cases in the source folder
I am processing 0 out of 1 (max process ID is 0, we start counting with 0!)
There are 1 cases that I would like to predict

Predicting temp_output_bet.nii.gz:
perform_everything_on_device: False
100%|████████████████████████████████████████████████████| 8/8 [01:52<00:00, 14.05s/it]
sending off prediction to background worker for resampling and export
done with temp_output_bet.nii.gz
2026-03-01 00:36:31,429 - INFO - Saved intermediate: 00_brain_extracted
2026-03-01 00:36:32,676 - INFO - Saved intermediate: 01_raw_reoriented
2026-03-01 00:36:32,677 - INFO - Processing HR path...
2026-03-01 00:36:32,677 - INFO - Applying N4 Bias Correction to HR...
2026-03-01 00:37:45,166 - INFO - Saved intermediate: 03_hr_n4
2026-03-01 00:37:45,167 - INFO - Applying whitestripe Normalization to HR...
2026-03-01 00:37:48,228 - INFO - Saved intermediate: 04_hr_norm
2026-03-01 00:37:48,229 - INFO - Registering HR to MNI152 (Affine)...
2026-03-01 00:38:38,310 - INFO - Saved intermediate: 05_hr_registered_mni
2026-03-01 00:38:38,804 - INFO - Simulating LR variants...
2026-03-01 00:38:38,806 - INFO - -> Simulating Thick Slice: 3.0mm
2026-03-01 00:38:58,929 - INFO - Saved intermediate: thick_3mm_03_n4
2026-03-01 00:38:59,832 - INFO - Saved intermediate: thick_3mm_04_norm
2026-03-01 00:39:40,037 - INFO - Saved intermediate: thick_3mm_05_reg
2026-03-01 00:39:40,458 - INFO - -> Simulating Thick Slice: 5.0mm
2026-03-01 00:39:47,962 - INFO - Saved intermediate: thick_5mm_03_n4
2026-03-01 00:39:48,366 - INFO - Saved intermediate: thick_5mm_04_norm
2026-03-01 00:40:12,803 - INFO - Saved intermediate: thick_5mm_05_reg
2026-03-01 00:40:13,109 - INFO - -> Simulating Gap: Thickness=3.0mm Gap=0.0mm
2026-03-01 00:40:22,610 - INFO - Saved intermediate: gap_th3_gap0mm_03_n4
2026-03-01 00:40:23,114 - INFO - Saved intermediate: gap_th3_gap0mm_04_norm
2026-03-01 00:40:51,090 - INFO - Saved intermediate: gap_th3_gap0mm_05_reg
2026-03-01 00:40:51,411 - INFO - -> Simulating Gap: Thickness=4.0mm Gap=0.4mm
2026-03-01 00:40:57,192 - INFO - Saved intermediate: gap_th4_gap0mm_03_n4
2026-03-01 00:40:57,509 - INFO - Saved intermediate: gap_th4_gap0mm_04_norm
2026-03-01 00:41:23,019 - INFO - Saved intermediate: gap_th4_gap0mm_05_reg
2026-03-01 00:41:23,335 - INFO - -> Simulating Gap: Thickness=5.0mm Gap=0.5mm
2026-03-01 00:41:29,602 - INFO - Saved intermediate: gap_th5_gap0mm_03_n4
2026-03-01 00:41:29,881 - INFO - Saved intermediate: gap_th5_gap0mm_04_norm
2026-03-01 00:42:03,385 - INFO - Saved intermediate: gap_th5_gap0mm_05_reg
2026-03-01 00:42:03,766 - INFO - -> Simulating Gap: Thickness=5.0mm Gap=1.0mm
2026-03-01 00:42:08,896 - INFO - Saved intermediate: gap_th5_gap1mm_03_n4
2026-03-01 00:42:09,284 - INFO - Saved intermediate: gap_th5_gap1mm_04_norm
2026-03-01 00:42:34,104 - INFO - Saved intermediate: gap_th5_gap1mm_05_reg
2026-03-01 00:42:34,490 - INFO - -> Simulating In-Plane Downsample: x1
2026-03-01 00:43:18,524 - INFO - Saved intermediate: inplane_ds1_03_n4
2026-03-01 00:43:24,134 - INFO - Saved intermediate: inplane_ds1_04_norm
2026-03-01 00:43:51,451 - INFO - Saved intermediate: inplane_ds1_05_reg
2026-03-01 00:43:52,139 - INFO - -> Simulating In-Plane Downsample: x2
2026-03-01 00:43:58,695 - INFO - Saved intermediate: inplane_ds2_03_n4
2026-03-01 00:43:59,407 - INFO - Saved intermediate: inplane_ds2_04_norm
2026-03-01 00:44:30,729 - INFO - Saved intermediate: inplane_ds2_05_reg
2026-03-01 00:44:32,514 - INFO - Successfully processed 100206_T2w.nii.gz
Pipeline complete. Data ready for WGAN training.
```

---

## Pipeline Stages Summary

| Step | Stage | Output Intermediate | Time |
|------|-------|---------------------|------|
| 1 | MNI152 template download | — | ~1m 00s |
| 2 | Brain extraction (HD-BET) | `00_brain_extracted` | ~3m 01s |
| 3 | Reorientation | `01_raw_reoriented` | — |
| 4 | N4 Bias Correction (HR) | `03_hr_n4` | ~1m 13s |
| 5 | WhiteStripe Normalization (HR) | `04_hr_norm` | ~3s |
| 6 | Affine Registration to MNI152 (HR) | `05_hr_registered_mni` | ~50s |
| 7 | LR Simulation: Thick Slice 3mm | `thick_3mm_03_n4`, `_04_norm`, `_05_reg` | ~1m 01s |
| 8 | LR Simulation: Thick Slice 5mm | `thick_5mm_03_n4`, `_04_norm`, `_05_reg` | ~32s |
| 9 | LR Simulation: Gap th=3mm gap=0mm | `gap_th3_gap0mm_03_n4`, `_04_norm`, `_05_reg` | ~38s |
| 10 | LR Simulation: Gap th=4mm gap=0.4mm | `gap_th4_gap0mm_03_n4`, `_04_norm`, `_05_reg` | ~32s |
| 11 | LR Simulation: Gap th=5mm gap=0.5mm | `gap_th5_gap0mm_03_n4`, `_04_norm`, `_05_reg` | ~40s |
| 12 | LR Simulation: Gap th=5mm gap=1mm | `gap_th5_gap1mm_03_n4`, `_04_norm`, `_05_reg` | ~31s |
| 13 | LR Simulation: In-Plane DS x1 | `inplane_ds1_03_n4`, `_04_norm`, `_05_reg` | ~1m 17s |
| 14 | LR Simulation: In-Plane DS x2 | `inplane_ds2_03_n4`, `_04_norm`, `_05_reg` | ~38s |

**Total runtime:** ~12 minutes 16 seconds

---

## LR Variants Produced (per subject)

Each variant generates 3 intermediates: `_03_n4` (bias corrected), `_04_norm` (normalized), `_05_reg` (registered to MNI152).

| Variant | Description |
|---------|-------------|
| `thick_3mm` | Thick-slice simulation at 3.0mm slice thickness |
| `thick_5mm` | Thick-slice simulation at 5.0mm slice thickness |
| `gap_th3_gap0mm` | Gap simulation: 3mm thickness, 0mm gap |
| `gap_th4_gap0mm` | Gap simulation: 4mm thickness, 0.4mm gap |
| `gap_th5_gap0mm` | Gap simulation: 5mm thickness, 0.5mm gap |
| `gap_th5_gap1mm` | Gap simulation: 5mm thickness, 1mm gap |
| `inplane_ds1` | In-plane downsampling x1 |
| `inplane_ds2` | In-plane downsampling x2 |

---

## Notes

- HD-BET ran on **CPU** (`perform_everything_on_device: False`) — GPU (CUDA) was not available.
- MNI152 template was downloaded fresh on first run (~109MB).
- All intermediates saved to `data/processed/` for inspection before WGAN training.
- This run confirms the pipeline is fully functional and output is ready to be consumed by the WGAN training repo.
