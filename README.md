# ⚡ Options Pricing Engine — C++17

![Language](https://img.shields.io/badge/language-C%2B%2B17-blue?style=flat-square)
![Build](https://img.shields.io/badge/build-passing-brightgreen?style=flat-square)
![License](https://img.shields.io/badge/license-MIT-lightgrey?style=flat-square)
![Domain](https://img.shields.io/badge/domain-Quantitative%20Finance-orange?style=flat-square)

> A high-performance C++17 engine for pricing European and American options using multiple numerical methods. Built as part of my exploration of quantitative finance and performance-critical systems programming.

---

## 🎯 Overview

This project implements four classic option pricing models from scratch in C++, focusing on **numerical accuracy**, **execution speed**, and **clean architecture**. It was developed to deepen my understanding of both financial mathematics and low-level performance engineering.

```
 Input Parameters          Pricing Models              Output
 ─────────────────         ─────────────────           ──────────────
  S  : Spot price    ───►  Black-Scholes (Closed)  ──► Call / Put Price
  K  : Strike        ───►  Binomial Tree            ──► Greeks (Δ, Γ, Θ, ν)
  T  : Maturity      ───►  Finite Difference (FDM)  ──► Convergence Report
  r  : Risk-free     ───►  Monte Carlo (Parallel)   ──► Std Error
  σ  : Volatility
```

---

## 🧠 Implemented Models

| Model | Type | Options | Notes |
|---|---|---|---|
| Black-Scholes | Analytical | European | Closed-form, O(1) |
| Binomial Tree (CRR) | Tree | European + American | Configurable steps |
| Finite Difference (FDM) | Grid | European + American | Crank-Nicolson scheme |
| Monte Carlo | Simulation | European + Path-dep. | Mersenne Twister RNG |

---

## 🚀 Getting Started

### Prerequisites
- C++17 compatible compiler (GCC ≥ 9, Clang ≥ 10, MSVC 2019+)
- CMake ≥ 3.14
- (Optional) OpenMP for parallelized Monte Carlo

### Build & Run

```bash
git clone https://github.com/elouafi-abderrahmane-2002/OptionsPricingCPP.git
cd OptionsPricingCPP
mkdir build && cd build
cmake .. -DCMAKE_BUILD_TYPE=Release
make -j4
./options_pricing
```

### Example Usage

```cpp
#include "BlackScholes.hpp"
#include "MonteCarlo.hpp"

// European Call — Black-Scholes
BlackScholes bs(100.0, 105.0, 1.0, 0.05, 0.2);
double call_price = bs.call_price();  // 8.022
double delta      = bs.delta();       // 0.537

// Monte Carlo with 1M simulations
MonteCarlo mc(100.0, 105.0, 1.0, 0.05, 0.2, 1'000'000);
auto [price, std_error] = mc.price_call();
```

---

## 📊 Performance Benchmarks

| Method | Simulations / Steps | Time | Precision |
|---|---|---|---|
| Black-Scholes | — | < 1 µs | Exact |
| Binomial Tree | 1 000 steps | ~2 ms | ±0.01 |
| Monte Carlo (1 thread) | 1 000 000 | ~420 ms | ±0.05 |
| Monte Carlo (OpenMP 4T) | 1 000 000 | ~115 ms | ±0.05 |

---

## 📐 Project Structure

```
OptionsPricingCPP/
├── include/
│   ├── BlackScholes.hpp
│   ├── BinomialTree.hpp
│   ├── FiniteDifference.hpp
│   └── MonteCarlo.hpp
├── src/
│   ├── BlackScholes.cpp
│   ├── BinomialTree.cpp
│   ├── FiniteDifference.cpp
│   ├── MonteCarlo.cpp
│   └── main.cpp
├── tests/
│   └── pricing_tests.cpp
└── CMakeLists.txt
```

---

## 📚 What I Learned

Working on this project gave me hands-on exposure to several concepts that I find genuinely difficult and rewarding:

- **Stochastic calculus fundamentals** — Itô's lemma, Geometric Brownian Motion, and how they lead to the Black-Scholes PDE
- **Numerical stability** — the Crank-Nicolson scheme for the FDM is unconditionally stable, unlike explicit Euler; understanding *why* this matters in practice was a key lesson
- **C++ performance patterns** — avoiding heap allocations in the inner loop, SIMD-friendly data layout, and why `std::mt19937` is the right default RNG for Monte Carlo
- **Greeks computation** — finite difference approximations vs. analytic formulas, and the trade-off between accuracy and speed

---

## 👤 Author

**Abderrahmane Elouafi**
Final-year Engineering Student — Big Data & Cloud Computing, ENSET Mohammedia
[LinkedIn](https://www.linkedin.com/in/abderrahmane-elouafi-43226736b/) · [Portfolio](https://my-first-porfolio-six.vercel.app/) · [GitHub](https://github.com/elouafi-abderrahmane-2002)

---

*Built with curiosity and a lot of numerical debugging.*
