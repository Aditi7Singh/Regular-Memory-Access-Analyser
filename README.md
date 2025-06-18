🔍 Regular Memory Access Analyzer

A static analysis tool built with LLVM/Clang to detect regular vs. irregular memory access patterns in C/C++ code. Regular patterns (like sequential array access) can significantly improve performance due to better cache utilization, while irregular patterns often degrade efficiency.

📌 Problem Statement

The tool aims to analyze C/C++ functions and identify regular memory access patterns, such as:

-- Accessing array elements in sequential order using loop variables (e.g., data[i] in a for-loop over i).

-- Emit diagnostic messages when such patterns are detected.

-- This enables developers to spot performance-critical code sections and optimize for better memory locality.

🚀 Features

✅ Detects sequential memory accesses (e.g., data[i])

🚫 Flags irregular accesses (e.g., output[indices[i]])

📄 Emits diagnostic messages with line numbers and reasoning

🛠️ Works directly on the AST using Clang's tooling APIs

🎯 Custom command-line flag: --analyze-regular-memory-access

💻 Example

Regular Memory Access -
void compute(float* data, int N) {
    for (int i = 0; i < N; ++i) {
        data[i] = sin(data[i]) + cos(data[i]);
    }
}

Irregular Memory Access -
void scatter(float* data, float* output, int* indices, int N) {
    for (int i = 0; i < N; ++i) {
        output[indices[i]] = data[i];
    }
}

Output:
Analyzing function 'compute'...
- Sequential access at line 3
  Reason: Index is loop variable 'i'

Analyzing function 'scatter'...
- Irregular access at line 3
  Reason: Index is variable 'indices', not clearly a loop variable
- This function may have irregular memory access.

🧠 Analysis Logic

AST Matcher identifies user-defined functions (FunctionDecl) excluding system headers.

Each function body is recursively traversed to find ArraySubscriptExpr nodes.

The index of each subscript is classified:

✅ Sequential if it’s a loop variable (i)

❌ Irregular if it’s another variable or a complex expression

🔧 Build Instructions
🛠️ Requirements
LLVM & Clang (recommended: 12.0+)

CMake

A C++17-compatible compiler

📦 Build

git clone https://github.com/Aditi7Singh/regular-memory-access-analyzer.git
cd regular-memory-access-analyzer
mkdir build && cd build
cmake ..
make

▶️ Usage

./regular-memory-access-analyzer --analyze-regular-memory-access file.cpp
You can also run it as part of a compilation database:
./regular-memory-access-analyzer --analyze-regular-memory-access \
    -p build/ path/to/source.cpp

📂 File Structure

regular-memory-access-analyzer/
├── CMakeLists.txt
├── src/
│   └── Analyzer.cpp          # Contains core analysis logic
├── include/
│   └── Analyzer.h            # Callback definitions (optional)
└── README.md                 # You're here!

📈 Future Work
⚡ Integrate LLVM IR-level memory pattern detection

📊 Visualize memory access graphs

🧵 Track loop nests and strides

🧠 ML-based prediction for irregularity

👥 Contributor
Aditi Singh - https://github.com/Aditi7Singh

📜 License
This project is licensed under the MIT License.

