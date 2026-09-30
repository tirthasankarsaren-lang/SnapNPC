# SnapNPC: On-Device Generative Dialogue Engine

## Overview
SnapNPC is a localized, low-latency generative dialogue engine designed for modern game development. It replaces rigid, scripted NPC dialogue trees with dynamic, context-aware conversations powered entirely on-device. Built specifically for Snapdragon-powered HP PCs, this solution leverages the Hexagon NPU to ensure ultra-low latency inference, complete offline availability, and privacy—all without draining battery or dropping the game's frame rate.

## Qualcomm AI Hub Integration
This project utilizes models optimized and exported directly via the **Qualcomm AI Hub**:
* **Model:** `Llama-v3.1-8B-Instruct` (Quantized to INT4 for edge deployment)
* **Runtime:** GenieX / Qualcomm QNN (Qualcomm Neural Network)

## Architecture & Data Flow
1. **Player Input:** Game engine captures player prompt and world context.
2. **Local API Dispatch:** Sent to local `localhost:8080` GenieX endpoint.
3. **Hexagon NPU Execution:** INT4 quantized Llama model executes on-device without cloud API calls.
4. **Game State Update:** NPC dialogue is returned to the game UI/TTS pipeline.

## Setup & Deployment
To run this locally on a Snapdragon PC before linking the game engine, use the AI Hub CLI to fetch and serve the model:

```bash
# 1. Install Qualcomm AI Hub CLI
pip install qai-hub qai-hub-models

# 2. Download and export the quantized model for Hexagon NPU
qai-hub-models generate Llama-v3.1-8B-Instruct --target-runtime qnn --device "Snapdragon X Elite Compute Platform"

# 3. Start the local inference server via GenieX
geniex serve --model-path ./build/Llama-v3.1-8B-Instruct.qnn --port 8080
