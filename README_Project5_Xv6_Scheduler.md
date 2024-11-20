
# Project 5 - Xv6: A New Scheduler

## Overview

This project involves modifying the basic Round Robin scheduler in Xv6 to support a simple 4-level multi-level feedback queue (MLFQ) while avoiding job starvation. You will use your existing Xv6 repository and make changes to implement the scheduler.

## Objectives

- Modify the `proc` structure to include fields for queue management and scheduling.
- Implement a scheduler with the following functionality:
  - Multi-level feedback queue with four levels.
  - Job starvation avoidance via priority boosting.
- Log scheduler output for testing and verification.

## Project Details

### Modifications to `proc` Structure
Add the following fields to the `proc` structure:
- `queue`: An integer value between 0 and 3, initialized to level 3.
- `remaining_iterations`: Number of iterations the process can run at the current queue level.
- `idle_iterations`: Number of iterations since the process last ran.

### Initialization
Initialize the above fields in the `allocproc()` function:
- `queue`: Set to 3.
- `remaining_iterations`: Set to 8.
- `idle_iterations`: Set to 0.

### Scheduler Updates
Modify the `scheduler()` function:
1. Adjust processes up or down queues based on `idle_iterations` or `remaining_iterations`.
2. Update the idle count for all processes.
3. Identify the highest queue with runnable processes.
4. Select and schedule the appropriate process:
   - Reset its idle count.
   - Decrement its remaining iterations.
   - Print scheduling information in the format:  
     `process [<name>:<pid>:Q<level>:<time remaining>ms:<idle time>ms idle] is running`.

### Avoid Starvation
Implement a priority boost for processes whose `idle_iterations` exceed the maximum iterations for their current level:
- Move the process up one queue level (maximum of 3).
- Reset `idle_iterations` to 0.
- Update `remaining_iterations` to the appropriate value for the new level.

### Testing
1. Copy `/usa/roosen/cisc361/stressproc.c` into your repository and add it as a user program.
2. Log the scheduler's output:
   - Use the `script stressproc.log` command to capture output.
   - Run `make qemu-nox` and execute `stressproc`.
   - Save and push `stressproc.log` to your repository.

### Extra Credit (20 Points)
Implement a more efficient version of the MLFQ scheduler by reducing the need to iterate through all processes on each timer interrupt. Document your solution in an `extracredit.txt` or `extracredit.md` file, explaining how your implementation improves efficiency.

---

## Submission Instructions

1. **Repository Tagging**  
   Tag your current repository commit before making changes:  
   ```bash
   git tag -a Project3 -m "Submitted Project 3"
   git push origin Project3
   ```
2. **Push Changes**  
   Add, commit, and push all modified files and logs to your GitLab repository.

3. **Files to Include**
   - All updated source files (`proc.h`, `proc.c`, etc.).
   - `stressproc.c` user program.
   - `stressproc.log` with scheduler output.
   - `extracredit.txt` or `extracredit.md` (for extra credit).

---

## Rubric

| **Criteria**                                  | **Points** |
|-----------------------------------------------|------------|
| Added fields to `proc` structure              | 15         |
| Fields correctly initialized in `allocproc`   | 15         |
| Highest queue identified by scheduler         | 15         |
| Idle count updated for all runnable processes | 15         |
| Process queues change correctly               | 15         |
| Appropriate process is scheduled              | 15         |
| Printed information includes queue data       | 15         |
| `stressproc` properly implemented             | 15         |
| `stressproc.log` shows scheduler working      | 30         |
| **Total**                                     | **150**    |

For extra credit, up to 20 additional points can be earned based on the efficiency of your solution. 

--- 

**Good luck with your implementation!**
