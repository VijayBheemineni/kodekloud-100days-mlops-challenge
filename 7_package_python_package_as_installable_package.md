# Task 
 The team needs the fraud-detection model code packaged as an installable Python distribution. A draft pyproject.toml exists at /root/code/fraud-detection/, but it does not build a wheel that meets the team's standard. Correct the file and produce a compliant package.

 # Fix

 ```
 # pyproject.toml file before fix
 [project]
name = "fraud-detection"
version = "0.0.1"
description = "Fraud detection model for xFusionCorp Industries"
requires-python = ">=3.8"
dependencies = []

[tool.setuptools.packages.find]
where = ["src"]
 ```

 ```
 # After fix
 [project]
name = "fraud_detection"
version = "0.1.0"
description = "Fraud detection model for xFusionCorp Industries"
requires-python = ">=3.10"
dependencies = [
    "scikit-learn",
    "pandas",
    "numpy"
]

[build-system]
requires = ["setuptools>=61.0", "wheel"]
build-backend = "setuptools.build_meta"

[tool.setuptools.packages.find]
where = ["src"]
 ```

 ```
 python3 -m build
 ```

 # What happens when we run 'python3 -m build'
 When you run python3 -m build, a very specific sequence of events happens:

    1) The Isolated Environment: build creates a temporary, clean folder (an isolated environment).

    2) The Requirements Check: It looks at your [build-system] requires list. It says, "Okay, I need to download setuptools and wheel into this temporary folder so I have the right tools to work with."

    3) The Engine Start: It looks at build-backend = "setuptools.build_meta". It "calls" that engine and says, "Hey Setuptools, I have a project here. Please look at the [project] settings and the src/ folder and give me the finished products."

    4) The Output: The engine (setuptools) reads your pyproject.toml, finds your code, and spits out the two files into your dist/ folder:

        The sdist (the .tar.gz source).

        The wheel (the .whl built distribution).

# What is wheel?
- In python world 'wheel' is a built distribution. a Wheel is just a ZIP file with a special extension. It contains the code files already placed in the folders where Python expects to find them, along with a metadata folder that tells Python which libraries (like pandas or numpy) need to be installed alongside it.

```
# Install the wheel
pip install dist/fraud_detection-0.1.0-py3-none-any.whl
```
