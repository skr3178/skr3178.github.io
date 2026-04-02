## ISAACGYM

![collage IsaacGym](videos_collage.gif)

Train!
Linux:

```
./isaaclab.sh -p scripts/reinforcement_learning/skrl/train.py --task Isaac-Ant-v0 --headless
```

```
./isaaclab.sh -p scripts/reinforcement_learning/rl_games/train.py --task=Isaac-Cartpole-Direct-v0
```

python3 scripts/reinforcement_learning/rl_games/train.py --task=Isaac-Cartpole-Direct-v0

python3 scripts/reinforcement_learning/rl_games/train.py --task=Isaac-Cartpole-Direct-v0 --record_video --max_steps=100

##   Action Application Flow
1. _pre_physics_step(actions) - Action Processing
```
def _pre_physics_step(self, actions: torch.Tensor) -> None:
    self.actions = self.action_scale * actions.clone()
```
What happens:
Called once per RL step (before any physics simulation)
Receives raw actions from the policy (typically in range [-1, 1])
Scales actions by self.action_scale (100.0 N from config)
Clones the tensor to avoid modifying the original
Stores processed actions in self.actions for later use
2. _apply_action() - Action Execution
```
def _apply_action(self) -> None:
    self.cartpole.set_joint_effort_target(self.actions, joint_ids=self._cart_dof_idx)
```
What happens:
Called multiple times per RL step (based on decimation)
Applies the stored actions to the actual robot
Targets the cart joint (_cart_dof_idx) specifically
Uses set_joint_effort_target() to apply forces
Why This Two-Step Design?
Decimation = 2 means:
RL step: Every 2 physics timesteps
Physics step: Every 1/120 seconds
So actions are applied for 2 consecutive physics steps
Benefits:
Efficiency: Process actions once, apply multiple times
Consistency: Same action applied for multiple physics steps
Flexibility: Can modify how actions are applied without reprocessing
Example Timeline
This design ensures smooth, consistent control while maintaining computational efficiency.
what is decimation, why scaling and why clone?


1. What is Decimation?
Decimation = 2 means:
RL steps: Every 2 physics timesteps
Physics timesteps: Every 1/120 seconds
Result: Actions are applied for 2 consecutive physics steps
Why use decimation?
Performance: RL algorithms don't need to make decisions every 1/120th of a second
Stability: Actions applied for multiple physics steps create smoother control
Efficiency: Reduces computational overhead of the RL loop
Timeline example:

RL Step 1: _pre_physics_step() → process action
  Physics Step 1: _apply_action() → apply force
  Physics Step 2: _apply_action() → apply same force
RL Step 2: _pre_physics_step() → process new action
  Physics Step 3: _apply_action() → apply new force
  Physics Step 4: _apply_action() → apply same new force

  Physics: 0ms → 8.33ms → 16.67ms → 25ms → 33.33ms
RL:     [Action A]     [Action A]     [Action B]


2. Why Scaling?
Raw actions from policy: Typically in range [-1, 1]
-1: Maximum force left
0: No force
+1: Maximum force right
Action scale = 100.0 N: Converts normalized actions to actual forces
-1 × 100 = -100 N (push cart left)
0 × 100 = 0 N (no force)
+1 × 100 = +100 N (push cart right)
Why scale?
Policy output: Neural networks work better with normalized values
Physical reality: Real forces need actual Newton values
Tunable: Easy to adjust force magnitude without retraining policy
3. Why Clone?
actions.clone() creates a copy of the tensor instead of referencing the original
Without clone:
self.actions = self.action_scale * actions  # DANGEROUS!
# Modifying self.actions could modify the original actions tensor

With clone:
self.actions = self.action_scale * actions.clone()  # SAFE!
# self.actions is independent of the original actions

Why this matters:
Memory safety: Prevents accidental modification of policy outputs
Debugging: Easier to track where changes occur
PyTorch behavior: Tensor operations can sometimes modify in-place
Best practice: Always clone when you need to store tensor data

### Background of isaacland and GPU based computation


### Franka Obs space

![Tensors Multiscene](./Tensors_Multiscene.png)

![franka_Obs](Franka_Obs.png)

![franks rewards](Franka_rewards.png)

To train franka robot:
```
python3 scripts/reinforcement_learning/sb3/train.py --task Isaac-Stack-Cube-Franka-v0 --num_envs 4 --headless --video
```

```
 python3 scripts/reinforcement_learning/sb3/train.py --task Isaac-Cartpole-v0 --num_envs 64 --headless --video --max_iteration 500
```

```
python3 scripts/reinforcement_learning/sb3/train.py --task Isaac-Lift-Cube-Franka-v0 --num_envs 64 --headless --video
```


```
python3 scripts/reinforcement_learning/sb3/train.py --task Isaac-Stack-Cube-So101-v0 --num_envs 4 --headless --video
```

```
./runheadless.sh -v
```

```
python3 scripts/reinforcement_learning/rsl_rl/train.py --task=Isaac-Cartpole-Direct-v0 --livestream 2

LIVESTREAM=0 python3 scripts/reinforcement_learning/rsl_rl/train.py --task=Isaac-Cartpole-Direct-v0 --livestream 2
```

```
python scripts/rsl_rl/train.py --task SO-ARM100-Reach-v0 --headless.
```

Status (Next step): 
Franka robot replaced by Lerobot.
Challenges:
- Lerobot is not on the table top
- Lerobot arm length needs to be adjusted or the positon of the cubes should be such that it is in reach
- Type of control system, position or torque control ??

Get the new one so101 usd and use it for the code.

Changing the USD file is all that was needed. 


Now change the algorithm used. 
No. of envs: 32 
Algorithm used: skrl

Sim to real

```
python3 scripts/reinforcement_learning/skrl/play.py \
  --task Isaac-Lift-Cube-Franka-v0 \
  --checkpoint /workspace/isaaclab/logs/skrl/franka_lift/2025-09-01_05-50-10_ppo_torch/checkpoints/best_agent.pt \
  --num_envs 1 \
  --video \
  --video_length 1200
  ```

  ```
  python3 scripts/reinforcement_learning/skrl/train.py --task Isaac-Lift-Cube-So101-v0 --num_envs 32 --headless --video
  ```lyct

  ![training in progress](rl-video-step-1200.gif)
