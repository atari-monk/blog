# Comments

## U allways produce code with comments, i need something to remove them, maybe py script that will take clipboard contect, remove comments and paste it back to clipboard, with no change to format of code ?

Sure! Here's a Python script that will take the content from the clipboard, remove the comments, and paste it back to the clipboard. This script will handle both single-line (`# comment`) and multi-line comments (`''' comment '''` or `""" comment """`), and ensure that the format of the code remains unchanged.

Make sure to install `pyperclip` by running:

```bash
pip install pyperclip
```

Now, here's the Python script:

```python
import pyperclip
import re

def remove_comments(code):
    # Remove single-line comments (everything after a #)
    code_no_single_line_comments = re.sub(r'#.*$', '', code, flags=re.MULTILINE)

    # Remove multi-line comments (everything between ''' or """)
    code_no_comments = re.sub(r'(?s)\'\'\'(.*?)\'\'\'', '', code_no_single_line_comments)
    code_no_comments = re.sub(r'(?s)\"\"\"(.*?)\"\"\"', '', code_no_comments)
    
    return code_no_comments

def main():
    # Get the code from the clipboard
    code = pyperclip.paste()
    
    # Remove comments
    cleaned_code = remove_comments(code)
    
    # Paste the cleaned code back to the clipboard
    pyperclip.copy(cleaned_code)
    print("Comments removed and code copied back to clipboard!")

if __name__ == "__main__":
    main()
```

### How it works:
1. It grabs the code from the clipboard.
2. The `remove_comments()` function removes:
   - Single-line comments (anything following `#`).
   - Multi-line comments (inside `'''` or `"""`).
3. The cleaned code is then copied back to the clipboard.

### Usage:
- Copy any code with comments to your clipboard.
- Run this script, and it will remove the comments and put the cleaned code back in your clipboard.

Let me know if you'd like to customize or improve it further!

## So this will work for comments in ts ?

The script I provided is designed for Python and will only work for Python-style comments. In TypeScript (TS), comments are similar, but we need to account for both single-line (`// comment`) and multi-line (`/* comment */`) comments instead.

Here’s an updated version of the script that works for TypeScript, removing single-line and multi-line comments:

```python
import pyperclip
import re

def remove_comments(code):
    # Remove single-line comments (everything after a //)
    code_no_single_line_comments = re.sub(r'//.*$', '', code, flags=re.MULTILINE)

    # Remove multi-line comments (everything between /* and */)
    code_no_comments = re.sub(r'(?s)/\*.*?\*/', '', code_no_single_line_comments)
    
    return code_no_comments

def main():
    # Get the code from the clipboard
    code = pyperclip.paste()
    
    # Remove comments
    cleaned_code = remove_comments(code)
    
    # Paste the cleaned code back to the clipboard
    pyperclip.copy(cleaned_code)
    print("Comments removed and code copied back to clipboard!")

if __name__ == "__main__":
    main()
```

### Key Changes:
- **Single-line comments** (`// comment`) are now handled by `r'//.*$'`, which matches any comment starting with `//`.
- **Multi-line comments** (`/* comment */`) are handled by `r'(?s)/\*.*?\*/'`, which matches everything between `/*` and `*/`.

### Usage:
- Copy your TypeScript code with comments to the clipboard.
- Run the script, and it will remove all comments (single-line and multi-line), and the cleaned code will be copied back to your clipboard.

Let me know if you need further adjustments!

## Can we habdle both ? also make a script with lib functions and then use in main (i like to have lib functions)

Certainly! I can create a more modular version of the script with functions organized in a separate library style and allow it to handle both Python and TypeScript comments.

### Here's the updated version with modular functions and handling both Python and TypeScript comments:

```python
import pyperclip
import re

# Lib functions for removing comments from code
def remove_python_comments(code: str) -> str:
    """
    Removes Python-style comments from the provided code.
    """
    # Remove single-line comments (everything after a #)
    code_no_single_line_comments = re.sub(r'#.*$', '', code, flags=re.MULTILINE)
    
    # Remove multi-line comments (everything between ''' and ''' or """ and """)
    code_no_comments = re.sub(r'(?s)\'\'\'(.*?)\'\'\'', '', code_no_single_line_comments)
    code_no_comments = re.sub(r'(?s)\"\"\"(.*?)\"\"\"', '', code_no_comments)
    
    return code_no_comments

def remove_ts_comments(code: str) -> str:
    """
    Removes TypeScript-style comments from the provided code.
    """
    # Remove single-line comments (everything after a //)
    code_no_single_line_comments = re.sub(r'//.*$', '', code, flags=re.MULTILINE)

    # Remove multi-line comments (everything between /* and */)
    code_no_comments = re.sub(r'(?s)/\*.*?\*/', '', code_no_single_line_comments)
    
    return code_no_comments

def remove_comments(code: str, language: str) -> str:
    """
    Removes comments from the provided code based on the language type.
    """
    if language == 'python':
        return remove_python_comments(code)
    elif language == 'typescript':
        return remove_ts_comments(code)
    else:
        raise ValueError(f"Unsupported language: {language}")

# Main function to interact with clipboard and remove comments
def main():
    # Get the code from the clipboard
    code = pyperclip.paste()
    
    # Ask the user for the programming language
    language = input("Enter the programming language (python/typescript): ").strip().lower()
    
    # Remove comments based on the language
    try:
        cleaned_code = remove_comments(code, language)
        
        # Paste the cleaned code back to the clipboard
        pyperclip.copy(cleaned_code)
        print("Comments removed and code copied back to clipboard!")
    except ValueError as e:
        print(e)

if __name__ == "__main__":
    main()
```

