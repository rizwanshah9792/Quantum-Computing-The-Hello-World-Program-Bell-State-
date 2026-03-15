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

## 💻 The Code

Here is the complete Python code for this flow:

```python
from qiskit import QuantumCircuit
from qiskit_aer import AerSimulator
from qiskit.visualization import plot_histogram
import matplotlib.pyplot as plt

# 1. Initialize 2 Qubits and 2 Classical Bits
qc = QuantumCircuit(2, 2)

# 2. Apply Hadamard gate to Qubit 0 (Superposition)
qc.h(0)

# 3. Apply CNOT gate to Qubit 0 and 1 (Entanglement)
qc.cx(0, 1)

# 4. Measure the qubits
qc.measure([0, 1], [0, 1])

# Draw the circuit
print(qc.draw())

# 5. Execute on a local simulator
simulator = AerSimulator()
job = simulator.run(qc, shots=1000)
result = job.result()

# Get the counts (the results of the 1000 shots)
counts = result.get_counts(qc)
print("\nTotal counts:", counts)

# Visualize the results
plot_histogram(counts)
plt.show()
```

## 🚀 How to Run

1. Open the Jupyter Notebook (`.ipynb` file) or Python script.
2. Run the cells sequentially.
3. You will see an ASCII drawing of your quantum circuit followed by a pop-up histogram showing the probability distribution of the `00` and `11` states!
