# 🔐 DP Optimization within FL Scenarios

This repository contains the **implementation and final results** of the experiments conducted in my thesis. The project focuses on the **optimization of Differential Privacy (DP)** when defending against various attacks in a **Federated Learning (FL)** setting.

---

## ❗ Attacks Implemented

Four distinct adversarial attacks are simulated to evaluate the robustness and efficiency of the proposed DP optimization:

- **Label Flipping**  
  The attacker manipulates its labels to confuse the global model during aggregation.

- **Gaussian Poisoning**  
  The attacker sends malicious model updates sampled from a Gaussian distribution to degrade global performance.

- **Backdoor Insertion**  
  A specific trigger pattern is injected into the training data, causing the model to learn unintended behaviors when the pattern is present.

- **DMUG (Deep Models Under the GAN)**  
  A privacy attack where the adversary trains a GAN to reconstruct private class data based on gradients from the global model.

---

## 🧠 DP Optimization via Linear Programming

At the core of this project lies a **Linear Programming (LP)** formulation designed to **dynamically adapt the DP parameters** $( \epsilon, \delta )$ during training.

- When model loss is **low** (i.e., the model performs well), **stricter privacy** is enforced.  
- When model loss is **high**, privacy constraints are **relaxed** to allow better learning.  
- This trade-off is formalized as an LP problem and solved using the **Simplex method** (`scipy.optimize.linprog`).

This strategy is compared against a **static approach** where fixed DP parameters are used throughout training.

---

## 📊 Key Results

### ✅ Accuracy vs. Privacy Budget (DMUG)

<p align="center">
  <img src="experiments/DMUG/DMUG_Static_DP.png" alt="Static DP" width="400"/>
  <img src="experiments/DMUG/DMUG_LP_DP.png" alt="LP Optimized DP" width="400"/><br>
  <em>Static DP (left) vs. LP-Optimized DP (right) — accuracy vs. privacy trade-off in DMUG scenario.</em>
</p>

### 🧩 DMUG Reconstructions (100 rounds)

<p align="center">
  <img src="experiments/DMUG/DP_static/recovered_static.png" alt="Static DP Recovered" width="200"/>
  <img src="experiments/DMUG/DP_optimized/recovered_optimized.png" alt="Optimized DP Recovered" width="200"/><br>
  <em>Left: static DP — clearer reconstruction. Right: LP-optimized DP — GAN struggles to recover the input.</em>
</p>

These results show that **privacy can be enforced more effectively** without sacrificing model performance by adapting the DP parameters throughout training. You can check the rest of the results in `figures_TFG.ipynb`.

---

## 💻 Tools & Libraries

- [`FLEXible-FL`](https://github.com/FLEXible-FL) & [`Flex-Clash`](https://github.com/FLEXible-FL/flex-clash) - FL real scenarios and simulations.
- [`SciPy Linprog`](https://docs.scipy.org/doc/scipy/reference/generated/scipy.optimize.linprog.html) - Linear Programming Tools.
- [`Meta AI Opacus`](https://opacus.ai/) - For client-level DP and other DP-related tools.
- [`PyTorch`](https://pytorch.org/) - High traceability & personalization learning with NN's.

---

## 📁 Project Structure

```plaintext
.
├── experiments/
│   ├── LabelFlipping/ (also for Gaussian and Backdoor)
│   │   ├── MNIST/
│   │   │   ├── DP_Static/
│   │   │   │   └── n_poisoned_experiments/
│   │   │   └── DP_Optimized/
│   │   │       └── n_poisoned_experiments/
│   │   ├── LabelFlipping_DP_LP.ipynb
│   │   ├── LabelFlipping_DP_Static.ipynb
│   │   ├── FashionMNIST/
│   │   └── CIFAR-10/
│   └── DMUG/
│       ├── DP_Static/
│       └── DP_Optimized/
└── figures_TFG.ipynb
