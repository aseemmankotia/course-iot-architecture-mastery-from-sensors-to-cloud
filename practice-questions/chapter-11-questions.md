## Chapter 11: Intelligence at the Edge — Practice Questions

### Multiple Choice

**Q1.** A smart agriculture system needs to detect plant diseases from camera images. The devices are solar-powered with limited connectivity, and decisions must be made within seconds to trigger automated spraying. Which inference placement strategy is most appropriate?

A) Cloud inference with batch processing
B) Gateway inference with local model hosting
C) On-device inference with quantized models
D) Hybrid approach with cloud-only model updates

<details>
<summary>Answer</summary>

**Correct: C**

On-device inference is optimal here because the system requires low latency (seconds), has limited connectivity, and must operate autonomously. Quantized models reduce power consumption for solar-powered devices. Cloud inference (A) would fail due to connectivity limitations and latency requirements. Gateway inference (B) adds unnecessary network hops and single points of failure. A hybrid approach (D) doesn't address the core inference placement need.

</details>

---

**Q2.** What is the primary goal of model quantization in edge AI deployments?

A) Increasing model accuracy by using higher precision weights
B) Reducing model size and computational requirements by using lower precision data types
C) Encrypting model weights for security purposes
D) Splitting the model across multiple edge devices

<details>
<summary>Answer</summary>

**Correct: B**

Model quantization converts model weights and activations from 32-bit floating point to lower precision formats (like 8-bit integers), significantly reducing memory footprint and computational requirements. This makes models feasible to run on constrained devices. Option A is incorrect as quantization typically trades some accuracy for efficiency. Option C describes encryption, not quantization. Option D describes model partitioning, a different optimization technique.

</details>

---

**Q3.** Which TinyML framework is specifically designed by Google for running machine learning models on microcontrollers with kilobytes of memory?

A) PyTorch Mobile
B) ONNX Runtime
C) TensorFlow Lite Micro
D) Apache MXNet

<details>
<summary>Answer</summary>

**Correct: C**

TensorFlow Lite Micro is Google's solution specifically engineered for microcontrollers with extremely limited resources (kilobytes of RAM). PyTorch Mobile (A) targets smartphones and larger edge devices. ONNX Runtime (B) is a cross-platform inference engine but not optimized for microcontrollers. Apache MXNet (D) is a full deep learning framework not designed for ultra-constrained devices.

</details>

---

**Q4.** In federated learning, what remains on the local device and is never transmitted to the central server?

A) Model gradients
B) Raw training data
C) Model architecture
D) Aggregated model weights

<details>
<summary>Answer</summary>

**Correct: B**

Federated learning's core privacy principle is that raw training data never leaves the local device. Instead, only model updates (gradients or weight differences) are transmitted to the central server for aggregation. Model gradients (A) are transmitted for aggregation. Model architecture (C) is typically shared across all participants. Aggregated weights (D) are computed on the server and distributed back to devices.

</details>

---

**Q5.** A fleet of 10,000 IoT sensors needs model updates deployed. Which challenge is unique to edge AI model updates compared to traditional software updates?

A) Network bandwidth limitations
B) Model version compatibility with existing inference pipelines and data preprocessing
C) Device authentication requirements
D) Rollback capability needs

<details>
<summary>Answer</summary>

**Correct: B**

Model updates introduce AI-specific challenges: new models may expect different input preprocessing, output formats, or have incompatible accuracy characteristics with downstream systems. Traditional software updates share concerns about bandwidth (A), authentication (C), and rollback (D), but model-inference pipeline compatibility is unique to ML deployments and can cause silent failures if input/output contracts change.

</details>

---

### True / False

**Q6.** Gateway-based inference is always preferable to on-device inference because gateways have more computational resources. — **True / False**

**False**

*While gateways typically offer more computational power, the optimal inference placement depends on multiple factors: latency requirements, network reliability, privacy concerns, and power constraints. On-device inference eliminates network latency and dependency, preserves data privacy, and continues operating during connectivity outages. For real-time safety-critical applications or privacy-sensitive data, on-device inference may be essential despite resource constraints.*

---

**Q7.** Federated learning completely eliminates privacy risks because raw data never leaves the device. — **True / False**

**False**

