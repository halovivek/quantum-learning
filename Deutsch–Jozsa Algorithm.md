# ⚛️ Deutsch–Jozsa Algorithm: The "Big Brother"

> **Scaling up: From 1 bit to N bits.**

The **Deutsch–Jozsa Algorithm** is a generalization of Deutsch's Algorithm. While Deutsch's algorithm handles a single input bit, Deutsch–Jozsa handles **multiple bits**, demonstrating an exponential speedup of quantum computers over classical ones for specific problems.

---

## 📈 What Changed?

In the original Deutsch algorithm, we dealt with a simple coin toss (0 or 1). Now, the input space has expanded.

* **Old Input:** 1 Bit ($0$ or $1$)
* **New Input:** $N$ Bits (Strings like `000`, `101`, `11110`...)

### The Problem
We have a Black Box function (Oracle) that takes this $N$-bit string. We know the function is guaranteed to be one of two types:

1.  **CONSTANT:** Returns the same output ($0$ or $1$) for **every possible input string**.
2.  **BALANCED:** Returns $0$ for exactly half of the inputs, and $1$ for the other half.

**Goal:** Determine which type it is with 100% certainty.

---

## 🐢 vs 🐇: The Speed Comparison

As the number of bits ($N$) grows, the difference between classical and quantum becomes massive.

| Feature | 💻 Classical Computer | ⚛️ Quantum Computer |
| :--- | :--- | :--- |
| **Strategy** | Brute Force (Check inputs one by one) | Parallel Computation |
| **Worst Case** | Must check **more than half** of all possible combinations ($2^{n-1} + 1$). | Uses **Superposition** to check all inputs at once. |
| **Complexity** | Exponential Time (Very Slow for large $N$) | Constant Time (Instant) |
| **Total Checks** | **Thousands/Millions** (depending on $N$) | **Exactly 1 Query** |

### Why is Classical so slow here?
If you have a 10-bit input, there are 1,024 possibilities.
* To be 100% sure the function isn't "Balanced," a classical computer might have to check **513** inputs.
* If the first 512 inputs all returned `0`, you still don't know if the next one might be `1`. You have to keep checking.

---

## 🧠 How the Quantum Solution Works

The Deutsch–Jozsa algorithm solves this in **one single run** of the circuit, regardless of how many bits the input has.

1.  **Massive Superposition:** It places the input register into a superposition of **every possible state** (from `00...0` to `11...1`) simultaneously.
2.  **The Oracle:** The function processes all these states at the same time.
3.  **Interference:**
    * If the function is **Constant**, the quantum amplitudes interfere constructively.
    * If the function is **Balanced**, the amplitudes interfere destructively (canceling each other out).
4.  **Measurement:** We measure the output. The result tells us immediately if it was Constant or Balanced.

---

## 🔑 Key Lesson

> **"Quantum computers can detect global patterns (properties of the whole function) very efficiently."**

Classical computers look at data points individually. Quantum computers can look at the **structure** of the data as a whole.

---

### 📚 Resources
* [Qiskit Textbook: Deutsch-Jozsa](https://qiskit.org/textbook/ch-algorithms/deutsch-jozsa.html)
* [Wikipedia: Deutsch–Jozsa Algorithm](https://en.wikipedia.org/wiki/Deutsch%E2%80%93Jozsa_algorithm)
