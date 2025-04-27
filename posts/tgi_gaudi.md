---
date: '2025-03-28T11:04:45+01:00'
title: '🚀 My Work on Accelerating LLM Inference with TGI on Intel Gaudi'
---

I'm thrilled to share my latest project: adding native integration of Intel Gaudi hardware support directly into Text Generation Inference (TGI), the production-ready serving solution for Large Language Models (LLMs). This integration brings the power of Intel's specialized AI accelerators to the high-performance inference stack, enabling more deployment options for the open-source AI community 🎉

## ✨ What I've Accomplished 

I've fully integrated Gaudi support into TGI's main codebase in PR [#3091](https://github.com/huggingface/text-generation-inference/pull/3091). Previously, we had to maintain a separate fork for Gaudi devices at [tgi-gaudi](https://github.com/huggingface/tgi-gaudi). This was cumbersome for users and prevented supporting the latest TGI features at launch. By leveraging the new [TGI multi-backend architecture](https://huggingface.co/blog/tgi-multi-backend), I've made it possible to support Gaudi directly on TGI – no more dealing with a custom repository 🙌

My integration supports Intel's full line of [Gaudi hardware](https://www.intel.com/content/www/us/en/developer/platform/gaudi/develop/overview.html):
- Gaudi1 💻
- Gaudi2 💻💻
- Gaudi3 💻💻💻

You can find more information on Gaudi hardware on [Intel's Gaudi product page](https://www.intel.com/content/www/us/en/developer/platform/gaudi/develop/overview.html).

## 🌟 Why I'm Proud of This Work 

The Gaudi backend for TGI provides several key benefits that I'm excited about:
- Hardware Diversity 🔄: Creating more options for deploying LLMs in production beyond traditional GPUs
- Cost Efficiency 💰: Gaudi hardware often provides compelling price-performance for specific workloads
- Production-Ready ⚙️: All the robustness of TGI (dynamic batching, streamed responses, etc.) now available on Gaudi
- Model Support 🤖: Run popular models like Llama 3.1, Mixtral, Mistral, and more on Gaudi hardware
- Advanced Features 🔥: Support for multi-card inference (sharding), vision-language models, and FP8 precision

## 🚦 How to Use My TGI Implementation on Gaudi 

If you'd like to try my work, the easiest way to run TGI on Gaudi is to use the official Docker image. You'll need to run the image on a Gaudi hardware machine. Here's a basic example to get you started: 

```bash
model=meta-llama/Meta-Llama-3.1-8B-Instruct 
volume=$PWD/data # share a volume with the Docker container to avoid downloading weights every run 
hf_token=YOUR_HF_ACCESS_TOKEN

docker run --runtime=habana --cap-add=sys_nice --ipc=host \
 -p 8080:80 \
 -v $volume:/data \
 -e HF_TOKEN=$hf_token \
 -e HABANA_VISIBLE_DEVICES=all \
 ghcr.io/huggingface/text-generation-inference:3.2.1-gaudi \
 --model-id $model 
```

Once the server is running, you can send inference requests: 

```bash
curl 127.0.0.1:8080/generate
 -X POST
 -d '{"inputs":"What is Deep Learning?","parameters":{"max_new_tokens":32}}'
 -H 'Content-Type: application/json'
```

For comprehensive documentation on using TGI with Gaudi, including how-to guides and advanced configurations, check out the new dedicated [Gaudi backend documentation](https://huggingface.co/docs/text-generation-inference/backends/gaudi) I created.

## 🎉 Optimized Modles

The following models are optimized for both single and multi-card configurations. This means these models run as fast as possible on Intel Gaudi. The modeling code is specifically optimized to target Intel Gaudi hardware, ensuring the best performance and fully utilizing Gaudi's capabilities:

- Llama 3.1 (8B and 70B)
- Llama 3.3 (70B)
- Llama 3.2 Vision (11B)
- Mistral (7B)
- Mixtral (8x7B)
- CodeLlama (13B)
- Falcon (180B)
- Qwen2 (72B) 
- Starcoder and Starcoder2 
- Gemma (7B) 
- Llava-v1.6-Mistral-7B 
- Phi-2

🏃‍♂️ We have also implemented advanced features on Gaudi hardware, such as FP8 quantization thanks to [Intel Neural Compressor (INC)](https://docs.habana.ai/en/latest/PyTorch/Inference_on_PyTorch/Quantization/Inference_Using_FP8.html), enabling even greater performance optimizations.

✨ What's next on the TGI Gaudi roadmap? We are working on expanding the model lineup with cutting-edge additions including DeepSeek-r1/v3, QWen-VL, and more powerful models to power your AI applications! 🚀

## 💪 Connect With Me About This Project 

I'd love to hear your feedback if you try out the TGI implementation on Gaudi hardware. The full documentation is available in the [TGI Gaudi backend documentation](https://huggingface.co/docs/text-generation-inference/backends/gaudi). 📚 

If you're interested in contributing or have questions about my implementation, feel free to reach out or open an issue with your feedback on GitHub. 🤝 

By bringing Intel Gaudi support directly into TGI, I'm proud to contribute to providing flexible, efficient, and production-ready tools for deploying LLMs. I'm excited to see what the community will build with this capability! 🎉