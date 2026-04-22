## Chapter 11: Intelligence at the Edge — Quick Reference

### Core Concepts
| Concept | One-line explanation |
|---------|---------------------|
| On-device inference | ML runs directly on sensor/MCU—lowest latency, highest privacy, limited compute |
| Gateway inference | ML runs on local hub (Pi, Jetson)—balances capability with proximity |
| Cloud inference | ML runs remotely—unlimited compute, requires connectivity, adds latency |
| Model quantization | Reducing precision (FP32→INT8) to shrink model size and speed inference |
| TinyML | Machine learning optimized for microcontrollers with KB-level memory |
| Federated learning | Training models across devices without centralizing raw data |

### Key Syntax / Commands
```bash
# TensorFlow Lite quantization
tflite_convert --output_file=model.tflite --saved_model_dir=./model \
  --inference_type=QUANTIZED_UINT8

# Edge Impulse CLI deployment
edge-impulse-linux-runner --model-file model.eim

# Check model size
ls -lh model.tflite  # Target: <100KB for MCUs
```

### Common Patterns
**Pattern 1: Tiered Inference**
Simple anomaly detection on-device → complex classification at gateway → retraining in cloud

**Pattern 2: Wake-word Architecture**
Tiny always-on model triggers larger model or cloud connection only when needed

### Things to Remember
✅ Latency-critical = edge; data-heavy training = cloud
✅ Quantization can achieve 4x size reduction with <2% accuracy loss
✅ Plan OTA model updates BEFORE deployment—it's the hardest part
❌ Don't choose edge AI just because it's trendy—match to actual requirements

### Quick Quiz
1. When is cloud inference preferred? → When models update frequently or require GPU-scale compute
2. What does INT8 quantization reduce? → Model size and inference time by ~4x vs FP32