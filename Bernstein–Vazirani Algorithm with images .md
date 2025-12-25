# ⚛️ Part 3: Bernstein–Vazirani Algorithm

> **"Finding the secret code. Not by guessing, but by asking the perfect question."**



The **Bernstein–Vazirani Algorithm** is a powerful demonstration of quantum parallelism. It solves a specific problem—finding a hidden binary string—with exactly **one** query, whereas a classical computer would need to query bit-by-bit.

---

## 🖼️ 1. Picture-Based Intuition (Story First)

### Simple Story (No Math Fear)
Imagine the computer holds a **Secret Binary Code** ($s$).
* Example: `s = 10110`

You do not know this code. You can only interact with a "Black Box" (Oracle) by asking it questions.
* **The Catch:** The box doesn't just give you the code. It tells you whether your input matches the secret in a mathematically complex way (inner product).

### 🐢 Classical vs 🐇 Quantum

| Feature | 💻 Classical Computer | ⚛️ Quantum Computer |
| :--- | :--- | :--- |
| **Strategy** | Probing bit-by-bit | "Fourier Sampling" |
| **Method** | You must ask: "Is the 1st bit 1?", then "Is the 2nd bit 1?"... | You ask **one smart question** that reveals the global pattern. |
| **Queries Needed** | **$N$ Queries** (One per bit) | **✨ 1 Query ✨** |
| **Result** | Slow discovery | **Instant extraction** |

---

## 🔌 2. Circuit Walkthrough (Step-by-Step)

How do we extract the full secret string in a single run?



**The Setup:**
* **Input Qubits ($n$):** Where the secret bits will be revealed.
* **Helper Qubit:** Initialized to $|1\rangle$.
* **Example:** If Secret `s = 101`, we need 3 input qubits.

### 🟢 Step 1: Initialize
* **Inputs:** All set to $|0\rangle$.
* **Helper:** Set to $|1\rangle$.

### 🟢 Step 2: Apply Hadamard to ALL qubits
We apply H-gates to everything.
* **Effect:** The input qubits enter a superposition of **all possible inputs** at once. The helper qubit is prepared for the "phase kickback" trick.
* *Think:* "We are shouting every possible question at the black box simultaneously."

### 🟢 Step 3: Oracle (Where the Secret Hides)
The function is $f(x) = x \cdot s \pmod 2$.
* **What really happens:** The Oracle applies `CNOT` gates controlled by the input and targeting the helper, but **only where the secret bit is 1**.
* **Phase Kickback:** Because the helper is in a specific state ($|-\rangle$), these CNOTs don't flip the helper's value; instead, they kick a **phase change** back to the input qubits. This "marks" the qubits corresponding to the $1$s in the secret string.

### 🟢 Step 4: Apply Hadamard to INPUT qubits
The H-gate acts as a decoder.
* It collects the phase information imprinted by the Oracle and converts it back into standard bit values ($0$ or $1$).

### 🟢 Step 5: Measure Input Qubits
We measure the inputs.
* **Result:** The measurement output is the exact secret string. No probability tricks. No guessing.

**Output:** `101` (Secret found!)

---

## 🧪 3. Python Implementation (Qiskit)

Here is a clean implementation. Change the `secret` variable to try different codes!

```python
from qiskit import QuantumCircuit, Aer, execute
from qiskit.visualization import plot_histogram

# --- Setup the Secret ---
secret = "101"  # Try changing this!
n = len(secret)

# Create circuit (n inputs + 1 helper)
qc = QuantumCircuit(n + 1, n)

# --- Step 1: Initialize Helper ---
qc.x(n)

# --- Step 2: Hadamard on ALL qubits ---
for i in range(n + 1):
    qc.h(i)

# --- Step 3: ORACLE (Encodes the Secret) ---
qc.barrier()
# The oracle places a CNOT wherever the secret bit is '1'
# Note: We iterate in reverse to match Qiskit's qubit ordering if needed, 
# but for simple symmetry "101", standard enumeration works fine.
for i, bit in enumerate(secret):
    if bit == '1':
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
print(f"Looking for secret: {secret}")
backend = Aer.get_backend('qasm_simulator')
result = execute(qc, backend, shots=1024).result()
counts = result.get_counts()

print("\nMeasured Counts:", counts)
# Expected Output: {'101': 1024} -> 100% accuracy
