# Texas Advanced Computing Center (TACC) Basics

This page provides a quick-start guide to using TACC systems (e.g., **Frontera**, **Stampede3**, **Lonestar6**) for astronomy and scientific computing. For a more in-depth look into the different architectures used in the different supercomputing clusters look <a href="https://docs.tacc.utexas.edu/" target="_blank" rel="noopener noreferrer">
  here.
</a>

---

## Logging In

TACC systems use SSH for access. You must have an active TACC account and an approved allocation.

```ssh username@SUPERCOMPUTER.tacc.utexas.edu```

## Copy a file to TACC

```scp myfile.txt username@SUPERCOMPUTER.tacc.utexas.edu:file/path/to/file.txt```

## Copy a directory from TACC

```bash 
rsync -avz username@stampede2.tacc.utexas.edu:/work/01234/username/folder_to_copy/ . 
```

## An Example Slurm File

```bash
#!/bin/bash
#SBATCH -J myjob            # Name of the job, when using squeue -u username 
                            # this job name is what pops up
#SBATCH -o myjob.out        # Standard output will be stored in this file
#SBATCH -e myjob.err        # All the errors will be stored here
#SBATCH -N 1                # The number of nodes requested
#SBATCH -n 48               # Total number of cores requested 
                            # This varies from queue to queue
#SBATCH -p normal           # The queue to run the job
                            # see your supercomputer documentation 
                            # for all the queues available 
#SBATCH -t 01:00:00         # Total time of your job to run
#SBATCH -A your_allocation  # Which allocation to charge the time
#SBATCH --mail-type=all 
#SBATCH --mail-user=your_email@email.com


module load python/3.10 #loading any neccessary tacc modules

#calling your script
python myscript.py
```

## Useful terminal commands on TACC

| Command           | Description                   |
| ----------------- | ----------------------------- |
| `squeue -u $USER` | See your queued/running jobs  |
| `scancel JOB_ID`  | Cancel a job                  |
| `showq -u $USER`  | Show job and node info        |
| `idev`            | Launch an interactive session |
| `module save`     | Save your loaded module set   |


## TACC Usage

- Use /scratch or /work for large files, not your home directory.
- Be mindful of file quotas and time limits.
- For multi-node jobs, make sure your code is designed to scale.
