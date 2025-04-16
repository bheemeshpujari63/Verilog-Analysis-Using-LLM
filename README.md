# Verilog-Analysis-Using-LLM
##  Overview

This project is a Python-based pipeline for **automated analysis, testbench generation, and simulation** of Verilog design files. Leveraging **LLM intelligence (Microsoft Phi-2)**, it combines natural language processing with hardware verification workflows to help engineers, researchers, and students validate digital logic designs faster and more efficiently.

---

##  Objectives

- ✅ **Automated Linting**: Detect syntax and semantic errors in Verilog files using `verilator`.
- ✅ **LLM-Based Code Analysis**: Use Microsoft's Phi-2 model for identifying issues and recommending design fixes.
- ✅ **Testbench Generation**: Automatically create testbenches based on extracted ports and module logic using the Phi-2 model.
- ✅ **Simulation & Verification**: Run simulations using `iverilog` + `vvp` to verify functional correctness.
- ✅ **Structured Reporting**: Compile results into a comprehensive `.xlsx` Excel file.

---

##  Tools and Technologies

| Category              | Tools/Frameworks |
|-----------------------|------------------|
| **Language**          | Python 3         |
| **LLM**               | Microsoft Phi-2  |
| **HDL Tools**         | Verilator, Icarus Verilog (iverilog) |
| **Python Libraries**  | `transformers`, `pandas`, `openpyxl`, `bitsandbytes`, `accelerate` |
| **Environment**       | Google Colab (T4 GPU) |

---

##  Methodology / Pipeline

### 1️⃣ **Setup & Initialization**
- Loads required packages.
- Mounts Google Drive and sets paths for Verilog files and output.

### 2️⃣ **File Discovery**
- Recursively scans directories for `.v` files.
- Separates regular Verilog files from `tb_*.v` testbenches.

### 3️⃣ **Pre-validation & Fixing**
- Uses regex to:
  - Validate module structure
  - Detect and fix nested modules
  - Ensure proper `endmodule` usage

### 4️⃣ **Parsing & Extraction**
- Extracts:
  - Module name
  - Port names
  - Port directions (`input`, `output`, `unknown`)
  - Optional heuristics for `clk`, `rst`, etc.

### 5️⃣ **Linting**
- Uses `verilator --lint-only` to:
  - Identify syntax and semantic issues
  - Catch missing ports, undeclared wires, etc.

### 6️⃣ **LLM-Powered Analysis**
- Sends full Verilog code + lint errors to **Phi-2**
- Prompted to:
  - Diagnose problems
  - Suggest fixes
- If LLM fails (token limit, syntax issues), falls back to rule-based analysis

### 7️⃣ **Testbench Generation**
- Uses Phi-2 to:
  - Generate testbench scaffolding
  - Create clock/reset logic
  - Add stimulus and monitoring
- Rule-based fallback if LLM fails or testbench is invalid

### 8️⃣ **Simulation**
- Compiles design + testbench using `iverilog`
- Simulates with `vvp`
- Captures errors, logs VCD output (optional)

### 9️⃣ **Benchmarking**
- Metrics include:
  - Number of lines
  - Number of ports
  - Expected runtime
  - Complexity indicators (operators, always blocks)

### 🔟 **Excel Reporting**
- Generates `verilog_pipeline_report.xlsx`
- Fields:
  - File Name
  - Module
  - Port List
  - Lint Status
  - LLM Analysis
  - Benchmark Data
  - Simulation Result
 
## Limitations
1.Phi-2 token limit (2048 tokens) affects large Verilog files  
2.Syntax hallucination possible in LLM-generated testbenches  
3.Nested module parsing is basic — only first module is processed  
4.No waveform viewer integration (VCDs are saved but not visualized)  

### ✅ 1. Setup Environment
```bash
!pip install transformers pandas openpyxl numpy bitsandbytes accelerate -q
!apt-get update -qq
!apt-get install -y verilator iverilog -qq
