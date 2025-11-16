---
license: mit
language:
- en
tags:
- ONNX
- ONNXRUNTIME
- AXEngine
---

## Introduction

**PyAXEngine** provides a Python API for the Axera NPU Runtime using the cffi module. The Python API is similar to ONNX Runtime and supports both development boards and M.2 accelerator cards to help community developers rapidly prototype.
Python scripts for quickly building NPU inference programs

Supported chips

- AX650N
- AX630C

Environment / Version

- python >= 3.8
- cffi >= 1.0.0
- ml-dtypes >= 0.1.0
- numpy >= 1.22.0

*Note: If your development environment is a compute card, we recommend the [pyAXCL](https://github.com/AXERA-TECH/pyaxcl) project for production development, as it contains the full API for compute card environments. PyAXEngine is better suited for rapid prototyping by algorithm engineers and cannot call some codec-related modules on card environments (this is not a design goal of PyAXEngine).* 

*Note: AX650 SDK versions prior to 2.18 and AX620E SDK versions prior to 3.12 do not support bf16. LLM models may report unknown dtype. Please upgrade accordingly.*

*If you are unsure how to upgrade the SDK, you can open an issue to request a download of the updated `libax_engine.so` rather than a full SDK upgrade.*

## Quick start

Demonstrated on the community dev board: **AiXinPai Pro (AX650N)**

### Get wheel package and install

- [下载链接](https://github.com/AXERA-TECH/pyaxengine/releases/latest)
- Copy the `axengine-x.x.x-py3-none-any.whl` onto your board and install it using `pip install axengine-x.x.x-py3-none-any.whl`.

### Simple example

This example requires PIL and OpenCV. Install them with `pip install pillow opencv-python-headless`. The `opencv-python-headless` package is a headless build that does not depend on a GUI; the non-headless build requires OpenGL ES, which may not be available in the runtime environment.

```python
Copy [classification.py](https://github.com/AXERA-TECH/pyaxengine/blob/main/examples/classification.py) to the device and run it.

```bash
root@ax650:~/samples# python3 classification.py -m /opt/data/npu/models/mobilenetv2.axmodel -i /opt/data/npu/images/cat.jpg
[INFO] Available providers:  ['AXCLRTExecutionProvider', 'AxEngineExecutionProvider']
[INFO] Using provider: AxEngineExecutionProvider
[INFO] Chip type: ChipType.MC50
[INFO] VNPU type: VNPUType.DISABLED
[INFO] Engine version: 2.10.1s
[INFO] Model type: 0 (single core)
[INFO] Compiler version: 1.2-patch2 7e6b2b5f
  ------------------------------------------------------
  Top 5 Predictions:
    Class Index: 282, Score: 9.774
    Class Index: 278, Score: 8.981
    Class Index: 277, Score: 8.453
    Class Index: 281, Score: 8.321
    Class Index: 287, Score: 7.924
  ------------------------------------------------------
  min =   0.890 ms   max =   22.417 ms   avg =   1.119 ms
  ------------------------------------------------------
```

The example also demonstrates how to switch compute devices. It can run on development boards such as **AX650/AX630C** or on the AX650 M.2 accelerator card.

Use the `-p` parameter to select the compute provider: `-p AxEngineExecutionProvider` uses the on-board NPU, while `-p AXCLRTExecutionProvider` uses the M.2 accelerator card.
Note: When using the M.2 accelerator card, ensure the card is installed in the host and the appropriate drivers are installed. See: [AXCL docs](https://axcl-docs.readthedocs.io/zh-cn/latest/).

```bash
root@ax650:~/samples# python3 classification.py -m /opt/data/npu/models/mobilenetv2.axmodel -i /opt/data/npu/images/cat.jpg -p AXCLRTExecutionProvider
[INFO] Available providers:  ['AXCLRTExecutionProvider', 'AxEngineExecutionProvider']
[INFO] Using provider: AXCLRTExecutionProvider
[INFO] SOC Name: AX650N
[INFO] VNPU type: VNPUType.DISABLED
[INFO] Compiler version: 1.2-patch2 7e6b2b5f
  ------------------------------------------------------
  Top 5 Predictions:
    Class Index: 282, Score: 9.774
    Class Index: 278, Score: 8.981
    Class Index: 277, Score: 8.453
    Class Index: 281, Score: 8.321
    Class Index: 287, Score: 7.924
  ------------------------------------------------------
  min =   1.587 ms   max =   12.624 ms   avg =   1.718 ms
  ------------------------------------------------------
root@ax650:~/samples# python3 classification.py -m /opt/data/npu/models/mobilenetv2.axmodel -i /opt/data/npu/images/cat.jpg -p AxEngineExecutionProvider
[INFO] Available providers:  ['AXCLRTExecutionProvider', 'AxEngineExecutionProvider']
[INFO] Using provider: AxEngineExecutionProvider
[INFO] Chip type: ChipType.MC50
[INFO] VNPU type: VNPUType.DISABLED
[INFO] Engine version: 2.10.1s
[INFO] Model type: 0 (single core)
[INFO] Compiler version: 1.2-patch2 7e6b2b5f
  ------------------------------------------------------
  Top 5 Predictions:
    Class Index: 282, Score: 9.774
    Class Index: 278, Score: 8.981
    Class Index: 277, Score: 8.453
    Class Index: 281, Score: 8.321
    Class Index: 287, Score: 7.924
  ------------------------------------------------------
  min =   0.897 ms   max =   22.542 ms   avg =   1.125 ms
  ------------------------------------------------------
```

## Community contributors

- [zylo117](https://github.com/zylo117): 提供了基于 cffi 的 AXCL Runtime Python API 实现
- [nnn](https://github.com/nnn112358)，[HongJie Li](https://github.com/techshoww) 和 [Shinichi Tanaka](https://github.com/s1tnk) 报告 cffi 的使用问题，[Shinichi Tanaka](https://github.com/s1tnk) 提供了解决方案


## Related projects

- [ax-samples](https://github.com/AXERA-TECH/ax-samples)
- [ax-llm](https://github.com/AXERA-TECH/ax-llm)
- [Pulsar2](https://pulsar2-docs.readthedocs.io/zh-cn/latest/)
- [AXCL](https://axcl-docs.readthedocs.io/zh-cn/latest/)
- [pyAXCL](https://github.com/AXERA-TECH/pyaxcl)