# Task
The xFusionCorp Industries ML platform team maintains a Cookiecutter template that new ML projects are generated from. A draft template exists at /root/code/mlops-template/, but it does not render. Correct the template and use it to generate a project.
# Fix

- 'cookiecutter.json'
```
# Fixed
{
    "project_name": "my-ml-project",
    "author": "xFusionCorp",
    "python_version": "3.11",
    "ml_framework": ["sklearn", "pytorch", "tensorflow"]
}
```
- 'requirements.txt'
```
# Fixed
{% if cookiecutter.ml_framework == 'sklearn' %}
scikit-learn
{% elif cookiecutter.ml_framework == 'pytorch' %}
torch
{% elif cookiecutter.ml_framework == 'tensorflow' %}
tensorflow
{% endif %}
```
- 'README.md'
```
# Fixed
# {{cookiecutter.project_name}}

Created by {{ cookiecutter.author }}.

```

- Command to create project based on template.
```
 cookiecutter /root/code/mlops-template/ -o /root/code/ --no-input project_name=churn-model ml_framework=sklearn
```

# cookiecutter
## 1. What is Cookiecutter?

**Cookiecutter** is a command-line utility that creates projects from **project templates**. 

Think of it like a **digital stencil** or a **muffin tin**. Instead of manually creating the same folders (`src/`, `tests/`, `data/`) and files (`README.md`, `Makefile`, `pyproject.toml`) every time you start a new machine learning project, you use a template. Cookiecutter "pours" your specific project details into that template to bake a perfectly structured project in seconds.

### How it Works
1.  **The Template:** You create a blueprint folder. Inside, filenames and variables are wrapped in placeholders like `{{ cookiecutter.project_name }}`.
2.  **The Input:** When you run the tool, it asks you interactive questions (e.g., "What is the project name?", "Which ML framework?").
3.  **The Output:** Cookiecutter replaces all placeholders with your specific answers and generates a ready-to-use directory.

---

## 3. Why use it for ML Engineering?

For a Senior Platform Engineer, Cookiecutter is a productivity booster that ensures project consistency across a team:

*   **Standardization:** Every project at your organization will follow the same directory structure. This makes it easy for other engineers to navigate your code.
*   **Best Practices:** You can bake your `Makefile`, `pre-commit` configs, and `pyproject.toml` settings (like the 120-character line length) directly into the template.
*   **Automation:** It eliminates "boilerplate fatigue." You spend less time setting up folders and more time writing model logic.