# Inference Pipeline Guide for Developers

This guide explains how to use the `MRIInferencePipeline` module within a web application backend to preprocess low-resolution (LR) MRI scans before feeding them to a super-resolution Machine Learning model.

## Overview
Unlike the training pipeline (`MRIPreprocessingPipeline`), which expects raw High-Resolution scans and artificially degrades them to create paired HR-LR datasets, the **Inference Pipeline** is designed to accept an already Low-Resolution clinical scan as its input. 

The pipeline performs the following steps sequentially:
1. Load Image
2. Brain Extraction (HD-BET, if enabled)
3. Reorient to Standard Coordinate System (RAI)
4. N4 Bias Field Correction
5. Intensity Normalization
6. Registration to MNI152 template

The output is an `ANTsImage` (and optionally a saved `.nii.gz` file) perfectly formatted to be passed directly to the Super-Resolution ML Model.

## Getting Started

### 1. Requirements

Ensure your environment has the required dependencies, primarily:
- `antspyx`
- `pyyaml`
- `HD-BET` (if brain extraction is enabled)

### 2. Configuration (`config.yaml`)

The Inference Pipeline reuses the existing `configs/config.yaml` file. The relevant sections for inference are:
- `paths.template_path`: Path to the MNI152 template.
- `preprocessing.brain_extraction`: Settings for HD-BET.
- `preprocessing.bias_correction`: Settings for N4 bias correction.
- `preprocessing.registration`: Registration type (e.g., Affine).
- `preprocessing.normalization`: Normalization method (e.g., whitestripe or zscore).

*(Note: The `simulation` and `training` sections in the config are ignored during inference).*

## Example Usage in Web Application

Here is an example of how you might integrate this into a FastAPI or Flask backend route:

```python
import os
import tempfile
from fastapi import FastAPI, UploadFile, File
from src.inference import MRIInferencePipeline

app = FastAPI()

# 1. Initialize pipeline ONCE during startup to keep it in memory
# This loads the MNI template and configures modules, which takes a few seconds.
PIPELINE_CONFIG_PATH = "configs/config.yaml"
inference_pipeline = MRIInferencePipeline(PIPELINE_CONFIG_PATH)

@app.post("/super_resolve/")
async def super_resolve_scan(file: UploadFile = File(...)):
    # Create a temporary directory for processing
    with tempfile.TemporaryDirectory() as temp_dir:
        input_path = os.path.join(temp_dir, file.filename)
        output_path = os.path.join(temp_dir, f"preprocessed_{file.filename}")
        
        # Save the uploaded LR file temporarily
        with open(input_path, "wb") as buffer:
            buffer.write(await file.read())
            
        try:
            # 2. Run the Inference Pipeline
            # process() returns an ANTsImage and also saves to output_path if provided
            preprocessed_ants_image = inference_pipeline.process(
                input_path=input_path, 
                output_path=output_path
            )
            
            # 3. Pass to Super-Resolution Model
            # Option A: Load the saved file
            # sr_output = my_ml_model.predict(output_path)
            
            # Option B: Pass ANTs image directly (convert to numpy array)
            # numpy_image = preprocessed_ants_image.numpy()
            # sr_output = my_ml_model.predict(numpy_image)
            
            # 4. Return result to user...
            return {"status": "success", "message": "Preprocessed and Super-resolved successfully!"}
            
        except Exception as e:
            return {"status": "error", "message": str(e)}
```

## Important Considerations

> [!WARNING]
> **Brain Extraction Device:** In `config.yaml`, the `device` for brain extraction defaults to `cpu`. If your web server has a GPU, change `device: "cuda"` to significantly speed up processing.

> [!TIP]
> **Memory Management:** `antspyx` handles images in C++. When working in a web server environment, be cautious about memory leaks. Keeping `MRIInferencePipeline` instantiated globally is good for performance (avoids reloading the MNI template), but ensure your ML model and the garbage collector are freeing up image arrays after each request.

> [!NOTE]
> **Output Shapes:** The final registered output will have the exact dimensions and spacing of the `mni152_template.nii.gz` file provided in your configuration. Your ML model must be trained to accept this specific tensor shape.