### Explanation of Changes:
1. **Library Functions**:
   - `remove_python_comments`: Removes comments in Python code (`#`, `'''`, and `"""`).
   - `remove_ts_comments`: Removes comments in TypeScript code (`//`, `/* */`).
   - `remove_comments`: A higher-level function that calls the appropriate function based on the `language` input (either `python` or `typescript`).

2. **Main Function**:
   - The script gets the code from the clipboard and asks the user to input the programming language (`python` or `typescript`).
   - Based on the input, it removes the appropriate comments and copies the cleaned code back to the clipboard.

### How to Use:
1. Copy the code you want to clean (in Python or TypeScript) to your clipboard.
2. Run the script.
3. When prompted, enter either `python` or `typescript` to specify the language.
4. The cleaned code will be copied back to your clipboard.

### Example:

- **Input**: Python code with comments in the clipboard.
- **Output**: Python code without comments copied back to the clipboard.

Let me know if you have any further adjustments or questions!

## Cant we detect witch it is auto ?

Yes, we can automatically detect whether the code is Python or TypeScript based on its content. For example, we can check for certain patterns in the code, such as Python-specific syntax (e.g., `def`, `import`, `print`) or TypeScript-specific syntax (e.g., `interface`, `import`, `let`, `const`).

Here’s an updated version of the script where we attempt to detect the language automatically:

### Updated Script:

```python
import pyperclip
import re

# Lib functions for removing comments from code
def remove_python_comments(code: str) -> str:
    """
    Removes Python-style comments from the provided code.
    """
    # Remove single-line comments (everything after a #)
    code_no_single_line_comments = re.sub(r'#.*$', '', code, flags=re.MULTILINE)
    
    # Remove multi-line comments (everything between ''' and ''' or """ and """)
    code_no_comments = re.sub(r'(?s)\'\'\'(.*?)\'\'\'', '', code_no_single_line_comments)
    code_no_comments = re.sub(r'(?s)\"\"\"(.*?)\"\"\"', '', code_no_comments)
    
    return code_no_comments

def remove_ts_comments(code: str) -> str:
    """
    Removes TypeScript-style comments from the provided code.
    """
    # Remove single-line comments (everything after a //)
    code_no_single_line_comments = re.sub(r'//.*$', '', code, flags=re.MULTILINE)

    # Remove multi-line comments (everything between /* and */)
    code_no_comments = re.sub(r'(?s)/\*.*?\*/', '', code_no_single_line_comments)
    
    return code_no_comments

def detect_language(code: str) -> str:
    """
    Detects the language (Python or TypeScript) from the provided code.
    """
    # Check for Python-specific keywords
    python_keywords = ['def', 'import', 'print', 'class', 'self']
    
    if any(keyword in code for keyword in python_keywords):
        return 'python'

    # Check for TypeScript-specific keywords
    ts_keywords = ['interface', 'import', 'let', 'const', 'function']
    
    if any(keyword in code for keyword in ts_keywords):
        return 'typescript'
    
    # If neither is detected, return an unknown language
    return 'unknown'

def remove_comments(code: str) -> str:
    """
    Removes comments from the provided code based on the detected language.
    """
    language = detect_language(code)
    
    if language == 'python':
        return remove_python_comments(code)
    elif language == 'typescript':
        return remove_ts_comments(code)
    else:
        raise ValueError(f"Could not detect language. Please specify it manually.")

# Main function to interact with clipboard and remove comments
def main():
    # Get the code from the clipboard
    code = pyperclip.paste()
    
    # Detect the language (Python or TypeScript)
    try:
        cleaned_code = remove_comments(code)
        
        # Paste the cleaned code back to the clipboard
        pyperclip.copy(cleaned_code)
        print("Comments removed and code copied back to clipboard!")
    except ValueError as e:
        print(e)

if __name__ == "__main__":
    main()
```

### Changes:
1. **`detect_language` Function**: 
   - This function scans the code for specific keywords that are unique to Python or TypeScript.
   - It checks for Python-specific keywords (`def`, `import`, `print`, etc.) and TypeScript-specific keywords (`interface`, `let`, `const`, etc.).
   
2. **Automatic Language Detection**:
   - The `remove_comments` function now calls `detect_language` to automatically determine whether the code is Python or TypeScript based on its content.
   
3. **Fallback**: 
   - If neither Python nor TypeScript keywords are found, it raises an error and asks for manual specification. You can handle unknown languages as you prefer.

### How to Use:
1. Copy the code (Python or TypeScript) to your clipboard.
2. Run the script, and it will automatically detect whether it's Python or TypeScript.
3. It will remove the comments and copy the cleaned code back to your clipboard.

### Example:

- **Input (Python Code with comments)**:  
  ```python
  # This is a comment
  def hello():
      print("Hello, world!")  # Another comment
  ```

- **Input (TypeScript Code with comments)**:  
  ```typescript
  // This is a comment
  function greet() {
      console.log("Hello, TypeScript!"); // Another comment
  }
  ```

- **Output**: The comments will be removed and the cleaned code copied to the clipboard.

This approach should work in most cases! Let me know if you'd like further tweaks.