# Makefile 
`makefile` is generally used to automate common daily tasks.

# Task
When we run `make all` we get error `Makefile:7: *** missing separator.  Stop.`. Basically we get this error because `makefile` uses `tabs` to identify the commands. At line 7 spaces we used.

```
# Add 'pytest' to requirements.txt. Without this package make will fail.
```

```
# Fixed make file
# fraud-detection Makefile

# Declare all targets as .PHONY to prevent conflicts with files of the same name
.PHONY: setup data train test clean all

# Default target
all: setup data train test

# Creates a virtual environment and installs dependencies
setup:
	python3 -m venv mlops-venv
	./mlops-venv/bin/pip install -r requirements.txt

# Runs the data processing script
data:
	./mlops-venv/bin/python src/data/process_data.py

# Runs the model training script
train:
	./mlops-venv/bin/python src/models/train.py

# Runs the test suite using pytest
test:
	./mlops-venv/bin/python -m pytest tests/

# Cleans up the environment and artifacts
clean:
	find . -type d -name "__pycache__" -exec rm -rf {} +
	rm -rf .pytest_cache
	rm -rf models/*
```