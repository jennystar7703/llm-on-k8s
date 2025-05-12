# Example Dockerfile (Test)

FROM nvidia/cuda:12.1.1-devel-ubuntu22.04

ENV DEBIAN_FRONTEND=noninteractive

# Install Python, pip, and other system dependencies
RUN apt-get update && apt-get install -y --no-install-recommends \
    python3-pip \
    python3-venv \
    git \
    && rm -rf /var/lib/apt/lists/*

# Install vLLM and FastAPI/Uvicorn for an API server
# Pin versions for stability - Requirements (requirements.txt for later)
RUN pip3 install vllm fastapi uvicorn[standard] huggingface_hub

# Create a non-root user (good practice)
RUN useradd -ms /bin/bash vllmuser
USER vllmuser
WORKDIR /home/vllmuser

# Expose vLLM port to listen on 
EXPOSE 8000

# Run vLLM
# Adjust CMD - model name, tensor-parallel-size, and other vLLM flags as needed
CMD ["python3", "-m", "vllm.entrypoints.api_server", \
     "--model", "deepseek-ai/DeepSeek-R1-Distill-Qwen-7B", \
     "--tensor-parallel-size", "8", \
     "--host", "0.0.0.0", \
     "--port", "8000", \
     "--gpu-memory-utilization", "0.90"]