# Homework 3 System Requirements and Verification

## Baseline

## User needs

| ID | Stakeholder need |
|---|---|
| UN-GIT-01 | A student developer needs a way to start tracking a local project because it has no recorded history. |
| UN-GIT-02 | A student developer needs to know which project files have changed because they may forget what they edited before recording a checkpoint. |
| UN-GIT-03 | A student developer needs to inspect changed content before recording it because a file may contain unintended edits. 
| UN-GIT-04 | A student developer needs to choose the file content to include in the next checkpoint because later edits may still be unfinished. |
| UN-GIT-05 | A student developer needs to record a meaningful checkpoint because they want to preserve a known project state and explain its purpose. |
| UN-GIT-06 | A student developer needs to review earlier checkpoints because they want to understand how the project reached its current state. |
| UN-GIT-07 | A student developer needs invalid commands to explain why they failed while preserving existing project files and recorded checkpoints. |

## User requirements

| ID | User-visible capability | Need |
|---|---|---|
| UR-GIT-01 | A student developer shall be able to initialize tracking in the current local project folder without removing existing project files. | UN-GIT-01, UN-GIT-07 |
| UR-GIT-02 | A student developer shall be able to see whether project files are untracked, staged, changed after staging, modified, deleted, or clean. | UN-GIT-02 |
| UR-GIT-03 | A student developer shall be able to view differences between current working file content and the content selected for the next checkpoint. | UN-GIT-03 |
| UR-GIT-04 | A student developer shall be able to view differences between content selected for the next checkpoint and the latest recorded checkpoint. | UN-GIT-03 |
| UR-GIT-05 | A student developer shall be able to select the current content of one existing project file for the next checkpoint without selecting unrelated files. | UN-GIT-04 |
| UR-GIT-06 | A student developer shall be able to create a checkpoint of selected content with a nonempty explanation while leaving later unselected edits in the working files. | UN-GIT-05, UN-GIT-04 |
| UR-GIT-07 | A student developer shall be able to view recorded checkpoints from newest to oldest, including their identifier and explanation. | UN-GIT-06 |
| UR-GIT-08 | A student developer shall receive a useful error when a command is invalid, a requested file is unavailable, or a path is outside the allowed project files. | UN-GIT-07 |
| UR-GIT-09 | A student developer shall be able to retry an operation after a failure without losing ordinary project files or an already recorded checkpoint. | UN-GIT-07 |

## Functional System Requirements

### SR-01 — Initialize a project
**SR-01 (source UR-GIT-01):** Given an uninitialized project, when `init` is run, MiniGit shall initialize the project without removing existing files.
**Verify:** Verify that the project becomes initialized and that existing project files are still present.

### SR-02 — Run init again
**SR-02 (source UR-GIT-01):** Given an initialized project, when `init` is run again, MiniGit shall not change the existing files at all.
**Verify:** Verify that the files and project state are unchanged after running `init` again.

### SR-03 — Add one file
**SR-03 (source UR-GIT-05):** Given two existing files, when `add notes.txt` is run, MiniGit shall stage `notes.txt` only.
**Verify:** Verify that `notes.txt` is staged and the other file isn't staged.

### SR-04 — Add a missing file
**SR-04 (source UR-GIT-08, UR-GIT-09):** Given a staged file, when `add missing.txt` is run, MiniGit shall show an error and the staged file should remain unchanged.
**Verify:** Verify that an error is shown and the staged file is still staged.

### SR-05 — Show file status
**SR-05 (source UR-GIT-02):** Given a staged file, when `status` is run, MiniGit shall show that the file is staged.
**Verify:** Run `status` and verify that the staged file is shown as staged as it should remain like that.

### SR-06 — Show file differences
**SR-06 (source UR-GIT-03):** Given a changed file, when `diff` is run, MiniGit shall show the changes.
**Verify:** Run `diff` and verify that the changed content is shown.

### SR-07 — Show staged differences
**SR-07 (source UR-GIT-04):** Given a staged file, when `diff --staged` is run, MiniGit shall show the staged changes.
**Verify:** Run `diff --staged` and verify that the staged changes are shown.

## SR-08 — Create a checkpoint
**SR-08 (source UR-GIT-06):** Given staged content, when `commit -m "message"` is run, MiniGit shall create a checkpoint and a message should be shown.
**Verify:** Run `commit -m "message"` and verify that a new checkpoint is created with the message.

### SR-09 — Empty commit message
**SR-09 (source UR-GIT-08, UR-GIT-09):** Given staged content, when `commit -m ""` is run, MiniGit shall show an error, no new checkpoint should be created at all.
**Verify:** Run `commit -m ""` and verify that an error is shown and no new checkpoint is created.

