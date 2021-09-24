# JupyterHub deployment in use [internally] at SensorLab at Jožef Stefan Institute

The project is based on [defeo/jupyterhub-docker](https://github.com/defeo/jupyterhub-docker/) and inspired by [blog #1](https://tustunkok.github.io/tutorial/notes-to-myself/vps/2020/05/16/how-to-create-a-gpu-powered-containerized-multi-user-jupyterhub-research-server.html), [blog #2](https://hackmd.io/@DanielChen/Sy81P-Aw4?type=view) with modifications for our usecase.

## Features

- Containerized single user Jupyter servers, using [DockerSpawner](https://github.com/jupyterhub/dockerspawner);
- Central authentication using [JupyterHub's NativeAuthenticator](https://github.com/jupyterhub/nativeauthenticator);
- User data persistence;
- Shared folder among users;
- Access to NVIDIA GPUs on host workstation;

## TODOs

- [ ] Use Traefik as reverse-proxy (instead of current NodeJS's dynamic-proxy);
- [x] GPU passthrough into container;
  - [x] Sort out GPU access for TensorFlow [help](https://hackmd.io/@DanielChen/Sy81P-Aw4?type=view);
  - [ ] Sort out GPU reservation and/or sharing (Options: vGPU, work queue);
- [ ] Decide whether we have user data persistence in Docker volumes or directly on a hard drive;
  - [ ] Docker volumes [backup script](https://gist.github.com/pirate/265e19a8a768a48cf12834ec87fb0eed);
- [ ] Migrate from `docker-compose` --> `docker-swarm` ([example](https://github.com/jupyterhub/dockerspawner/blob/main/examples/swarm/docker-compose.yml));
- [x] Take advantage of [docker-stacks](https://github.com/jupyter/docker-stacks) containers;
- [ ] Docker image with [C++ REPL kernel](https://blog.jupyter.org/interactive-workflows-for-c-with-jupyter-fe9b54227d92);

### Installation prerequirements

- install `docker` and `docker-compose`;
- install `nvidia-container-toolkit`;
- ~~install `nvidia-docker2` (see [discussion](https://github.com/NVIDIA/nvidia-docker/issues/1268#issuecomment-632692949));~~

### Run

Once you are ready, build and launch the application with

```bash
# Build Docker containers
docker-compose build

# Start whole system
docker-compose up -d
```

Read the [Docker Compose manual](https://docs.docker.com/compose/) to
learn how to manage your application.

### Usecases

#### TensorFlow 2.5.x

```bash
# (optional) For TensorFlow 2.5.x to use GPU, we need to install cuDNN 8.1.x (and appropriate CUDA toolkit)
mamba install --yes --quiet cudnn=8.1

# Install TensorFlow from pip (conda/mamba only has versions prior v2.5)
pip install --quiet tensorflow==2.5.*

```

#### pyTorch 1.9.x

```bash
mamba install --yes --quiet cudatoolkit=11.1 pytorch=1.9 torchvision torchaudio -c pytorch -c nvidia -c conda-forge
```
