# Multi-Turn Reinforcement Learning in RAGEN

This README summarizes the concept, strategy, and algorithm of **multi-turn reinforcement learning (RL)** as described and discussed in relation to **RAGEN.pdf**.

---

## 🧩 1. What is Multi-Turn Reinforcement Learning for LLMs?

**Multi-turn reinforcement learning (RL)** refers to training a large language model (LLM) to perform well over **multi-step (multi-turn) interactions** — not just single responses.

Instead of optimizing for “the best next message,” the model is trained for **“the best sequence of responses or actions”** that lead to successful task completion or coherent conversation.

### Comparison to Standard RLHF

| Aspect | **Single-turn RL (RLHF)** | **Multi-turn RL** |
|--------|----------------------------|-------------------|
| Unit of optimization | One response | Whole conversation or episode |
| Reward signal | Human preference for one reply | Outcome of a full trajectory or dialogue |
| Common use | Tone, politeness, factual correctness | Multi-step planning, dialogue reasoning, agent behavior |
| Example | “Was this answer polite?” | “Did the model successfully complete the task after 6 steps?” |

---

## 🧠 2. Application to Sokoban and Bandit Problems

In **Sokoban** (a multi-step puzzle), each **turn** is a move. The model’s success depends on the entire sequence of moves that leads to solving the puzzle.  
In **bandit problems**, each **turn** is an arm pull — the model learns strategies across rounds to balance exploration vs. exploitation.

So, “multi-turn” means **sequential optimization**: the model must learn how early decisions influence later outcomes.

| Setting | Reward Source | LLM’s Role | “Multi-turn” Meaning |
|----------|----------------|-------------|----------------------|
| Sokoban | Environment score (goal reached) | Agent chooses moves | Each push = one turn |
| Bandit | Payoff of pulled arm | Agent selects arm | Each pull = one turn |
| LLM Dialogue | Judge or task completion | Generates multi-step conversation | Each exchange = one turn |

---

## ⚙️ 3. Reward Structure in Multi-Turn RL

Rewards in multi-turn RL can come from:
1. **Environment-based reward** – task success, numeric performance, correctness.  
2. **LLM-evaluated reward (AI feedback)** – another LLM acts as a *critic* scoring reasoning quality or coherence.  
3. **Hybrid reward** – combination of both.

Example reward formula:
\[
R_{total} = w_1 R_{env} + w_2 R_{LLM}
\]

In RAGEN, both environment and LLM-based evaluators contribute to total reward.

---

## 🧩 4. Training Dynamics

Each **turn** is a full decision-action-feedback step:

1. Observe state or conversation so far.  
2. Generate action or message.  
3. Receive response and (possibly delayed) reward.  
4. Continue until end of episode.  
5. Update the policy based on total episode return.

The model therefore learns **long-horizon strategies** — where early choices shape later success.

---

## 🧠 5. Strategy Used in RAGEN (Summary)

- Uses **multi-turn trajectory rollouts** — the LLM interacts over multiple steps.
- Aggregates **rewards from environment + LLM evaluators**.
- Uses **policy gradient (PPO or GRPO)** to update the policy based on full-episode returns.
- Supports **dual reward supervision**: external environment feedback and internal LLM-based scoring.
- Focuses on **credit assignment across turns** rather than per-response optimization.

---

## ⚙️ 6. Pseudocode: Multi-Turn RL Algorithm (from RAGEN.pdf)

```python
Algorithm 1: Multi-turn Reinforcement Learning in RAGEN

Initialize policy model πθ, reward model Rφ, and environment E

for each iteration do
    for each episode do
        Initialize context s0
        for t = 1 to T do
            # Generate the next action or message
            at ~ πθ(at | st)

            # Interact with the environment
            st+1, rt_env = E(st, at)

            # Optional: get LLM-based feedback
            rt_llm = Rφ(st, at, st+1)

            # Store transition
            D ← D ∪ {(st, at, rt_env + rt_llm, st+1)}
        end for

        # Compute total episode reward
        Rt = Σt(rt_env + rt_llm)
    end for

    # Update policy parameters using PPO or policy gradient
    θ ← θ + α ∇θ Eπθ[Σt γ^t Rt]

    # Optionally update reward model Rφ using new trajectories
end for
```

---

## 🎯 7. Key Components

| Symbol | Description |
|---------|-------------|
| **πθ** | Policy model (LLM agent) |
| **Rφ** | Reward model (environment-based or LLM-evaluator) |
| **E** | Environment or simulated user |
| **rt_env** | Environment reward (task success, correctness) |
| **rt_llm** | LLM-based reward (quality, reasoning consistency) |
| **D** | Replay buffer of trajectories |
| **θ update** | PPO or policy gradient optimization |

---

## ✅ 8. Summary

Multi-turn RL trains LLMs to think and act **over multiple decisions**, optimizing **long-horizon rewards** that reflect success across entire sequences rather than one-step predictions.  
This approach bridges **reinforcement learning**, **agentic behavior**, and **LLM reasoning** into one unified training paradigm.

---

**File generated from conversation and analysis of RAGEN.pdf.**


Talk RLHF:


![alt text](<talk_RLHF/Screenshot 2025-10-27 at 10.03.41 PM.png>) 

## Multi agent RL on LLM

Source: https://www.youtube.com/watch?v=uCkjoGIrVRM

# Summary/Notes 

1. ![alt text](<talk_RLHF/Screenshot 2025-10-27 at 10.36.15 PM.png>) 
Key idea is to have a defender and a aggresor LLM. Both try to protect each's interest. Aggressor trying to breach into while defender tries to hold/control the LLM. 
The defender should try to stop/block attacks which are more harsh/detrimental while allow benign ones. 

2. ![alt text](<talk_RLHF/Screenshot 2025-10-27 at 10.36.27 PM.png>) 

Reward model is custom and provided in: 
The goal? or one of the objective is to attain a Nash Equilibrium- where neither party is better of changing their position.

3. ![alt text](<talk_RLHF/Screenshot 2025-10-27 at 10.37.09 PM.png>) 

To optimise training and reduce the GPU resources, several of the models are reused such as the Policy model and the reward model. Still >4xA100 are needed for training.

4. ![alt text](<talk_RLHF/Screenshot 2025-10-27 at 10.37.41 PM.png>) 

Base reward system. Ranges between +1 , 0, -1. 

5. ![alt text](<talk_RLHF/Screenshot 2025-10-27 at 10.37.58 PM.png>) 

Examples of the types of adversial attacks and benign attacks made.
 
6. ![alt text](<talk_RLHF/Screenshot 2025-10-27 at 10.38.21 PM.png>) 

Shows well over +10% improvement in performance just by self play- same used in alphago and hide n seek games.

7. ![alt text](<talk_RLHF/Screenshot 2025-10-27 at 10.38.32 PM.png>) 

8. ![alt text](<talk_RLHF/Screenshot 2025-10-27 at 10.39.06 PM.png>) 

9. ![alt text](<talk_RLHF/Screenshot 2025-10-27 at 10.39.41 PM.png>) 
