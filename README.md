<div align="center">
<a href="https://osher.com.au/">
<img src="web/src/assets/logo-with-text.png" width="520" alt="Osher Digital logo">
</a>
</div>

<p align="center">
  <a href="./README.md"><img alt="README in English" src="https://img.shields.io/badge/English-DBEDFA"></a>
</p>

<h4 align="center">
  <a href="https://osher.com.au">Website</a> |
  <a href="mailto:info@osher.com.au">Contact</a>
</h4>

#

<details open>
<summary><b>📕 Table of Contents</b></summary>

- 💡 [What is Osher Digital?](#-what-is-osher-digital)
- 🌟 [Key Features](#-key-features)
- 🔎 [System Architecture](#-system-architecture)
- 🎬 [Get Started](#-get-started)
- 🔧 [Configurations](#-configurations)
- 🔧 [Build a docker image without embedding models](#-build-a-docker-image-without-embedding-models)
- 🔧 [Build a docker image including embedding models](#-build-a-docker-image-including-embedding-models)
- 🔨 [Launch service from source for development](#-launch-service-from-source-for-development)
- 📚 [Documentation](#-documentation)

</details>

## 💡 What is Osher Digital?

Osher Digital is a powerful Retrieval-Augmented Generation (RAG) engine that combines cutting-edge RAG with Agent capabilities to create a superior context layer for LLMs. It offers a streamlined RAG workflow adaptable to enterprises of any scale. Powered by a converged context engine and pre-built agent templates, Osher Digital enables developers to transform complex data into high-fidelity, production-ready AI systems with exceptional efficiency and precision.

## 🌟 Key Features

### 🍭 **"Quality in, quality out"**

- [Deep document understanding](./deepdoc/README.md)-based knowledge extraction from unstructured data with complicated formats.
- Finds "needle in a data haystack" of literally unlimited tokens.

### 🍱 **Template-based chunking**

- Intelligent and explainable.
- Plenty of template options to choose from.

### 🌱 **Grounded citations with reduced hallucinations**

- Visualization of text chunking to allow human intervention.
- Quick view of the key references and traceable citations to support grounded answers.

### 🍔 **Compatibility with heterogeneous data sources**

- Supports Word, slides, excel, txt, images, scanned copies, structured data, web pages, and more.

### 🛀 **Automated and effortless RAG workflow**

- Streamlined RAG orchestration catered to both personal and large businesses.
- Configurable LLMs as well as embedding models.
- Multiple recall paired with fused re-ranking.
- Intuitive APIs for seamless integration with business.

## 🔎 System Architecture

<div align="center" style="margin-top:20px;margin-bottom:20px;">
<img src="https://github.com/infiniflow/ragflow/assets/12318111/d6ac5664-c237-4200-a7c2-a4a00691b485" width="1000"/>
</div>

## 🎬 Get Started

### 📝 Prerequisites

- CPU >= 4 cores
- RAM >= 16 GB
- Disk >= 50 GB
- Docker >= 24.0.0 & Docker Compose >= v2.26.1
- [gVisor](https://gvisor.dev/docs/user_guide/install/): Required only if you intend to use the code executor (sandbox) feature.

> [!TIP]
> If you have not installed Docker on your local machine (Windows, Mac, or Linux), see [Install Docker Engine](https://docs.docker.com/engine/install/).

### 🚀 Start up the server

1. Ensure `vm.max_map_count` >= 262144:

   > To check the value of `vm.max_map_count`:
   >
   > ```bash
   > $ sysctl vm.max_map_count
   > ```
   >
   > Reset `vm.max_map_count` to a value at least 262144 if it is not.
   >
   > ```bash
   > # In this case, we set it to 262144:
   > $ sudo sysctl -w vm.max_map_count=262144
   > ```
   >
   > This change will be reset after a system reboot. To ensure your change remains permanent, add or update the `vm.max_map_count` value in **/etc/sysctl.conf** accordingly:
   >
   > ```bash
   > vm.max_map_count=262144
   > ```

2. Clone the repo:

   ```bash
   $ git clone https://github.com/osher-digital/osher-digital.git
   ```

3. Start up the server using the pre-built Docker images:

> [!CAUTION]
> All Docker images are built for x86 platforms. We don't currently offer Docker images for ARM64.
> If you are on an ARM64 platform, you'll need to build a Docker image compatible with your system.

   > The command below uses the Docker configuration to start the server.

   ```bash
   $ cd osher-digital/docker
   # Use CPU for embedding and DeepDoc tasks:
   $ docker compose -f docker-compose.yml up -d

   # To use GPU to accelerate embedding and DeepDoc tasks:
   # docker compose -f docker-compose-gpu.yml up -d
   ```

4. Check the server status after having the server up and running:

   ```bash
   $ docker logs -f osher-digital-server
   ```

   _The following output confirms a successful launch of the system:_

   ```bash

         ____   ___    ______ ______ __
        / __ \ /   |  / ____// ____// /____  _      __
       / /_/ // /| | / / __ / /_   / // __ \| | /| / /
      / _, _// ___ |/ /_/ // __/  / // /_/ /| |/ |/ /
     /_/ |_|/_/  |_|\____//_/    /_/ \____/ |__/|__/

    * Running on all addresses (0.0.0.0)
   ```

   > If you skip this confirmation step and directly log in, your browser may prompt a `network anormal` error because, at that moment, the system may not be fully initialized.

5. In your web browser, enter the IP address of your server and log in.
   > With the default settings, you only need to enter `http://IP_OF_YOUR_MACHINE` (**sans** port number) as the default HTTP serving port `80` can be omitted when using the default configurations.

6. In [service_conf.yaml.template](./docker/service_conf.yaml.template), select the desired LLM factory in `user_default_llm` and update the `API_KEY` field with the corresponding API key.

   _The show is on!_

## 🔧 Configurations

When it comes to system configurations, you will need to manage the following files:

- [.env](./docker/.env): Keeps the fundamental setups for the system, such as `SVR_HTTP_PORT`, `MYSQL_PASSWORD`, and `MINIO_PASSWORD`.
- [service_conf.yaml.template](./docker/service_conf.yaml.template): Configures the back-end services. The environment variables in this file will be automatically populated when the Docker container starts. Any environment variables set within the Docker container will be available for use, allowing you to customize service behavior based on the deployment environment.
- [docker-compose.yml](./docker/docker-compose.yml): The system relies on [docker-compose.yml](./docker/docker-compose.yml) to start up.

> The [./docker/README](./docker/README.md) file provides a detailed description of the environment settings and service configurations which can be used as `${ENV_VARS}` in the [service_conf.yaml.template](./docker/service_conf.yaml.template) file.

To update the default HTTP serving port (80), go to [docker-compose.yml](./docker/docker-compose.yml) and change `80:80` to `<YOUR_SERVING_PORT>:80`.

Updates to the above configurations require a reboot of all containers to take effect:

> ```bash
> $ docker compose -f docker-compose.yml up -d
> ```

### Switch doc engine from Elasticsearch to Infinity

The system uses Elasticsearch by default for storing full text and vectors. To switch to [Infinity](https://github.com/infiniflow/infinity/), follow these steps:

1. Stop all running containers:

   ```bash
   $ docker compose -f docker/docker-compose.yml down -v
   ```

> [!WARNING]
> `-v` will delete the docker container volumes, and the existing data will be cleared.

2. Set `DOC_ENGINE` in **docker/.env** to `infinity`.

3. Start the containers:

   ```bash
   $ docker compose -f docker-compose.yml up -d
   ```

> [!WARNING]
> Switching to Infinity on a Linux/arm64 machine is not yet officially supported.

## 🔧 Build a Docker image without embedding models

This image is approximately 2 GB in size and relies on external LLM and embedding services.

```bash
git clone https://github.com/osher-digital/osher-digital.git
cd osher-digital/
docker build --platform linux/amd64 --build-arg LIGHTEN=1 -f Dockerfile -t osher-digital/osher-digital:latest-slim .
```

## 🔧 Build a Docker image including embedding models

This image is approximately 9 GB in size. As it includes embedding models, it relies on external LLM services only.

```bash
git clone https://github.com/osher-digital/osher-digital.git
cd osher-digital/
docker build --platform linux/amd64 -f Dockerfile -t osher-digital/osher-digital:latest .
```

## 🔨 Launch service from source for development

1. Install `uv` and `pre-commit`, or skip this step if they are already installed:

   ```bash
   pipx install uv pre-commit
   ```

2. Clone the source code and install Python dependencies:

   ```bash
   git clone https://github.com/osher-digital/osher-digital.git
   cd osher-digital/
   uv sync --python 3.10 --all-extras # install dependent python modules
   uv run download_deps.py
   pre-commit install
   ```

3. Launch the dependent services (MinIO, Elasticsearch, Redis, and MySQL) using Docker Compose:

   ```bash
   docker compose -f docker/docker-compose-base.yml up -d
   ```

   Add the following line to `/etc/hosts` to resolve all hosts specified in **docker/.env** to `127.0.0.1`:

   ```
   127.0.0.1       es01 infinity mysql minio redis sandbox-executor-manager
   ```

4. If you cannot access HuggingFace, set the `HF_ENDPOINT` environment variable to use a mirror site:

   ```bash
   export HF_ENDPOINT=https://hf-mirror.com
   ```

5. If your operating system does not have jemalloc, please install it as follows:

   ```bash
   # ubuntu
   sudo apt-get install libjemalloc-dev
   # centos
   sudo yum install jemalloc
   # mac
   sudo brew install jemalloc
   ```

6. Launch backend service:

   ```bash
   source .venv/bin/activate
   export PYTHONPATH=$(pwd)
   bash docker/launch_backend_service.sh
   ```

7. Install frontend dependencies:

   ```bash
   cd web
   npm install
   ```

8. Launch frontend service:

   ```bash
   npm run dev
   ```

   _The following output confirms a successful launch of the system:_

   ![](https://github.com/user-attachments/assets/0daf462c-a24d-4496-a66f-92533534e187)

9. Stop front-end and back-end service after development is complete:

   ```bash
   pkill -f "ragflow_server.py|task_executor.py"
   ```

## 📚 Documentation

For more information, please contact us at [info@osher.com.au](mailto:info@osher.com.au).

## 📄 License

This project is licensed under the Apache License 2.0. See the [LICENSE](LICENSE) file for details.

---

© 2025 Osher Digital. All rights reserved.
