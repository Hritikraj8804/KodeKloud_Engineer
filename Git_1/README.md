# Git Practice Tasks (KodeKloud)

A repository dedicated to practicing and demonstrating core Git commands and workflows learned from KodeKloud labs and courses. This project serves as a personal workbook and reference for mastering Git essentials.

## 🚀 Overview

This repository contains solutions and practice files for various Git tasks, primarily inspired by the excellent labs and challenges provided by KodeKloud. The goal is to solidify understanding of fundamental Git concepts, including:

* **Repository Initialization & Cloning:** Setting up new projects and getting existing ones.
* **Staging & Committing Changes:** Tracking and saving modifications.
* **Branching & Merging:** Working on isolated features and integrating them.
* **Remote Repositories:** Collaborating and syncing with GitHub/GitLab.
* **History Management:** Undoing changes, inspecting logs, and rebasing.
* **Tagging & Aliases:** Marking important versions and creating shortcuts.

Each directory or set of files within this repo corresponds to a specific Git concept or task, allowing for focused practice and easy review.

## 🌟 Getting Started

To explore or practice with these tasks, you'll need Git installed on your system.

### Prerequisites

* `Git` (You can download it from [git-scm.com](https://git-scm.com/downloads))

### Installation

1.  **Clone this repository:**
    ```bash
    git clone [https://github.com/your-username/kodekloud-git-tasks.git](https://github.com/your-username/kodekloud-git-tasks.git)
    cd kodekloud-git-tasks
    ```
    *(Replace `your-username` with your GitHub username)*

2.  You are now inside the repository and can navigate to specific task directories.

## 🏃 Usage / Exploring Tasks

Each task or concept typically resides in its own subdirectory. You can navigate into these directories and perform the specified Git operations.

**Example Workflow for a specific task (e.g., `branching-merging-challenge`):**

1.  **Navigate to the task directory:**
    ```bash
    cd branching-merging-challenge
    ```
2.  **Examine the instructions (if any) or existing files.**
3.  **Perform Git commands as per the KodeKloud lab or your understanding.**
    * `git branch new-feature`
    * `git checkout new-feature`
    * `# Make some changes...`
    * `git add .`
    * `git commit -m "Add new feature"`
    * `git checkout main`
    * `git merge new-feature`
    * etc.
4.  **Feel free to reset or re-practice:**
    * `git reset --hard origin/main` (Use with caution! This will revert your changes to the last pushed state of `main`.)
