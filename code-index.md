# Cline Codebase Indexing Prompt

## Goal  
Index this codebase by generating a hierarchical documentation structure that follows the folder structure of the project. The goal is to create a table of contents that enables efficient retrieval of relevant code context.  

## Structure  

### 1. Recursive Folder and File Indexing  
- **Process all directories and subdirectories recursively.**  
- Create a root-level summary that lists all major directories and their purpose.  
- Each directory should link to a subpage containing more details about its contents.  
- If a directory contains subdirectories, create additional nested sections to reflect the hierarchy.  

### 2. Subpages and Details  
- For each folder, generate a summary of its purpose and key components.  
- Identify important files, their roles, and how they relate to other parts of the codebase.  
- For each file, list relevant classes, functions, and key constants along with a brief description.  
- If a file has significant functions or classes, create nested sections with their names, descriptions, and references to line numbers.  

### 3. Cross-Referencing for Retrieval  
- Include an index of key features and modules (e.g., “Reports,” “Database Models,” “APIs”).  
- Link back to relevant files and functions.  
- Ensure reports, APIs, and business logic components are easy to locate based on function names or file paths.  

### 4. Code References  
- If possible, include references to line numbers and function signatures for precision.  
- Capture dependencies between modules where applicable.  

### 5. Formatting Requirements  
- **All documentation must be generated in Markdown format (.md).**  
- Use proper Markdown syntax for headings, lists, and links.  
- Format code snippets using triple backticks (```) and specify the language when applicable.  

## Expected Outcome  
The final structure should allow for intuitive searches, enabling retrieval of relevant sections when queried about specific features, reports, or modules.  
The documentation should **mirror the folder structure exactly, traversing all directories and subdirectories recursively.**  
All generated files should be formatted as Markdown for consistency and readability.  
