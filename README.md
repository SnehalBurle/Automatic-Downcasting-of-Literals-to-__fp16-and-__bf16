# Automatic Down casting of Constants to __fp16 / __bf16 in Clang Frontend
This Clang plugin performs automatic downcasting of floating-point literals to low-precision formats like __fp16 or __bf16 when safe. 
It also generates a JSON report for visualization of the downcasting decisions and their effects, optionally integrates with a web-based heatmap UI for analysis.

## Features
AST-level analysis of floating-point constants.
Automatic downcasting to __fp16 or __bf16 when safe based on error thresholds.
Modifies source code (modified.cpp) with downcasted values.
Generates JSON output (float_map.json) for further visualization.
Web UI heatmap viewer for visualizing performance and errors.

## Use Cases
Performance and memory optimization in embedded systems (e.g., ML on edge devices).
Teaching and debugging floating-point precision trade-offs.
Visualization of downcast safety in large codebases.

## Requirements
LLVM/Clang (tested with LLVM 18)
Python 3.x for runner and script tools
C++17 compatible compiler
Web browser for viewing index.html

## Building the Plugin
bash
Copy
Edit
clang++ -shared -fPIC -o FloatDowncastChecker.so -std=c++17   -I$(llvm-config --includedir) -fno-rtti FloatDowncastChecker.cpp   $(llvm-config --cxxflags --ldflags --libs --system-libs)

If using LLVM 18, make sure llvm-config is in your PATH.

## Running the Tool
Prepare your source:
Your input source file should be named test.cpp.
Run with Clang plugin:

bash
Copy
Edit
For fp16 : clang++ -fsyntax-only   -Xclang -load -Xclang ./FloatDowncastChecker.so   -Xclang -plugin -Xclang float-downcast   -Xclang -plugin-arg-float-downcast -Xclang -threshold=0.001   -Xclang -plugin-arg-float-downcast -Xclang -mode=fp16   test.cpp
For bf16 : clang++ -fsyntax-only   -Xclang -load -Xclang ./FloatDowncastChecker.so   -Xclang -plugin -Xclang float-downcast   -Xclang -plugin-arg-float-downcast -Xclang -threshold=0.001   -Xclang -plugin-arg-float-downcast -Xclang -mode=bf16   test.cpp


## Run the script to analyze and downcast:
bash
Copy
Edit
python3 benchmark.py

This script:
Compiles both original and modified source files
Times execution
Saves performance metrics and float conversion metadata in run_test.json
