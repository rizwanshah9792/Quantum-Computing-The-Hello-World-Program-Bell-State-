# Quantum Computing "Hello World" with Qiskit ⚛️

Welcome to my first Quantum Computing project! As a Computer Science student stepping into the quantum world, this project serves as a basic introduction to **IBM's Qiskit** framework. 

This repository contains a Python notebook/script that creates a **Bell State** — the "Hello World" of quantum mechanics. It demonstrates two fundamental quantum concepts: **Superposition** and **Entanglement**.

## 🛠️ Prerequisites & Installation

Before running the code, you need to install the Qiskit library and a few tools for visualization. Open your terminal or Jupyter Notebook and run:

```bash
pip install qiskit qiskit-aer matplotlib
pip install pylatexenc
```

## 🧠 The Code Flow (Step-by-Step)

A typical quantum program follows a specific lifecycle. Here is the flow of our Hello World code:

### 1. Initialization
First, we initialize a **Quantum Circuit**. We define a circuit with 2 Qubits (quantum bits) and 2 Classical bits. The classical bits are necessary to store the final 0s and 1s we get after measuring the qubits.
* **Code equivalent:** `qc = QuantumCircuit(2, 2)`

### 2. Creating Superposition (The Hadamard Gate)
By default, qubits start in a state of `0`. We apply a **Hadamard gate (`H`)** to the first qubit. This puts the qubit into a state of *superposition*, meaning it now has an equal probability of being measured as a `0` or a `1`.
* **Code equivalent:** `qc.h(0)`

### 3. Creating Entanglement (The CNOT Gate)
Next, we apply a **Controlled-NOT gate (`CX` or `CNOT`)**. This gate acts as an IF statement: *IF the first qubit is 1, flip the second qubit*. 
Because the first qubit is in a superposition, this operation successfully **entangles** the two qubits. Their fates are now tied together. If one is measured as `0`, the other will be `0`. If one is `1`, the other will be `1`.
* **Code equivalent:** `qc.cx(0, 1)`

### 4. Measurement
Quantum states are fragile. To see the result, we must "measure" the qubits. This collapses their quantum state into standard classical bits (`0` or `1`), which we store in the 2 classical bits we created in step 1.
* **Code equivalent:** `qc.measure([0, 1], [0, 1])`

### 5. Execution and Visualization
Finally, we run the circuit on a local quantum simulator (`AerSimulator`). Because quantum mechanics deals with probabilities, we run the circuit multiple times (e.g., 1000 "shots") and plot the results on a histogram. We should see the results split roughly 50/50 between `00` and `11`.
* **Code equivalent:** `simulator.run(qc)` and `plot_histogram()`

---


## 🚀 How to Run

1. Open the Jupyter Notebook (`.ipynb` file) or Python script.
2. Run the cells sequentially.
3. You will see an ASCII drawing of your quantum circuit followed by a pop-up histogram showing the probability distribution of the `00` and `11` states!




# Why Do Quantum Circuits Often Output More 0s than 1s?

When you run a perfectly entangled quantum circuit (like a 2-qubit Bell State or a 3-qubit GHZ State), the math says you should get exactly a **50/50 split** between all `0`s and all `1`s. 

However, when you actually run the code, you might see results like `000` getting 1022 counts and `111` getting 978 counts. Why does `0` seem to win? 

The answer depends on whether you are running a **Simulator** or a **Real Quantum Computer**.

---

## Reason 1: The Math (When using a Simulator)
If you are using `AerSimulator`, you are using a mathematically "perfect" fake quantum computer. Any imbalance you see here is purely due to **Statistical Variance**.

* **The Coin Flip Analogy:** If you flip a perfectly fair coin 1,000 times, the chance of getting exactly 500 Heads and 500 Tails is incredibly low. You are much more likely to get something like 532 Heads and 468 Tails.
* **True Randomness:** The simulator relies on true mathematical randomness. If `00` beats `11` three times in a row, it is just a random coincidence. 

**Try this test:** Re-run your simulator cell 5 or 6 times in a row. Eventually, you will see the `11...1` side win! The simulator has no actual preference.

---

## Reason 2: The Physics (When using Real Quantum Hardware)
If you run this exact same code on a *real* IBM quantum computer, the `00...0` state will almost *always* get higher counts than the `11...1` state. This is not a coincidence; it is actual physics.

* **The "Lazy Universe" Rule:** In quantum physics, the $|0\rangle$ state is the "ground state" (lowest energy). The $|1\rangle$ state is the "excited state" (highest energy).
* **The Heavy Ball Analogy:** Think of the $|0\rangle$ state like a ball resting comfortably on the floor. Think of the $|1\rangle$ state like holding that heavy ball up in the air. 
* **Energy Decay (T1 Relaxation):** It takes energy to keep a qubit in the $|1\rangle$ state. Over the tiny fraction of a second it takes to run the circuit, the qubit naturally wants to release its energy and "fall down" to the comfortable $|0\rangle$ state.
* **Measurement Errors:** Reading quantum states is a messy process. Real quantum sensors are slightly more likely to make a mistake by accidentally reading a `1` as a `0` than the other way around.

---

## Summary
* **In a Simulator:** It is just the random luck of a coin flip. 
* **On Real Hardware:** The universe is lazy. Qubits constantly try to lose their energy and fall back down to `0`, causing a natural bias toward `00...0`.
