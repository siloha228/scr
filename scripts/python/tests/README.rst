========================================================
Testing scripts for SCR python scripts
========================================================

| The *test\*.py* scripts can each be ran independently  
| Scripts can mostly be ran without arguments  
| These scripts accept the following arguments:  
|  
| ``test_watchdog.py <launcher> <launcher args> <program> <program args>``  
| *ex: test_watchdog.py srun -N 2 -n 2 sleeper*  
|  ``test_launch.py <launcher> <launcher args> <program> <program args>``  
| *ex: test_launch.py srun -N 2 -n 2 test_api*  
| ``test_pdsh.py <launcher> <program> <program args>``  
| *ex: test_pdsh.py srun printer*  
| ``test_flush_file.py <scr prefix directory>``  
| *specify the prefix directory, otherwise scrpy/tests is used*  
|  
| The file *runtest.sh* will iterate through all test*.py scripts  
| *It can be ran in one of three ways:*  
|  
| ``./runtest.sh``  
|   do all test*.py scripts and the bottom portion of runtest.sh  
| ``./runtest.sh scripts``  
|   only do the test*.py scripts, following 1 run of the test_api  
| ``./runtest.sh <word>``  
|   use some other word to only do the bottom of runtest.sh  

========================================================
Usage instructions for runtest.sh for all use cases  
========================================================

| **These variables at the top of runtest.sh must be set:**  
|  
|   launcher="srun"  - options:{srun,jsrun,flux,aprun,mpirun,lrun}  
|   *you may verify the format of launcher args near the top of runtest.sh*  
|   numnodes="2"     - the number of nodes in the allocation  
|   MPICC="mpicc"    - the MPI C compiler to use for compiling test programs  

========================================================
Running runtest.sh within an interactive allocation  
========================================================

| **SLURM + srun**  
| ``salloc -N2 -ppdebug``  
| ``cd install/bin/scrpy/tests``  
| ``./runtest.sh``  
|  
| **SLURM + flux**  
| ``cd install/bin/scrpy/tests``  
| ``source fluxenv.sh``  
| ``salloc -N2 -ppdebug``  
| ``srun -N2 -n2 --pty flux start``  
| ``./runtest.sh``  
|  
| **LSF + jsrun**  
| ``bsub -q pdebug -nnodes 2 -Is /usr/bin/bash``  
| ``cd install/bin/scrpy/tests``  
| ``./runtest.sh``  

========================================================
Running runtest.sh within a batch script
========================================================

| **SLURM + flux**  
| *Prepare the environment*  
| ``cd install/bin/scrpy/tests``  
| ``source fluxenv.sh``  
| *Setup your submission script*  
| ``#!/usr/bin/bash``  
| ``#SBATCH -N, -J, -t, -p, -A, -o ...``  
| ``#SBATCH --export=ALL``  
| ``#SBATCH --wait-all-nodes=1``  
| ``srun -N 2 -n 2 flux start ./runtest.sh``  
| *With these steps completed, submit the script:*  
| ``sbatch submit.sh``  
|  
| **SLURM +srun**
| Same as above except with this execution command:  
| ``./runtest.sh``  
|  
