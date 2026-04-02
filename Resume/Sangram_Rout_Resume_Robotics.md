# Sangram Rout | Robotics & AI Engineer
sangram.kr.rout@gmail.com • +44 78537 84922 • UK Work Authorization (HPI Visa)
[LinkedIn](https://linkedin.com) • [GitHub](https://github.com/skr3178) • [Portfolio](https://skr3178.github.io)

---

## PROFESSIONAL SUMMARY

Robotics & Mechanical Engineer specializing in reinforcement learning, humanoid locomotion, and robot simulation. Strong track record building end-to-end RL pipelines (PPO, SAC, TD-MPC) and deploying them in IsaacSim on real humanoid platforms (Unitree G1). Published in Nature Scientific Reports, awarded a U.S. patent, and co-PI on a $1.3M DOE-funded project. Combines deep ML/AI expertise with hands-on mechanical and embedded systems experience.

---

## CORE SKILLS

| Category | Skills |
|----------|--------|
| **Robotics & RL** | PPO, SAC, TD-MPC, AMP/Motion Imitation, Humanoid Locomotion, Reward Design, Multi-Agent RL, HiL Training |
| **Simulation & ROS** | IsaacSim (Isaac Lab), MuJoCo, Gazebo, ROS2 (Nav2, SLAM, URDF), 3D Gaussian Splatting |
| **ML / AI** | PyTorch, Stable Baselines3, World Models (DreamerV3/V4), VLA (Pi0, Dot Policy), Diffusion Models, Vision-Language Models |
| **Programming** | Python, C++ (basics), Pandas, Git, Data Visualization, ONNX model compression |
| **Mechanical** | SolidWorks, Ansys (FEA/Thermal), CAD/CAE, GD&T, ASME/EN standards, PCB design |

---

## SELECTED ROBOTICS & AI PROJECTS

### Humanoid & Locomotion

**Unitree G1 RL Locomotion** | *[GitHub](https://github.com/skr3178/SO101-IsaacSim)*
- Designed reward functions for humanoid gait and running behaviors trained in Isaac Lab/IsaacSim
- Integrated motion retargeting from MikuMiku Dance data to Unitree humanoid
- Implemented PPO-based training pipeline for sim-to-real locomotion transfer

**Humanoid Digital Twin** | *[GitHub](https://github.com/skr3178/humanoid-twin-world-model)*
- Built real-time 3D Gaussian Splatting scene with Unitree G1 controlled via keyboard
- Integrated world model API for interactive cobblestone path simulation
- Created browser-based split-screen viewer for digital twin visualization

**DeepMimic / AMP: Motion Imitation** | *[GitHub](https://github.com/skr3178/AMP_MUL)*
- Implemented Adversarial Motion Priors algorithm from motion capture data in Isaac Gym
- Built discriminator network distinguishing agent-generated vs reference motion
- Trained humanoid characters to imitate complex MoCap animations using PPO + AMP rewards

---

### World Models & Generative AI

**DreamerV4** | *[GitHub](https://github.com/skr3178/DreamerV4)*
- Open-source reimplementation of world model architecture on compact MineRL dataset
- Implemented tokenizer, dynamics model, and RL components under limited compute (RTX 3060)
- Achieved pixel prediction capability with visual reconstruction validation

**Genie: Generative Interactive Environments** | *[GitHub](https://github.com/skr3178/Genie_google)*
- Scratch implementation of Genie world model for learning latent actions from unlabeled video
- Trained video tokenizer (ST-ViViT), latent action model (20-layer transformer), and dynamics model
- Published pre-trained checkpoints to HuggingFace for Pong environment generation

**CoPilot4D: LiDAR Future Prediction** | *[GitHub](https://github.com/skr3178/copilot4D)*
- Open-source reimplementation of 3-second LiDAR future prediction model
- Implemented U-Net tokenizer with VQ codebook and world model for BEV prediction
- Published checkpoints to HuggingFace with 84.32% validation accuracy

---

### Perception & Trajectory

**TopoDiffuser: Multimodal Trajectory Prediction** | *[GitHub](https://github.com/skr3178/TopoDiffuser)*
- Diffusion-based framework for trajectory prediction using LiDAR, camera, and HD maps
- Achieved LiDAR-only prediction within 1m accuracy (82.5% hit rate) on KITTI dataset
- Published pre-trained encoder and diffusion checkpoints to HuggingFace

**ACTION100M: Hierarchical Video Action Annotation** | *[GitHub](https://github.com/skr3178/ACTION100M)*
- Implemented pipeline for generating hierarchical video action annotations
- Processed video data for action recognition and temporal segmentation

---

### Policy Learning & Vision-Language-Action (VLA)

**Pi0 VLA: Flow Matching for Robotics** | *[GitHub](https://github.com/skr3178/openpi)*
- Deep implementation study of state-of-the-art VLA model using flow matching
- Implemented action chunking, action embeddings, and continuous action generation
- Explored flow matching advantages over diffusion for high-frequency robot control

**Dot Policy: Decoder-only Transformer VLA** | *[GitHub](https://github.com/skr3178/Dot_policy)*
- Created vision-language-action model predicting 20-step action sequences
- Trained on PushT environment with ResNet encoder and transformer decoder
- Achieved normalized loss reduction through proper action normalization

**Eureka: LLM-based Reward Design** | *[GitHub](https://github.com/skr3178/Eureka)*
- Repurposed LLM-based reward formulation for dexterous piano-playing robot
- Compared LLM-generated rewards vs hand-tuned rewards for Humanoid and Hand environments
- Resolved CUDA memory issues and implemented sequential simulation analysis

**Human-in-the-Loop RL** | *[GitHub](https://github.com/skr3178/gym-hil)*
- Implemented pick-and-place training with keyboard-driven human intervention
- Built on LeRobot/HuggingFace framework with SAC policy and ResNet vision encoder
- Created continuous keyboard control for real-time policy correction

---

## EXPERIENCE

**Mechanical Engineer** | VSParticle, Delft, Netherlands | *Aug 2024 – Apr 2025*
- Co-led development of a multi-material nano-printer for novel material synthesis
- Reimplemented gas recycling system using microcontrollers, sensors & valves
- Designed custom Faraday enclosures and PCB layouts for electrocatalysis substrate holders

**Research Assistant – Human Computer Interaction** | Northwestern University, Evanston, IL | *Jul 2023 – Jan 2024*
- Prototyped mechanical logic gates (AND, XOR, NOR) using nitinol shape-memory actuators
- Demonstrated functional half/full adder circuits with 3D-printed parts & copper-plated contacts

**Mechanical Engineer** | Heliogen, Pasadena, CA | *Aug 2021 – Apr 2023*
- Led CAD/FEA (thermal & stress) design of a multi-ton 2 m solar particle receiver
- Conducted Life Cycle Analysis for 26.6 MW solar plant; identified CO₂ reduction strategies
- Analyzed and visualized 5M+ wind datapoints to optimize receiver particle loss
- Designed a novel particle valve — contributed to a co-PI role on $1.3M DOE funding

**Mechanical Design Engineer** | EagleBurgmann Expansion Joints, Denmark | *Oct 2014 – Aug 2019*
- Designed large-scale expansion joints rated to 800°C & 200 bar
- Applied ASME & EN standards for high-pressure components; achieved $2M+ annual cost impact

**Powertrain Engineer (Formula SAE)** | Ashwa Racing, Bangalore, India | *Jan 2011 – Apr 2014*
- Designed intake/exhaust system for 600cc engine — delivered 40% gains in power and torque

---

## EDUCATION

**M.S. Mechanical Engineering (Robotics Focus)** | Northwestern University, Evanston, IL | *2019 – 2021*
- Thesis: Soft Robotics Design & Simulation
- Coursework: Reinforcement Learning, Control Systems, Robot Manipulation, Machine Dynamics

---

## PUBLICATIONS, PATENTS & CERTIFICATIONS

- **Nature Scientific Reports (2022)**: KnitMorphs – Numerical Simulation of Knit-Based Morphable Structures
- **U.S. Patent (approved)**: Systems & Methods for Dieless Composite Forming
- **NVIDIA GTC 2025**: Building AI Agents with Multimodal Models (certified)
- **DOE Co-PI**: $1.3M project for high-temperature particle in-line mass flow sensor
