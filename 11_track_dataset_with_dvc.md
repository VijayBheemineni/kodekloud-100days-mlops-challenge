# Task
A teammate has added the transactions dataset to the xFusionCorp Industries fraud-detection repository, but it was committed directly to Git instead of being tracked with DVC. Bring the repository in line with the team standard—every dataset under data/ must be tracked by DVC, not by Git.
# Fix
```
# Check if 'data/raw/transactions.csv' is tracked by git.
git ls-files data/raw/transactions.csv
# or
git status data/raw/transactions.csv
# or
git log --oneline -- data/raw/transactions.csv

# Remove the dataset from git tracking
git rm --cached data/raw/transactions.csv

# Track dataset with DVC
dvc add data/raw/transactions.csv

# Add all files to git and commit
git add data/raw/transactions.csv data/raw/transactions.csv.dvc data/raw/.gitignore
git commit -m "Track transactions dataset with DVC"

# Validate 
dvc list . data/raw
# or
dvc list . -R --dvc-only
# or
dvc status
```

# What happens when 'dvc add'
- Calculate Hash :- DVC reads the content of 'transacation.csv' and calculates MD5.
- Move data to cache :- copies the file to hidden directory '.dvc/cache'. And file is stored with 'MD5' hash name.
- Creates a pointer :- It creates 'transactions.csv.dvc' file with hash and other details.
- Create .gitignore :- created '.gitignore' in same folder as 'transactions.csv' and adds 'transactions.csv'
```
outs:
- md5: c4dd797a1451653b3cb76a8bb7b2b4d9
  size: 379
  hash: md5
  path: transactions.csv
```