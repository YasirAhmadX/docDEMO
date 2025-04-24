# docDEMO Documentation

## 🧾 Project Overview

This project, `docDEMO`, demonstrates the functionality of `doc-keeper.py`, a script that automatically generates documentation for a given codebase using the Gemini API. The project includes several Python scripts for code generation, review, and optimization, leveraging the Ollama API.  It also incorporates a GitHub Actions workflow to automate the documentation generation process upon pushes to the repository.

## ⚙️ Setup & Installation Instructions

1. **Clone the repository:**
   ```bash
   git clone https://github.com/your-username/your-repository.git
   ```

2. **Navigate to the project directory:**
   ```bash
   cd your-repository
   ```

3. **Create a virtual environment (recommended):**
   ```bash
   python3 -m venv .venv
   source .venv/bin/activate  # On Linux/macOS
   .venv\Scripts\activate  # On Windows
   ```

4. **Install required packages:**
   ```bash
   pip install -r requirements.txt
   ```

5. **Set up Gemini API Key:**
    - Obtain a Gemini API key from Google Cloud Console.
    - Create an environment variable named `GEMINI_API_KEY` and set its value to your Gemini API key.  This is the recommended way to securely store your key. Alternatively, you can directly insert the key into `doc-keeper.py`.

## 🧩 Explanation of Key Modules, Classes, and Functions

### coder.py

This module uses the Ollama API to generate code based on user prompts. It also saves the generated code to files and includes logging functionality.

- `log_message(message: str)`: Logs a message with a timestamp to the `codegen_log.txt` file in the `logs` directory.
- `generate_code_with_ollama(prompt: str) -> str`: Generates code using the Ollama API with 'qwen2.5-coder:0.5b' model and given prompt. Returns the generated code or a message if the prompt is not code-related.
- `save_file(content: str, file_type: str, name: str = "") -> str | None`: Saves the given content to a file with the specified file type in the `coder_folder` directory. Returns the file path or None if an error occurs.
- `create(user_prompt: str, name: str = "")`: Generates code using `generate_code_with_ollama` and saves it using `save_file`. Detects the file type from the first line of the generated code (like `python`, `c`, etc.)

### reviewer.py

This module utilizes the Ollama API to review code for potential issues and provides feedback.

- `log_message(message: str)`: Logs a message with a timestamp to the `review_log.txt` file in the `logs` directory.
- `read_code_from_file(file_path: str) -> str | None`: Reads and returns the content of the specified file. Logs messages on success or failure. Returns None if the file is empty or cannot be read.
- `review_code(code_content: str) -> str`: Sends the code content to the Ollama API ('qwen2.5-coder:0.5b' model) for review and returns the review report.  Includes a system prompt to guide the review process.
- `save_review(review_text: str, original_file: str) -> str | None`: Saves the review text to a `.txt` file in the `reviews` directory. The filename is based on the original file name and a timestamp.


### optimizer.py

This module interacts with the Ollama API to optimize given code for robustness and error handling.

- `log_message(message: str)`: Logs a message with a timestamp to the `optimizer_log.txt` file in the `logs` directory.
- `read_code_from_file(file_path: str) -> str | None`: Reads and returns the content of the specified file. Logs success or failure messages. Returns None if the file is empty or if reading fails.
- `optimize_code(code_content: str) -> str | None`: Sends the code content to the Ollama API ('qwen2.5-coder:0.5b' model) for optimization.  The system prompt instructs the API to focus on adding error handling and robustness checks.
- `save_optimized_code(code_text: str, original_file: str) -> str | None`: Saves the optimized code to a file in the `optim` folder. The file name is the same as the original file.


### coder_test.py

This module contains tests for the `coder.py` module, specifically for the `generate_code_with_ollama` and `save_file` functions. It uses the Ollama API for code generation.

- `generate_code_with_ollama(prompt: str) -> str`: Generates code using the Ollama API (identical to the function in `coder.py`).
- `save_file(content: str, file_type: str, name: str = "") -> str`: Saves content to a file (functionality matches `coder.py`).

### doc-keeper.py

This script is responsible for reading the repository files and generating documentation using the Gemini API.

- `IGNORE_EXTENSIONS`: Tuple containing file extensions to be ignored.
- `IGNORE_FILES`: Tuple containing filenames to be ignored.
- `IGNORE_DIRS`: Tuple containing directory names to be ignored.
- `read_repo_files(base_path: str) -> dict`: Recursively reads all readable files in the given directory, excluding specified files and directories. Returns a dictionary mapping relative file paths to their content.
- `generate_documentation(repo_files: dict) -> str`: Generates Markdown documentation using the Gemini API, based on the provided files. It constructs a prompt including overview, setup instructions, and code examples.  Uses 'gemini-1.5-pro' model. Limits individual file content to 3000 characters to avoid exceeding token limits.
- `write_documentation(doc_text: str, output_file: str = "DOCUMENTATION.md")`: Writes the given documentation text to the specified output file.


## 🗂 Folder & File Structure with Descriptions

- `.github/workflows/main.yml`: GitHub Actions workflow to automate documentation generation on push and manual trigger.
- `coder.py`: Module for generating code using the Ollama API.
- `coder.ipynb`: Jupyter Notebook for experimenting with the Ollama API.
- `coder_folder/`: Directory to store generated code files.
- `coder_test.py`: Test module for `coder.py`.
- `doc-keeper.py`: Script to generate documentation using the Gemini API.
- `logs/`: Directory to store log files from `coder.py`, `reviewer.py`, and `optimizer.py`.
- `optim/`: Directory to store optimized code files.
- `optimizer.py`: Module for optimizing code using the Ollama API.
- `README.md`:  Project's README file.
- `requirements.txt`:  List of required Python packages.
- `reviewer.py`: Module for code review using the Ollama API.
- `reviews/`: Directory to store code review files.


## 🔧 How to Use

This project primarily functions through the `doc-keeper.py` script and the GitHub Actions workflow.

1. **To generate documentation manually:**
   ```bash
   python doc-keeper.py
   ```
    This will create a `DOCUMENTATION.md` file in the project root directory.

2. **The GitHub Actions workflow** automatically generates documentation on pushes to the `main` branch. The workflow commits and pushes the updated `DOCUMENTATION.md` file back to the repository.


## 🤝 Contribution Guidelines (if applicable)

Not explicitly defined in the codebase.


## 🧪 Testing & Debugging Instructions (if test files exist)

- The `coder_test.py` file contains test functions for `coder.py`. You can run these tests using a test runner like `pytest`.  Ensure you have `pytest` installed: `pip install pytest`.  Then, run:
   ```bash
   pytest coder_test.py
   ```


The log files in the `logs` directory can be used for debugging. Each script (`coder.py`, `reviewer.py`, `optimizer.py`) writes its log messages to a separate file within this directory.



The incomplete `create` function in `coder.py` and inconsistent notebook should be addressed for full functionality. The missing timestamp in `reviewer.py`'s `save_review` also needs correction. Additionally, consider adding more robust error handling and input validation to prevent unexpected behavior and improve overall project quality. Ensure that all dependencies listed in `requirements.txt` are actually used in the project. It seems that `streamlit`, `langchain_core`, `langchain_ollama`, and `langchain_community` are listed but there's no code using them within provided scripts, so it's recommended to either use them or remove them from requirements to avoid installing potentially unused modules by users working with provided setup instructions.