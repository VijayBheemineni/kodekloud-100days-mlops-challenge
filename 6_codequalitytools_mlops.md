# Task
Update 'pyproject.toml' with settings for various tools like 'black', 'ruff' etc.

# Fix
```
# Original Pyproject.toml
[project]
name = "fraud-detection"
version = "0.1.0"

[tool.ruff]
line-length = 88
select = ["E", "F"]

[tool.black]
line-length = 100
```

```
# Fixed pyproject.toml
[project]
name = "fraud-detection"
version = "0.1.0"

[tool.ruff]
line-length = 120
[tool.ruff.lint]
select = ["E", "F", "W", "I"]

[tool.black]
line-length = 120
```

```
# run 'ruff'. We get below error. So fix with '--fix' option.
 ruff check src
I001 [*] Import block is un-sorted or un-formatted
 --> src/data/process_data.py:1:1
  |
1 | / import os
2 | | import pandas as pd
  | |___________________^
  |
help: Organize imports

F401 [*] `os` imported but unused
 --> src/data/process_data.py:1:8
  |
1 | import os
  |        ^^
2 | import pandas as pd
  |
help: Remove unused import: `os`

Found 2 errors.
[*] 2 fixable with the `--fix` option.

# run 'ruff' with '--fix' option. 
ruff check src --fix
Found 1 error (1 fixed, 0 remaining).

# run 'ruff' again. Now it should pass with 0 errors
ruff check src
All checks passed!
 echo $?
0

# Run 'black'
 black --check src/
All done! ✨ 🍰 ✨
5 files would be left unchanged.

 echo $?
0
```

# What is 'pyproject.toml'
'pyproject.toml' is a configuration file that tells Python tools how to build your project, what libraries it needs, and how various development tools (like linters and formatters) should behave. Before 'pyproject.toml' where python tool has its own configuration file which was tough to maintain.
# What are 'black' and 'ruff'
- 'black' is code formatter. It doesn't care if your code is "right" or "wrong" logically; it only cares how it looks.
- 'ruff' is a 'linter'. It checks code for "smells"—actual errors, unused variables, or bad habits.It can replace dozens of older tools. It can check your logic, sort your imports (replacing isort), and even automatically fix many of the problems it finds.

In a modern workflow, you use Ruff to make sure your code is smart and error-free, and you use Black to make sure your code is pretty. By putting their settings in pyproject.toml, you ensure that every developer on your team is using the exact same rules.