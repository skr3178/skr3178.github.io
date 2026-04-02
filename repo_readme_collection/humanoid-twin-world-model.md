# Attempt to recreate an X-Post implementation I saw earlier


# Unitree Digital simulation of humanoid walking

First download the unitree G1 Assests.
Secondly, clone the Unitree_RL_GYM and Untree_RL_lab repos from github
Third, download the samples from world models which will be used as the reference here: `sample_european_cobblestone/`

## Demo

![Browser split-screen demo](assets/browser-split-screen-demo.gif)

## Run the current split-screen app

Start the browser-based left/right viewer from this repository root:

```bash
export CONDA_NO_PLUGINS=true
source ~/miniconda3/etc/profile.d/conda.sh
conda activate lingbot
export DISPLAY=:0
export XAUTHORITY=/run/user/1000/gdm/Xauthority
export MUJOCO_GL=glfw
cd /media/skr/storage/lingbot/X_posting
python server.py
```

Then open:

```text
http://localhost:8080/X_posting/frontend/index.html
```

Keyboard controls in the browser:

```text
W / Up    forward
S / Down  backward
A / Left  turn left
D / Right turn right
Space     stop
C         toggle left camera mode
```

---

## Repository References

### External GitHub Repos (clone separately)

| Repo | GitHub Link |
|------|-------------|
| Unitree RL Gym | https://github.com/unitreerobotics/unitree_rl_gym.git |
| Unitree RL Lab | https://github.com/unitreerobotics/unitree_rl_lab.git |

Clone into `unitree_g1/`:
```bash
git clone https://github.com/unitreerobotics/unitree_rl_gym.git unitree_g1/unitree_rl_gym
git clone https://github.com/unitreerobotics/unitree_rl_lab.git unitree_g1/unitree_rl_lab
```

### This Project (https://github.com/skr3178/humanoid-twin-world-model)

| File / Folder | Description |
|---------------|-------------|
| `server.py` | Main backend server |
| `viewer_sidebyside.py` | Side-by-side MuJoCo viewer |
| `frontend/index.html` | Frontend UI |
| `frontend/app.js` | Frontend logic |
| `assets/` | README media assets, including the browser split-screen demo GIF |
| `requirements.txt` | Python dependencies |
| `unitree_g1/` | Unitree G1 MuJoCo assets (MJCF/XML + STL meshes) |
| `unitree_g1/g1.xml` | G1 robot MJCF model |
| `unitree_g1/scene.xml` | MuJoCo scene definition |
| `codexplan.md` | Implementation plan |
| `code_running.md` | Instructions to run the code |
| `x_1/` | X (Twitter) post 1 references |
| `x_2/` | X (Twitter) post 2 references |
| `sample_european_cobblestone/` | World model reference samples (large, not tracked in git) |
