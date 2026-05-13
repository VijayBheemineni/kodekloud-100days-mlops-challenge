# Task
The xFusionCorp Industries ML team is adopting DVC so that datasets and model files are versioned separately from code. Initialise DVC inside the existing Git repository at /root/code/fraud-detection/ and record the initialisation in Git.
# Fix
```
dvc init
git add .dvc/ .dvcignore
git commit -m "Initialize DVC"
```
# Data Version Control (DVC)

**Data Version Control (DVC)** is an open-source command-line tool designed to manage large files, datasets, and machine learning models. It bridges the gap between **Git** (which handles code) and **Remote Storage** (which handles data).

---

## 1. The Problem: Git's "Heavy Data" Limitation
Git is designed to track changes in text files. If you attempt to store large binary files (like a 2GB CSV dataset or a 500MB model weight file) directly in Git:
*   The repository becomes massive and slow.
*   Cloning the repo takes hours.
*   Standard Git hosting services (GitHub, GitLab) often block files over 100MB.

---

## 2. The Solution: How DVC Works
DVC allows you to keep your data outside of Git while still versioning it alongside your code. 

### The "Pointer" System
When you track a file with DVC, it creates a **sidecar file** (a pointer).
*   **The Data:** Your heavy file (e.g., `dataset.zip`) is moved to a hidden cache and added to `.gitignore`.
*   **The Pointer:** A tiny text file (e.g., `dataset.zip.dvc`) is created. This file contains a unique **MD5 hash** (a fingerprint) of the data.

You commit the tiny `.dvc` file to Git. This ensures that every version of your code is linked to a specific, immutable version of your data.



---

## 3. Example Workflow: Cat vs. Dog Classifier

Imagine you are building an AI to identify pets.

### Step A: Initial Dataset
You have a folder called `images/` containing 10,000 photos (5GB).
1.  **Track with DVC:** `dvc add images/`
2.  **Result:** DVC creates `images.dvc`. The actual 5GB of images are now ignored by Git.
3.  **Git Commit:** `git add images.dvc && git commit -m "First 10k images"`

### Step B: Updating Data
You receive 5,000 more photos.
1.  **Update DVC:** `dvc add images/` (DVC detects the change and updates the hash).
2.  **Git Commit:** `git add images.dvc && git commit -m "Added 5k more images"`

### Step C: Switching Versions (Time Travel)
If your new model performs poorly and you need to go back to the original 10,000 images:
1.  **Git Checkout:** `git checkout <previous-commit-hash>` (This brings back the old version of the `.dvc` pointer).
2.  **DVC Checkout:** `dvc checkout` (DVC reads the pointer and instantly swaps the physical images in your folder to match that version).

---

## 4. Key Benefits

| Feature | Description |
| :--- | :--- |
| **Storage Agnostic** | Store your data on S3, Google Cloud, Azure, or even a simple network drive (SFTP/SSH). |
| **Reproducibility** | Anyone can clone your repo and run `dvc pull` to get the *exact* dataset you used to get your results. |
| **No Duplication** | DVC uses "reflinks" or "hardlinks" so that switching between versions is nearly instantaneous and doesn't waste disk space. |
| **ML Pipelines** | DVC can also track the "recipe" of your data (e.g., which script produced which file), ensuring the entire workflow is versioned. |

---

## 5. Summary
DVC acts as a **librarian**. Git holds the **catalog cards** (the `.dvc` files) that tell you which "book" belongs to which version of your project. DVC is the one who goes into the **storage room** (the Remote Cache) to fetch the actual **heavy books** (the data) whenever you need them.