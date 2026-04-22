## Chapter 11: Intelligence at the Edge — Flashcards

| # | Front (Question) | Back (Answer) |
|---|-----------------|---------------|
| 1 | What are the three main locations where IoT inference can occur? | On-device (at the sensor/endpoint), gateway (intermediate node), and cloud (remote servers) |
| 2 | When should you choose on-device inference over cloud inference? | When you need ultra-low latency, offline operation, strong privacy guarantees, or want to minimize bandwidth costs |
| 3 | What is model quantization? | The process of reducing the precision of model weights and activations (e.g., from 32-bit floats to 8-bit integers) to decrease model size and computational requirements |
| 4 | What is TinyML? | Machine learning designed for microcontrollers and extremely resource-constrained devices, typically operating with milliwatts of power and kilobytes of memory |
| 5 | What are the main tradeoffs of cloud inference vs edge inference? | Cloud offers more compute power and easier updates but has higher latency, requires connectivity, and raises privacy concerns |
| 6 | Why is model update a significant deployment challenge for edge AI? | Edge devices are distributed, may have limited connectivity, require version management, and updates must not disrupt operations |
| 7 | What is federated learning? | A distributed ML approach where models are trained across multiple devices using local data, with only model updates (not raw data) sent to a central server |
| 8 | What role does a gateway play in edge AI architecture? | It aggregates data from multiple sensors, can run more complex models than endpoints, and reduces cloud communication while maintaining some centralized control |
| 9 | Name two popular TinyML/edge AI frameworks. | TensorFlow Lite Micro, Edge Impulse, ONNX Runtime, PyTorch Mobile, or Apache TVM |
| 10 | What are the privacy benefits of federated learning? | Raw data never leaves the local device; only aggregated model gradients or weights are shared, reducing exposure of sensitive information |
| 11 | What constraints make on-device inference challenging? | Limited memory (KB-MB), low processing power, battery/power constraints, and restricted storage for model weights |
| 12 | What is the typical accuracy tradeoff with quantization? | Quantized models may lose 1-3% accuracy compared to full-precision models, though careful quantization-aware training can minimize this loss |
| 13 | Why might latency requirements push decisions toward edge inference? | Edge inference avoids network round-trip delays, enabling real-time responses in milliseconds rather than hundreds of milliseconds or seconds |
| 14 | What factors determine the optimal edge-vs-cloud inference decision? | Latency requirements, privacy constraints, update frequency, connectivity reliability, compute needs, and total cost of ownership |
| 15 | What is quantization-aware training? | Training a model while simulating the effects of quantization, allowing the model to learn to maintain accuracy despite reduced precision |

### Key Terms

| Term | Definition |
|------|-----------|
| Edge Inference | Running ML models directly on local devices near the data source rather than in the cloud |
| Model Quantization | Reducing numerical precision of model parameters to shrink model size and speed up inference |
| TinyML | Machine learning optimized for microcontrollers with severe memory, compute, and power constraints |
| Federated Learning | Distributed training approach where local devices train on local data and share only model updates |
| Gateway Inference | Running ML models on intermediate aggregation nodes between sensors and cloud |
| Over-the-Air (OTA) Updates | Wireless delivery of software or model updates to deployed edge devices |
| Inference Latency | The time from input to prediction output when running a ML model |

### Memory Tricks
- **PLUC** for edge-vs-cloud decision factors: **P**rivacy, **L**atency, **U**pdate frequency, **C**ost
- **"Quant = Squint"**: Quantization makes models "squint" at data with less precision, but they still see the big picture
- **"Fed keeps data in bed"**: Federated learning keeps data sleeping safely on local devices, never traveling to the cloud
- **TinyML = "Tiny Memory, Limited power"**: Remember the key constraints of microcontroller ML