### SR-10 — View checkpoints
**SR-10 (source UR-GIT-07):** The recorded checkpoints that we have, when `log` is run, MiniGit shall show the checkpoints from newest to oldest.
**Verify:** Run `log` and verify that the checkpoints are shown from newest to oldest.

### SR-11 — Show checkpoint details
**SR-11 (source UR-GIT-07):** With our recorded checkpoints, when `log` is run, MiniGit shall show each checkpoint's ID and message.
**Verify:** Run `log` and verify that each checkpoint has an ID and message associated with it.

### SR-12 — Keep files after an error
**SR-12 (source UR-GIT-09):** Given existing project files, when an invalid command is run, MiniGit shall keep the existing files unchanged at all times
**Verify:** Run an invalid command and verify that the existing files are still unchanged.

## Test

### AT-01 — Initialize a project

**Tests:** SR-01
**Do:** Run `init` on a new project.
**Result:** Project initializes and files remain.

### AT-02 — Run init again

**Tests:** SR-02
**Do:** Run `init` again.
**Result:** Files stay unchanged.

### AT-03 — Add one file

**Tests:** SR-03
**Do:** Run `add notes.txt`.
**Result:** Only `notes.txt` is staged.

### AT-04 — Add a missing file

**Tests:** SR-04
**Do:** Run `add missing.txt`.
**Result:** Error appears and staged file remains.

### AT-05 — Show file status

**Tests:** SR-05
**Do:** Run `status`.
**Result:** Staged file is shown.

### AT-06 — Show file differences

**Tests:** SR-06
**Do:** Change a file and run `diff`.
**Result:** Changes are shown.

### AT-07 — Show staged differences

**Tests:** SR-07
**Do:** Stage a file and run `diff --staged`.
**Result:** Staged changes are shown.

### AT-08 — Create a checkpoint

**Tests:** SR-08
**Do:** Run `commit -m "First checkpoint"`.
**Result:** Checkpoint is created.

### AT-09 — Empty commit message

**Tests:** SR-09
**Do:** Run `commit -m ""`.
**Result:** Error appears and no checkpoint is created.

### AT-10 — View checkpoints

**Tests:** SR-10
**Do:** Run `log` with two checkpoints.
**Result:** Newest checkpoint appears first.

### AT-11 — Show checkpoint details

**Tests:** SR-11
**Do:** Run `log`.
**Result:** ID and message appear.

### AT-12 — Keep files after an error

**Tests:** SR-12
**Do:** Run an invalid command.
**Result:** Existing files remain unchanged.

## Verification Methods
### VM-01 — Initialize
**Verify:** SR-01  
**Method:** Run `init` and check the project files.

### VM-02 — Run init again
**Verify:** SR-02  
**Method:** Run `init` again and check that files are unchanged.

### VM-03 — Add one file
**Verify:** SR-03  
**Method:** Run `add notes.txt` and `status`.

### VM-04 — Missing file
**Verify:** SR-04  
**Method:** Run `add missing.txt` and check the error.

### VM-05 — File status
**Verify:** SR-05  
**Method:** Run `status` and check the output.

### VM-06 — File differences
**Verify:** SR-06  
**Method:** Run `diff` and check the output.

### VM-07 — Staged differences
**Verify:** SR-07  
**Method:** Run `diff --staged` and check the output.

### VM-08 — Create checkpoint
**Verify:** SR-08  
**Method:** Run `commit -m "message"` and `log`.

### VM-09 — Empty message
**Verify:** SR-09  
**Method:** Run `commit -m ""` and check the error.

### VM-10 — View checkpoints
**Verify:** SR-10  
**Method:** Run `log` and check the order.

### VM-11 — Checkpoint details
**Verify:** SR-11  
**Method:** Run `log` and check the ID and message.

### VM-12 — Files after error
**Verify:** SR-12  
**Method:** Run an invalid command and check the files.

## Traceability Matrix

| UR | SR | Acceptance Test | Verification Method |
|---|---|---|---|
| UR-GIT-01 | SR-01, SR-02 | AT-01, AT-02 | VM-01, VM-02 |
| UR-GIT-02 | SR-05 | AT-05 | VM-05 |
| UR-GIT-03 | SR-06, SR-07 | AT-06, AT-07 | VM-06, VM-07 |
| UR-GIT-04 | SR-08 | AT-08 | VM-08 |
| UR-GIT-05 | SR-03 | AT-03 | VM-03 |
| UR-GIT-06 | SR-10, SR-11 | AT-10, AT-11 | VM-10, VM-11 |
| UR-GIT-07 | SR-09, SR-12 | AT-09, AT-12 | VM-09, VM-12 |
| UR-GIT-08 | SR-04, SR-09 | AT-04, AT-09 | VM-04, VM-09 |
| UR-GIT-09 | SR-04, SR-09, SR-12 | AT-04, AT-09, AT-12 | VM-04, VM-09, VM-12 |



## Functional System Requirements
