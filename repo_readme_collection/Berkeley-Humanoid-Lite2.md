### Training code for reinforcement learning

```
python ./scripts/rsl_rl/train.py --task Velocity-Berkeley-Humanoid-Lite-v0 --headless
```
```
python ./scripts/rsl_rl/play.py --task Velocity-Berkeley-Humanoid-Lite-v0 --num_envs 16
```
```
export PATH="/workspace/isaaclab/_isaac_sim/kit/python/bin:$PATH" 
&&
export ACCEPT_EULA=Y
&& python3 ./scripts/rsl_rl/train_fast.py --task Velocity-Berkeley-Humanoid-Lite-v0 --headless --num_envs 4096 --max_iterations 100
```

```
cd /workspace/isaaclab_extension_template && export PATH="/workspace/isaaclab/_isaac_sim/kit/python/bin:$PATH" && export ACCEPT_EULA=Y && python3 ./scripts/rsl_rl/play.py --task Velocity-Berkeley-Humanoid-Lite-v0 --num_envs 16 --video --checkpoint model_99.pt --load_run "2025-09-02_17-53-05" --headless
```
To start a new docker container called isaac_sim2

```
docker compose --env-file .env.base --file docker-compose.enhanced.yaml up isaac-lab-template_2
```

```
docker run --name isaac-lab-template_2 --entrypoint bash -it --runtime=nvidia --gpus all     -e "ACCEPT_EULA=Y"     -e "PRIVACY_CONSENT=Y"     -e "OMNI_KIT_ALLOW_ROOT=1"     --network=host     -v ~/docker/isaac-sim/cache/kit:/isaac-sim/kit/cache:rw     -v ~/docker/isaac-sim/cache/ov:/root/.cache/ov:rw     -v ~/docker/isaac-sim/cache/pip:/root/.cache/pip:rw     -v ~/docker/isaac-sim/cache/glcache:/root/.cache/nvidia/GLCache:rw     -v ~/docker/isaac-sim/cache/computecache:/root/.nv/ComputeCache:rw     -v ~/docker/isaac-sim/logs:/root/.nvidia-omniverse/logs:rw     -v ~/docker/isaac-sim/data:/root/.local/share/ov/data:rw     -v ~/docker/isaac-sim/documents:/root/Documents:rw     -v $(pwd)/..:/workspace/isaaclab_extension_template     isaac-lab-template_2
```

## Import and trained policies on humanoids: 

First lets review the degrees of freedom and comparison:
- Berkeley Humanoid doesn't have fingers which can be controlled.
- Cost is estimated as $4.9K

- Unitree H1 has more degrees of freedom on hands/fingers. 
- Cost estimated as $6K
- However, cost is perhaps not inclusive of software stack which would drive costs up(as per forums).

- Degrees of freedom of URDF/USD file
1. Berkeley lite- humanoid & Biped resp.

![Biped](DOF_Biped.png)
![Humanoid](DOF_Humanoid.png)

2. Unitree H1

![Unitree](DOF_Unitree.png)

Reward function for Humanoids: 

![Model](Model.gif)

![RL_trained_99_steps](rl_video.gif)
