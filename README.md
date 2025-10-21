# WebViewJSdetect

**JavaScript Vulnerability Detection in Android WebView via Coverage-Guided, Thread-Adaptive, Concurrent Abstract Interpretation**

> This repository provides the open-source implementation accompanying the paper **WebViewJSdetect**, a static analysis system for detecting JavaScript vulnerabilities embedded in Android WebView.

------

## 👩‍🔬 Paper

**Title:** WebViewJSdetect: JavaScript Vulnerability Detection in Android WebView via Coverage-Guided Thread-Adaptive Concurrent Abstract Interpretation
 **Authors:** Zhanhui Yuan∗, Zhi Yang∗, Jinglei Tan∗, Hongqi Zhang∗
 ∗ School of Cryptography Engineering, People’s Liberation Army Information Engineering University, Zhengzhou, China

**Abstract**
Android WebView enables applications to embed web content within native UI, wherein embedded JavaScript often requires elevated privileges (e.g., cross-origin requests, access to sensitive data). While this enhances functionality, it introduces risks such as unauthorized API access and privilege escalation. Existing static analysis struggles to model JavaScript’s dynamic features and runtime Java interface interactions, causing false negatives on deep, dynamically triggered vulnerabilities; lack of priority-guided analysis and adaptive scheduling further hurts efficiency.  WebViewJSdetect models JavaScript’s dynamic features via concurrent abstract interpretation and mitigates state explosion with a coverage-guided adaptive thread scheduling mechanism. 

------

## ✨ Highlights

- **Concurrent Abstract Interpretation**
  Models JavaScript branches, callbacks, and asynchronous events as concurrent threads and simulates runtime interactions between JavaScript and Java interfaces, enabling deep, cross-language information-flow tracking.
- **Coverage-Guided, Adaptive Thread Scheduling**
  Prioritizes insufficiently covered or high-risk code regions; dynamically adjusts priorities and time slices based on new-coverage ratio, branch depth, and wait time; merges redundant states to curb state explosion.

------

## 🧭 System Architecture

WebViewJSdetect consists of two primary components:

- **Thread Scheduler**
  Assigns execution priorities using signals such as branch depth and coverage feedback. It can preempt a running thread and later merge completed threads into their parents to maintain continuity and reduce redundancy.
- **Thread Executor**
  Performs abstract interpretation on WebView-embedded JavaScript, interacts with the modeled Android WebView API, propagates taint from attacker-controlled inputs, and checks if taint reaches sensitive sinks without sanitization.


------

## 📦 Repository Layout

```
WebViewJSdetect/
├─ docs/                    # Documentation, figures (e.g., system architecture)
├─ src/                     # Core analysis engine (scheduler, executor, abstract domain)
├─ scripts/                 # Helper scripts
├─ demos/ or datasets/      # Example datasets for quick testing
├─ single_run.sh            # Entry script (CLI similar to the CoCo project)
├─ install.sh               # One-click installer (Python/Node deps and setup)
├─ requirements.txt         # Python dependencies (authoritative)
├─ package.json             # Node.js dependencies, if applicable (authoritative)
├─ LICENSE                  # License file
└─ README.md                # This file
```

------

## 🧰 Requirements

- **Python**：3.8–3.11 recommended (3.7+ may work).
- **Node.js**：12+ (LTS recommended) — only if JS tooling is enabled.
- **Bash & Git**：Required for scripts and cloning.
- **Python dependencies**：Refer to  `requirements.txt` (static analysis/graph calculation/parsing, etc.)
- **Node dependencies**：If using JS parsing/building tools, please refer to  `package.json`

> The final version shall be based on the `requirements.txt` / `package.json` in the code repository.

------

## 🔗 Installation

### Option A — One-click (recommended)

```bash
# Clone repository
git clone https://github.com/Project-YZH/WebViewJSdetect.git
cd WebViewJSdetect

# Automatically install dependencies (Python/Node dependencies, necessary tools, etc.)
./install.sh
```

### Option B — Manual

```bash
# Python dependencies
python3 -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt

# Node dependencies (if applicable in your setup)
npm install
```

------

## ▶️ Quick Start

### Prepare input:

Collect the **JavaScript files used by Android WebView** (e.g., extracted from `assets/`, local packaged resources, or captured web resources) into a directory, say `./my_webview_js/`.

### **Run the analyzer**:

```bash
./single_run.sh [input_dir]
./single_run.sh demos/webview_sample
```

### **Inspect results**:

The tool emits logs and a detection report (path and format are printed by the script).

------

## 🧪 Demo

A small, ready-to-run sample is provided in demos:

```bash
./single_run.sh demos/exec_code
# or
./single_run.sh demos/test
```

------

## ⚙️ Usage & Options

> Options may evolve; run ./single_run.sh -h for the authoritative list.

- `--timeout`： Max analysis time per path/thread.
- `--max-threads`：Upper bound on concurrent analysis threads.
- `--priority-policy`：Scheduling policy (e.g., `coverage-first`, `risk-first`).
- `--report`：Output report directory and/or format (e.g., `json`, `txt`).
- `--sinks` / `--sources`：Custom source/sink specifications.
- `--log-level <level>` ：Verbosity (`info`, `debug`).

------

## 🧩 List of Dependencies

> The **authoritative lists** are `requirements.txt` (Python) and `package.json` (Node).

- **Python (core)**：抽象解释/图计算/数据结构（如 `networkx`、解析/静态分析相关库等）

  - Parsing/AST & static analysis utilities

  - Graph/CFG & data-flow analysis (e.g., a graph library)

  - CLI & logging, JSON/YAML handling

- **Node.js (if enabled):**

  - JavaScript parser/AST tooling (e.g., `acorn`, `esprima`, or equivalent)
  - Build utilities used by parser wrappers or preprocessing scripts

- **System tools:**：`bash`, `coreutils`, `git`

------

## 🔒 CVE Identifiers & Responsible Disclosure

**CVE Identifiers**
CVE identifiers are currently being requested from the relevant authorities and are temporarily unavailable. We apologize for the inconvenience. Once assigned, the CVE IDs will be added to this repository without delay.

**Vulnerability Details & Responsible Disclosure**
Our analysis confirmed 30 vulnerable applications. We strictly follow responsible disclosure principles and have reported all details to the corresponding development teams. For security reasons, we do **not** disclose package names directly in the paper. After obtaining explicit permission from all vendors, we will publish a list of vulnerable package names in this repository.

------

## 📚 Citation

If this project helps your research or engineering, please cite (Example of occupying space, to be completed with official publication information)：

```bibtex
@article{WebViewJSdetect,
  title   = {WebViewJSdetect: JavaScript Vulnerability Detection in Android WebView via Coverage-Guided Thread-Adaptive Concurrent Abstract Interpretation},
  author  = {Yuan, Zhanhui and Yang, Zhi and Tan, Jinglei and Zhang, Hongqi},
  note    = {Under Review}
}
```

------

## 🤝 Contributing

Contributions are welcome!
You can help with:

- Bug reports and compatibility fixes
- New source/sink specifications
- Scheduler and abstract-domain optimizations
- Dataset curation and benchmarking scripts

Please open an issue or a pull request with a clear description and minimal reproducer.

------

## 📄 License

See the `LICENSE` file.

------

## 📫 Contact

- Security-sensitive reports & confidential disclosures: please contact the maintainers privately (see the paper or repository profile for details). Thank you for practicing responsible disclosure.

------

> **Note**：This README will be continuously updated as the code and paper progress; If there are any discrepancies with the script/dependencies, please refer to the actual files in the repository.
