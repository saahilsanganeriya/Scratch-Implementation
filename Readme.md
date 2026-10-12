# About this fork

This is a copy of [METRA-DRL-Project/Scratch-Implementation](https://github.com/METRA-DRL-Project/Scratch-Implementation),
a six-person team project for a Georgia Tech Deep Reinforcement Learning course in spring 2025. The team rewrote
[METRA](https://arxiv.org/abs/2310.08887) (Park et al.), a method that learns distinct skills with no reward, from
scratch and adapted it to discrete-action Atari (Ms. Pac-Man) by replacing its policy learner with
[SAC-Discrete](https://arxiv.org/abs/1910.07207). The skills collapsed into the same behaviour. The team traced this
to a collapsed state representation and fixed the representation with a ResNet-18 encoder
(`PretrainedResNetEncoder` in `main.py`).

**Who built what.** Saahil Sanganeriya was one of the six authors of the team's report, "METRA for Discrete Action
Spaces". He worked on the SAC-Discrete adaptation and on diagnosing the representation collapse. Every commit in
this repository is by teammates Vidushi Maheshwari and Macrina Lobo; this note is the fork's only change.

| Encoder | Mean ‖Δφ‖ between consecutive states | Silhouette score by skill | Source |
|---|---|---|---|
| Original convolutional encoder (epochs 0–90) | 2.9e-8 to 6.4e-8 | about -0.50 | `runs/SAC-Discrete-20_114556` |
| ResNet-18 encoder (epochs 0–550) | 1.1e-3 to 1.0e-2 | -0.10 to 0.05 | `runs/SAC-Discrete-20_165643` |

Δφ is the change in the learned representation from one state to the next; near zero means every state looks the
same to the agent. The ResNet-18 encoder raised it by about five orders of magnitude, but the silhouette scores show
that the skills still did not form separate clusters. The report was a course submission and is not published
online. The team's README follows, unchanged.

---

# Setup
To run the training + evaluation loop of METRA + SAC-Discrete

1. Make sure you have model/ and runs/ directory where the logs will be stored
2. Run
```
conda create -n <env_name>
conda activate <env_name>
pip install -r requirements.txt
python main.py
```

You can run METRA + SAC on Ant Environment by running `python ant.py` (and the same setup steps as above)


# Results
You can look at the tensorboard logs that support the argument in report. Specifically,
1. `runs/SAC-20_155305`: METRA + SAC implementation on ant environment
2. `runs/SAC-Discrete-18_183958`, `runs/SAC-Discrete-18_190806/`, `runs/SAC-Discrete-18_191832`: represent the sudden policy/entropy drop that cause unstable loss functions, prompting the adoption of a decaying temperature schedule
3. `runs/SAC-Discrete-20_114556`: Mean $\Delta \phi$ in original architecture
4. `runs/SAC-Discrete-20_165643`: Mean $\Delta \phi$ in ResNet backboned architecture

drl.ipynb contains code for additional experiments with the entropy loss, experiments with a different rollout policy, and experiments with the complexity of the skill dimensions and distribution. Associated runs + videos + tensorboard snapshots can be found at https://drive.google.com/drive/folders/1YT7w6mKUSjMqVTBrj8OtQj_f8QOPlGw5?usp=share_link 
