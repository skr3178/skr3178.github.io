# DOT Policy: Decoder-Only Transformer for Robot Learning

This repository contains an implementation of a Decoder-Only Transformer (DOT) policy for robot learning, built on top of the LeRobot framework. The project focuses on visuomotor policy learning using ResNet backbones and transformer architectures for robotic manipulation tasks.


First trial the algo wasn't training well. Stuck in the middle. 

ACT policy recommended to train for longer steps. 

I added action chunking based of the ACT policy and it works better now. 

Inspired by work from Ilia (idea and concept was heard, exact implementation was come up with individually) and Nvidia Groot. Decoder only transformer. 

## 🎬 Visual Demonstrations

### Training Progression
![Training Progression](training_progression_collage.gif)
*This GIF shows the training progression of the DOT policy over multiple epochs, demonstrating how the robot learns to perform manipulation tasks through iterative improvement.*

### ACT (Action Chunking Transformer) Performance
![ACT Performance](ACT.gif)
*Demonstrates the Action Chunking Transformer in action, showing how the policy predicts sequences of actions rather than single-step predictions, reducing compounding errors.*

### ResNet Architecture Comparison
![ResNet Comparison](resnet.gif)
*Shows the difference between the original ResNet implementation and the updated multimodal version that handles both images and state information.*

### Trained ResNet Performance
![Trained ResNet](resnet_trained.gif)
*Final performance of the trained ResNet-based policy on manipulation tasks, showcasing the effectiveness of the multimodal approach.*

## 🏗️ Architecture Overview

### Core Components

1. **ResNet Backbone**: 
   - Base model: ResNet-10
   - Processes 96x96x3 RGB images
   - Outputs 128-dimensional features
   - Supports both single-modal (images only) and multi-modal (images + state) configurations

2. **Decoder-Only Transformer**:
   - 8-layer transformer decoder
   - Hidden dimension: 512
   - LoRA adaptation for efficient fine-tuning
   - Cross-attention to ResNet features

3. **Multi-Modal Processing**:
   - **ModuleDict Architecture**: Handles images and state information separately
   - **Images**: Processed through ResNet backbone
   - **State**: 2-dimensional input processed through linear layer
   - **Combined Features**: Concatenated embeddings for transformer input

### Key Features

- **Action Chunking**: Predicts sequences of actions to reduce compounding errors
- **Multi-Camera Support**: Utilizes wrist, front, and top-down cameras for richer scene understanding
- **LoRA Adaptation**: Efficient fine-tuning with minimal parameter updates
- **Temporal Ensembling**: Optional temporal weighting for improved performance

## 📊 Dataset and Training

### Dataset: PushT
- **Source**: LeRobot PushT dataset (`lerobot/pusht`)
- **Task**: Tabletop pushing task with puck manipulation
- **Format**: Parquet files for actions/observations + video files for visual data
- **Demonstrations**: Human teleoperated demonstrations

### Training Configuration
- **Normalization**: 
  - Visual: MEAN_STD normalization
  - State: MIN_MAX normalization
  - Action: MIN_MAX normalization
- **Checkpointing**: Every 50,000 iterations
- **Evaluation**: Every 1,000 iterations

### Training Command
```bash
python dot2/lerobot/lerobot/scripts/train.py
```

## 🔧 Evaluation

### Evaluate Pretrained Policy
```bash
python lerobot/scripts/eval.py \
    --policy.path=lerobot/diffusion_pusht \
    --env.type=pusht \
    --eval.batch_size=10 \
    --eval.n_episodes=10 \
    --use_amp=false \
    --device=cuda
```

### Evaluate Custom Checkpoints
```bash
python lerobot/scripts/eval.py --policy.path={OUTPUT_DIR}/checkpoints/last/pretrained_model
```

## 📁 Project Structure

```
dot2/
├── lerobot/                    # Main LeRobot framework
│   ├── lerobot/
│   │   ├── common/
│   │   │   ├── policies/       # Policy implementations (ACT, DOT, ResNet)
│   │   │   ├── datasets/       # Dataset loading utilities
│   │   │   └── envs/          # Environment configurations
│   │   ├── scripts/           # Training and evaluation scripts
│   │   └── configs/           # Configuration files
│   └── outputs/               # Training outputs and checkpoints
├── Reference/                  # Documentation and reference materials
├── Resnet_old/               # Legacy ResNet implementations
└── *.gif                     # Visual demonstrations
```

## 🎯 Key Advantages

1. **Reduced Compounding Errors**: Action chunking predicts sequences rather than single steps
2. **Multi-Modal Learning**: Combines visual and proprioceptive information effectively
3. **Efficient Training**: LoRA adaptation minimizes trainable parameters
4. **Robust Performance**: Multi-camera setup provides redundancy and richer information

## 📈 Performance Metrics

The policy is evaluated on:
- **Success Rate**: Task completion percentage
- **Reward**: Cumulative reward per episode
- **Efficiency**: Steps to completion
- **Robustness**: Performance across different scenarios

## 🔬 Technical Details

### Input Processing
- **Images**: [batch_size, 3, 96, 96] RGB tensors
- **State**: [batch_size, 2] proprioceptive information
- **Actions**: [batch_size, action_dim] robot control commands

### Output Generation
- **Autoregressive**: Generates action sequences token by token
- **Conditioned**: On visual and state information
- **Chunked**: Predicts multiple timesteps per inference

## 📚 References

- [LeRobot Framework](https://github.com/huggingface/lerobot)
- [ACT: Action Chunking Transformer](https://arxiv.org/abs/2206.10093)
- [Diffusion Policy](https://arxiv.org/abs/2303.04137)
- [PushT Dataset](https://huggingface.co/datasets/lerobot/pusht)

## 🤝 Contributing

This project builds upon the LeRobot framework. For contributions and improvements, please refer to the main LeRobot repository guidelines.

---

*This implementation demonstrates the effectiveness of decoder-only transformers in robotic manipulation tasks, combining the power of visual understanding with sequential action prediction.*
