# ⚛️ Part 2: Deutsch–Jozsa Algorithm (Scaling Up)

> **"Scaling the magic from 1 bit to N bits. Proving quantum computers can detect global patterns instantly."**



The **Deutsch–Jozsa Algorithm** is the "Big Brother" of Deutsch's algorithm. It generalizes the problem from a single bit to $N$ bits, demonstrating that quantum computers can handle exponentially larger input spaces with a single query, while classical computers struggle as the input grows.

---

## 🖼️ 1. Picture-Based Intuition (The Big Idea)

### Simple Story (No Math)
You are given a **Mystery Function** (Oracle) that takes a long string of bits (e.g., `01101...101`) as input and outputs a single bit (`0` or `1`).

You are promised that the function is one of two types:
1.  🔹 **CONSTANT:** Returns the **same output** for every single possible input.
2.  🔹 **BALANCED:** Returns `0` for exactly half the inputs and `1` for the other half.

👉 **Your Task:** Determine which type it is.

### 🐢 vs 🐇: The Scalability Crisis

As the number of bits ($n$) grows, the difference becomes massive.

| Feature | 💻 Classical Computer | ⚛️ Quantum Computer |
| :--- | :--- | :--- |
| **Strategy** | Trial and Error | Parallel Phase Interference |
| **Worst Case** | Must check **more than half** of all $2^n$ inputs. | Checks **ALL** $2^n$ inputs at once. |
| **Speed** | 🐢 **Exponentially Slow** (as $n$ grows) | 🚀 **Constant Time** (Instant) |
| **Total Checks** | Thousands or Millions | **✨ Exactly 1 Run ✨** |

---

## 🔌 2. Circuit Walkthrough (Step-by-Step)

How do we check millions of inputs in one go?



**The Setup:**
* **Input Qubits ($n$):** All initialized to $|0\rangle$.
* **Helper Qubit (Ancilla):** Initialized to $|1\rangle$.

### 🟢 Step 1: Initialize
* **Input:** Clean slate ($|00...0\rangle$).
* **Helper:** Set to $|1\rangle$ to enable the "phase kickback" trick.

### 🟢 Step 2: Apply Hadamard to ALL Qubits
We apply the H-gate to every qubit.
* **Effect:** This creates a **massive superposition**. The input qubits now represent **every possible bitstring** ($000...$ to $111...$) simultaneously.
* *Think:* "We are preparing to ask the function every possible question at once."

### 🟢 Step 3: Oracle (The Black Box)
The oracle processes the superposition state: $|x, y\rangle \to |x, y \oplus f(x)\rangle$.
* **The Magic:** The result of the function ($0$ or $1$) isn't written to the bit value; it is encoded into the **phase** of the quantum state.
    * **Constant:** All phases align.
    * **Balanced:** Phases flip signs relative to each other, creating a specific pattern.

### 🟢 Step 4: Apply Hadamard to INPUT Qubits
This step creates **Interference**. It acts like a lens, focusing the "correct" answer.
* **Constant Function:** The waves reinforce each other constructively at the state $|00...0\rangle$.
* **Balanced Function:** The waves cancel out (destructive interference) at $|00...0\rangle$.

### 🟢 Step 5: Measure Input Qubits
We look at the final state of the input register.

| Measurement Result | Conclusion |
| :--- | :--- |
| **All Zeros** (`000...0`) | **CONSTANT** |
| **Any other string** (e.g., `010...1`) | **BALANCED** |

---

## 🧪 3. Python Implementation (Qiskit)

Here is a clean implementation for 3 input qubits ($n=3$).

```python
from qiskit import QuantumCircuit, Aer, execute
from qiskit.visualization import plot_histogram

# Setup
n = 3  # number of input qubits
qc = QuantumCircuit(n + 1, n)

# --- Step 1: Initialize Helper ---
qc.x(n)  # Set helper to |1>

# --- Step 2: Hadamard on ALL qubits ---
for i in range(n + 1):
    qc.h(i)

# --- Step 3: ORACLE (The Black Box) ---
qc.barrier()
# Example: A BALANCED oracle (CNOTs act as f(x)=x, which is balanced)
for i in range(n):
    qc.cx(i, n)
qc.barrier()

# --- Step 4: Hadamard on INPUT qubits ---
for i in range(n):
    qc.h(i)

# --- Step 5: Measure INPUT qubits ---
qc.barrier()
for i in range(n):
    qc.measure(i, i)

# --- Run ---
print(f"Running Deutsch-Jozsa with {n} qubits...")
backend = Aer.get_backend('qasm_simulator')
result = execute(qc, backend, shots=1024).result()
counts = result.get_counts()

print("\nResults:", counts)
# Interpretation:
# If result is '000': CONSTANT
# If result is anything else (e.g., '111'): BALANCED
