# MC2 - Monte Carlo Fuzzing Implementation

## Prerequisites

Make sure you have the following dependencies installed:

- **LLVM 11** (`llvm-11-dev clang-11`)
- **cmake** (version 3.1 or higher)
- **libcurlpp-dev**

On Ubuntu/Debian systems:
```bash
sudo apt-get install llvm-11-dev clang-11 cmake libcurlpp-dev
```

## Build Instructions

1. Navigate to the mc2 directory:
   ```bash
   cd mc2
   ```

2. Build the project:
   ```bash
   make build
   ```

   This will:
   - Create a build directory and run cmake
   - Compile the LLVM pass in the `skeleton/` directory  
   - Compile `runtime_func.c` and `onebyte_test.c` with the LLVM pass

3. Clean previous builds if needed:
   ```bash
   make clean
   ```

## Execution Modes

The project provides three different execution modes to demonstrate Monte Carlo-based fuzzing:

### 1. Basic Execution (No Monte Carlo)
```bash
make noMonteCarloExecution
```

**What it does:** Runs the test program with standard execution flow.

**Example output:**
```
3 is input
Took false direction: -12 is the result
1.000000 ratio
```

### 2. Monte Carlo Execution
```bash
make monteCarloExecution
```

**What it does:** Uses Monte Carlo methods with the policy file (`onebyte.policy`) to guide execution toward different paths.

**Example output:**
```
3 is input
Took true direction: 3 is the result
0.000000 ratio
```

### 3. Noisy Binary Search
```bash
make noisyBinarySearch
```

**What it does:** Performs automated fuzzing with multiple input values to systematically explore different code paths and identify promising input regions.

**Example output:**
```
57 is input
Took true direction: 57 is the result
[... multiple test runs with different values ...]
--- Most Promising Input Region ----
0: [0, 1]
```

## Understanding the Test Program

The `onebyte_test.c` program demonstrates a simple conditional branch:

- **Input:** Reads one byte from `onebyte.txt` (default value: `3`)
- **Logic:** 
  - If byte value < 3: takes "true" path (addition: `result = byte + 0`)
  - If byte value ≥ 3: takes "false" path (subtraction: `result = byte - 15`)

## Project Features

This implementation demonstrates:

1. **LLVM-based Code Instrumentation:** Uses LLVM passes to track branch coverage and execution paths
2. **Monte Carlo Guided Execution:** Employs Monte Carlo methods to manipulate program execution toward interesting paths
3. **Automated Input Generation:** Performs noisy binary search to efficiently discover input ranges that trigger specific behaviors
4. **Policy-based Fuzzing:** Uses configurable policies (`onebyte.policy`) to guide the fuzzing process

## Files Overview

- `onebyte_test.c` - Simple test program with conditional branching
- `onebyte.txt` - Input file containing test data (1 byte)
- `onebyte.policy` - Monte Carlo policy configuration
- `runtime_func.c` - Runtime functions for instrumentation
- `skeleton/` - LLVM pass implementation
- `Makefile` - Build and execution targets

## Troubleshooting

If you encounter build errors:

1. **LLVM version mismatch:** Ensure you have exactly LLVM 11.1.0 installed
2. **Missing headers:** Verify `llvm-11-dev` package is properly installed
3. **CMake errors:** Make sure cmake version is 3.1 or higher

## Quick Start

```bash
# Install dependencies (Ubuntu/Debian)
sudo apt-get install llvm-11-dev clang-11 cmake libcurlpp-dev

# Build the project
cd mc2
make build

# Run basic execution
make noMonteCarloExecution

# Try Monte Carlo execution
make monteCarloExecution

# Explore with noisy binary search
make noisyBinarySearch
```