*While federated learning significantly improves privacy by keeping raw data local, it does not eliminate all privacy risks. Model gradients transmitted to the server can potentially leak information about training data through gradient inversion attacks or membership inference attacks. Additional techniques like differential privacy and secure aggregation are often needed to strengthen privacy guarantees in federated learning systems.*

---

### Short Answer

**Q8.** Explain two key tradeoffs when choosing between 8-bit integer quantization and 16-bit floating point quantization for edge deployment.

<details>
<summary>Answer</summary>

**Model Size and Memory:** 8-bit quantization reduces model size by 4x compared to 32-bit (and 2x compared to 16-bit), enabling deployment on more memory-constrained devices and faster loading times.

**Inference Accuracy:** 16-bit floating point typically preserves more model accuracy than 8-bit integer quantization, as it maintains better numerical precision. Models with sensitive operations (like attention mechanisms) may degrade significantly with aggressive 8-bit quantization.

**Inference Speed:** 8-bit integer operations are often faster on edge hardware with dedicated integer processing units, while 16-bit may require floating-point hardware support.

**Quantization Complexity:** 8-bit quantization often requires calibration datasets and quantization-aware training to maintain acceptable accuracy, adding deployment complexity.

</details>

---

**Q9.** Describe two strategies for managing model updates across a large fleet of heterogeneous IoT devices with varying capabilities.

<details>
<summary>Answer</summary>

**Model Tiering:** Deploy different model variants optimized for different device capability classes. High-capability devices receive full models while constrained devices receive smaller, more aggressively quantized versions. Updates are targeted by device tier.

**Staged Rollouts:** Deploy updates incrementally, starting with a small percentage of devices (canary deployment), monitoring inference quality metrics, then gradually expanding. This catches model compatibility issues before fleet-wide impact.

**A/B Testing Infrastructure:** Run old and new models simultaneously on subsets of devices, comparing real-world performance before committing to full deployment.

**Delta Updates:** Transmit only the changed model weights rather than complete models, reducing bandwidth requirements for bandwidth-constrained devices.

**Version Pinning with Gradual Migration:** Allow devices to maintain current model versions while new devices receive updated models, migrating existing devices during maintenance windows.

</details>

---

### Scenario-Based

**Q10.** A healthcare company is developing a wearable device that monitors heart rhythms and detects arrhythmias. The device has 256KB of RAM, a low-power ARM Cortex-M4 processor, and Bluetooth connectivity to a smartphone app. Patients wear the device 24/7, and false negatives (missing actual arrhythmias) have serious health consequences. The company also wants to improve the model over time using real patient data while complying with HIPAA privacy regulations.

Design an inference and learning architecture for this system. Address: (a) where inference should occur and why, (b) what model optimization techniques you would apply, (c) how you would handle model updates, and (d) how you could leverage patient data for improvement while maintaining privacy compliance.

<details>
<summary>Answer</summary>

**(a) Inference Placement:**
Primary inference should occur on-device with smartphone gateway as backup. On-device inference ensures continuous monitoring even without phone connectivity, provides lowest latency for critical arrhythmia alerts, and keeps sensitive health data local. The smartphone can run a more accurate secondary model for validation of detected events before alerting healthcare providers.

**(b) Model Optimization:**
Apply TensorFlow Lite Micro or similar TinyML framework. Use 8-bit integer quantization with quantization-aware training to fit within 256KB RAM while maintaining high sensitivity (minimizing false negatives). Employ model pruning to remove redundant parameters. Consider knowledge distillation from a larger cloud model to maximize accuracy within size constraints. Given the false negative consequences, optimize the detection threshold for high recall even at some precision cost.

**(c) Model Updates:**
Implement staged OTA updates through the smartphone app during charging periods to minimize power impact. Include model versioning and rollback capability. Validate new models against known arrhythmia patterns before deployment. Use A/B testing with a subset of devices before fleet-wide rollout. Maintain backward compatibility in data preprocessing pipelines.

**(d) Privacy-Compliant Learning:**
Implement federated learning where model training occurs on smartphones using locally-stored ECG data. Only encrypted model gradients are transmitted to central servers for aggregation. Apply differential privacy by adding calibrated noise to gradients before transmission. Use secure aggregation protocols so the server only sees combined updates from multiple patients. Obtain explicit patient consent for participation in model improvement. Maintain HIPAA compliance by ensuring no protected health information leaves the device, only mathematical model updates.

</details>