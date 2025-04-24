# docDEMO Documentation

🧾 **Project Overview**

This project, `docDEMO`, demonstrates the functionality of `doc-keeper.py`, a script that automatically generates documentation for a given codebase using Google Gemini.  The project includes several Python scripts that interact with the Ollama language model for code generation, review, and optimization. It also includes a GitHub Actions workflow to automate the documentation generation process.

⚙️ **Setup & Installation Instructions**

1. **Clone the repository:**
   ```bash
   git clone https://github.com/your-username/docDEMO.git  # Replace with your repo URL
   cd docDEMO
   ```

2. **Create and activate a virtual environment:**
   ```bash
   python3 -m venv .venv
   source .venv/bin/activate  # On Linux/macOS
   .venv\Scripts\activate  # On Windows
   ```

3. **Install dependencies:**
   ```bash
   pip install -r requirements.txt
   ```

4. **Set up Gemini API Key:**
    - Obtain a Gemini API key from Google Cloud Console.
    - Set the `GEMINI_API_KEY` environment variable. This is the recommended way to handle API keys for security.


🧩 **Explanation of Key Modules, Classes, and Functions**

- **`coder.py`**: This module handles code generation using the Ollama API.
    - `log_message(message: str)`: Logs a message with a timestamp to `codegen_log.txt`.
    - `generate_code_with_ollama(prompt: str) -> str`: Generates code based on the given prompt.  Returns the generated code or a message if the prompt isn't code-related.
    - `save_file(content: str, file_type: str, name: str = "") -> str | None`: Saves the generated code to a file in the `coder_folder` directory. Returns the file path or None if an error occurs.
    - `create(user_prompt: str, name: str = "")`: Generates code and saves it. Combines `generate_code_with_ollama` and `save_file`.

- **`reviewer.py`**: This module handles code review using the Ollama API.
    - `log_message(message: str)`: Logs a message with a timestamp to `review_log.txt`.
    - `read_code_from_file(file_path: str) -> str | None`: Reads and returns the content of a file. Returns None if the file is empty or if an error occurs.
    - `review_code(code_content: str) -> str`: Sends code to Ollama for review and returns the review report.
    - `save_review(review_text: str, original_file: str)`: Saves the review to a file in the `reviews` folder.

- **`optimizer.py`**: This module handles code optimization using the Ollama API.
    - `log_message(message: str)`: Logs a message with a timestamp to `optimizer_log.txt`.
    - `read_code_from_file(file_path: str) -> str | None`: Reads and returns the content of a file. Returns None if the file is empty or if an error occurs.
    - `optimize_code(code_content: str) -> str | None`: Sends code to Ollama for optimization and returns the optimized code. Returns None if an error occurs.
    - `save_optimized_code(code_text: str, original_file: str) -> str | None`: Saves the optimized code to a file in the `optim` folder. Returns the path or None upon failure.
    - `main()`: Main function to handle command-line arguments and run the optimization.


- **`doc-keeper.py`**: This module uses Gemini to generate documentation for the repository.
    - `IGNORE_EXTENSIONS`, `IGNORE_FILES`, `IGNORE_DIRS`: Tuples containing files and folders to exclude from documentation generation.
    - `read_repo_files(base_path: str) -> dict`: Recursively reads all allowed files in the repository and returns a dictionary mapping file paths to content.
    - `generate_documentation(repo_files: dict) -> str`: Generates Markdown documentation using the Gemini API.
    - `write_documentation(doc_text: str, output_file: str = "DOCUMENTATION.md")`: Writes the generated documentation to a Markdown file.

- **`coder_test.py`**: Contains tests for the `coder.py` module.
    - `generate_code_with_ollama(prompt: str) -> str`:  (Same as in `coder.py`)
    - `save_file(content: str, file_type: str, name: str = "") -> str`: (Similar to `coder.py`, appears incomplete in provided code)


🗂 **Folder & File Structure with Descriptions**

```
docDEMO/
├── .github/workflows/main.yml        # GitHub Actions workflow for documentation generation.
├── coder.py                         # Code generation module.
├── coder.ipynb                       # Jupyter Notebook for interacting with Ollama.
├── coder_folder/                   # Folder to store generated code.
├── coder_test.py                     # Tests for coder.py.
├── doc-keeper.py                     # Documentation generation script.
├── logs/                            # Folder for log files.
├── optim/                           # Folder to store optimized code.
├── README.md                        # Project description.
├── requirements.txt                  # Project dependencies.
├── reviewer.py                      # Code review module.
└── reviews/                         # Folder to store code reviews.
```


🔧 **How to Use**

**Code Generation (using `coder.py`):**

```bash
python coder.py <prompt> <file_type> <optional_file_name>
```

Example:
```bash
python coder.py "write a python function to calculate factorial" py factorial
```

**Code Review (using `reviewer.py`):**

```bash
python reviewer.py <file_path>
```

Example:
```bash
python reviewer.py coder_folder/factorial.py
```

**Code Optimization (using `optimizer.py`):**

```bash
python optimizer.py <file_path>
```

Example:
```bash
python optimizer.py coder_folder/factorial.py
```

**Documentation Generation (using `doc-keeper.py`):**

```bash
python doc-keeper.py
```
This will generate `DOCUMENTATION.md` in the project root.


🤝 **Contribution Guidelines**

Not explicitly defined in the project.


🧪 **Testing & Debugging Instructions**

- **`coder_test.py`**: This file contains tests (currently incomplete) for `coder.py`. You can run these tests using a testing framework like `pytest`.

- The logging functions in `coder.py`, `reviewer.py`, and `optimizer.py` write logs to files in the `logs` folder.  These logs can be useful for debugging.


The complete code with docstrings is too large to fit in this response.  However, the provided explanations and structure should help you understand the project and navigate the codebase. You can easily add the docstrings to the original files based on the function descriptions given here.  Key changes would be adding the type hints (e.g., `-> str`) and detailed descriptions within the triple quotes.