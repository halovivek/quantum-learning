# ⚛️ Deutsch’s Algorithm: The Quantum Foundation

> **"Think of Deutsch’s Algorithm as the 'Letter A' of the quantum alphabet. You must learn this before reading sentences."**

Welcome to the starting point of quantum algorithms. This is the simplest proof that a quantum computer can solve a specific problem faster than a classical computer by utilizing fundamental quantum properties like superposition and interference.

---

## 🖼️ 1. Picture Intuition (Think Like a Human)

### The Story
Imagine you have a **mystery machine** (a "black box").
* You can put in a **0** or a **1**.
* It gives back a **0** or a **1**.

You are told the machine is one of two types:
1.  🔹 **CONSTANT:** Always gives the same output, no matter what you put in. (e.g., Input 0→ Output 0; Input 1→ Output 0).
2.  🔹 **BALANCED:** The output changes depending on the input. (e.g., Input 0→ Output 0; Input 1→ Output 1).

👉 **Your Task:** Identify which type of machine it is.

### Classical vs. Quantum Thinking

Here is how an old-school computer compares to a quantum computer for this task:

| Approach | How it works | Checks Needed |
| :--- | :--- | :--- |
| **Classical Way** (Old) | You must check input `0` AND input `1` separately to compare the results. | **2 Checks** |
| **Quantum Way** (New) | Tries `0` and `1` at the same time using superposition and combines results using interference. | **✨ 1 Check ✨** |

<div align="center">
  <img src="images/intuition_diagram.png" alt="Classical vs Quantum Intuition Diagram" width="600">
  <p><em>Visualizing the speedup.</em></p>
</div>

---

## 🔌 2. Circuit Walkthrough (Step-by-Step)

How does the magic happen? It's a careful arrangement of quantum gates.

**The Setup:**
* **Qubit 0 (Top line):** The Input qubit.
* **Qubit 1 (Bottom line):** The Helper qubit (ancilla).

<div align="center">
  <img src="images/full_circuit_diagram.png" alt="Full Deutsch Algorithm Circuit" width="700">
</div>

### Step 1: Initialize
We start the input qubit at `|0⟩` and flip the helper qubit to `|1⟩`.
* *Why is the helper `|1⟩`?* This is crucial for creating the necessary phase interference later in the circuit.

### Step 2: Apply Hadamard (H) Gates
We apply H-gates to both qubits to put them into **superposition**.
* Now, the input qubit is effectively `0` and `1` at the same time.

### Step 3: Apply the Oracle (The Black Box)
This is the mysterious function $f(x)$. The oracle interacts with the qubits in superposition.
* If the function is **Constant**, the quantum states align.
* If the function is **Balanced**, the states flip signs relative to each other. This is where the information is hidden.

### Step 4: Apply Hadamard Again (Input Qubit Only)
This final H-gate causes **Interference**. It takes the hidden sign information from Step 3 and converts it back into a readable result.
* **Constant** functions cause constructive interference (waves add up to 0).
* **Balanced** functions cause destructive interference (waves cancel out to 1).

### Step 5: Measure
We measure the input qubit.

| Measurement Result | Meaning |
| :--- | :--- |
| **0** | CONSTANT Function |
| **1** | BALANCED Function |

🎯 **Result obtained in exactly one measurement.**

---

## 🧪 3. Python Implementation (Qiskit)

Here is a minimal, beginner-friendly implementation using Qiskit.

```python
from qiskit import QuantumCircuit, Aer, execute
from qiskit.visualization import plot_histogram

# Create circuit with 2 qubits and 1 classical bit for measurement
qc = QuantumCircuit(2, 1)

# --- Step 1: Initialize ---
qc.x(1)              # Set helper qubit to |1>

# --- Step 2: Hadamard ---
qc.barrier()         # Visual separation
qc.h(0)
qc.h(1)

# --- Step 3: ORACLE (The Black Box) ---
qc.barrier()
# Example: We are using a BALANCED function here (CNOT behaves as f(x)=x)
# If you want a CONSTANT function, comment out the next line.
qc.cx(0, 1)

# --- Step 4: Hadamard again (Input only) ---
qc.barrier()
qc.h(0)

# --- Step 5: Measure input qubit ---
qc.barrier()
qc.measure(0, 0)

# --- Run the circuit ---
print("Running Simulator...")
backend = Aer.get_backend('qasm_simulator')
result = execute(qc, backend, shots=1024).result()
counts = result.get_counts()

print("\nResults:", counts)
# Interpretation:
# If results are mostly '0': The oracle was CONSTANT
# If results are mostly '1': The oracle was BALANCED (like in this example code)
