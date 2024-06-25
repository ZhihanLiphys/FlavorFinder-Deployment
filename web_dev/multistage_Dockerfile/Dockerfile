# Stage 1: Build Stage
# FROM nvidia/cuda:11.3.0-cudnn8-devel-ubuntu20.04 as build
FROM nvidia/cuda:12.5.0-devel-ubuntu20.04 as build

# Set environment variables for CUDA and cuDNN
ENV CUDA_VERSION=11.3
ENV CUDNN_VERSION=8

# Install Python and other build dependencies
RUN apt-get update && apt-get install -y \
    python3-pip \
    python3-dev \
    build-essential \
    && rm -rf /var/lib/apt/lists/*

# Install Python dependencies for building the application

RUN pip3 install torch==2.0.0+cu117 -f https://download.pytorch.org/whl/cu113/torch_stable.html
RUN pip3 install transformers

# Copy application code to the build stage
COPY . /app
WORKDIR /app

# Optional: Run any build steps needed for your application
# RUN python3 setup.py install

# Stage 2: Runtime Stage
# FROM nvidia/cuda:11.3.0-cudnn8-runtime-ubuntu20.04
FROM nvidia/cuda:12.5.0-runtime-ubuntu20.04

# Set environment variables for CUDA and cuDNN
ENV CUDA_VERSION=12.5
#ENV CUDNN_VERSION=8

# Install Python and minimal runtime dependencies
RUN apt-get update && apt-get install -y \
    python3-pip \
    && rm -rf /var/lib/apt/lists/*

# Install Python runtime dependencies
RUN pip3 install torch==2.0.0+cu117 -f https://download.pytorch.org/whl/cu117/torch_stable.html
RUN pip3 install transformers

# Copy only the necessary files from the build stage
COPY --from=build /app /app
WORKDIR /app

# Command to run your application
CMD ["python3", "app_test.py"]

