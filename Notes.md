# GEN711/811 Practical Command Reference


A handy reference for all commands, tools, and concepts covered in Labs 1–7.


---


## Table of Contents
1. [SSH & Connecting to RON](#1-ssh--connecting-to-ron)
2. [Git & GitHub](#2-git--github)
3. [Navigating the Filesystem](#3-navigating-the-filesystem)
4. [Viewing & Searching Files](#4-viewing--searching-files)
5. [Creating, Copying, Moving & Deleting](#5-creating-copying-moving--deleting)
6. [Wildcards & Patterns](#6-wildcards--patterns)
7. [Permissions](#7-permissions)
8. [History & Shortcuts](#8-history--shortcuts)
9. [Redirection & Pipes](#9-redirection--pipes)
10. [For Loops & Scripts](#10-for-loops--scripts)
11. [Conda Environments](#11-conda-environments)
12. [FastQC](#12-fastqc)
13. [Trimmomatic](#13-trimmomatic)
14. [Downloading Files](#14-downloading-files)
15. [Practical Exam Task Cheatsheet](#15-practical-exam-task-cheatsheet)


---


## 1. SSH & Connecting to RON


```bash
# Connect to RON
ssh username@ron.sr.unh.edu


# Fix a host key mismatch error (run this if you get a host key error)
ssh-keygen -R ron.sr.unh.edu


# Generate an SSH key pair (on RON, for GitHub auth)
ssh-keygen -t ed25519 -C "your.email@unh.edu"
# Press Enter for default location, Enter twice for no passphrase


# View your public key (to paste into GitHub)
cat ~/.ssh/id_ed25519.pub


# Test your GitHub SSH connection
ssh -T git@github.com


# Generate SSH keys on your LOCAL machine (to avoid typing RON password)
cd ~/.ssh
ssh-keygen -t ed25519 -b 4096
chmod 400 ~/.ssh/id_ed25519


# Copy your local public key to RON (run this on YOUR machine)
ssh-copy-id username@ron.sr.unh.edu
```


---


## 2. Git & GitHub


### Cloning & Setup
```bash
# Clone a repo into your home directory
git clone https://github.com/YOURUSERNAME/REPONAME.git


# Set your Git username and email (do this once on RON)
git config --global user.name "YOUR_GITHUB_USERNAME"
git config --global user.email "your.email@unh.edu"
```


### Switching to SSH authentication
```bash
# Check current remote URL
git remote -v


# Switch remote to SSH (so you don't need to type password every time)
git remote set-url origin git@github.com:YOURUSERNAME/REPOSITORY.git
```


### Syncing a forked repo with the original (upstream)
```bash
cd ~/gen711-811


# Add the original repo as 'upstream' (only need to do this once)
git remote add upstream https://github.com/jthmiller/gen711-811.git


# Fetch and merge new changes from the original repo
git fetch upstream
git merge upstream/master master


# Alternative: rebase instead of merge
git rebase upstream/master
```


### Everyday Git workflow
```bash
git fetch origin          # Check for remote changes
git rebase origin/main    # Apply remote changes on top of yours


# If there are conflicts during rebase:
git add .
git rebase --continue
```


---


## 3. Navigating the Filesystem


```bash
pwd                         # Print current working directory
ls                          # List contents of current directory
ls -l                       # Long format (shows permissions, size, date)
ls -lh                      # Long format with human-readable file sizes
ls -a                       # Show ALL files including hidden ones (starting with .)
ls -la                      # Long format + hidden files
ls /bin                     # List contents of a specific directory (absolute path)


cd directoryname            # Change into a directory
cd ..                       # Go up one directory level
cd ../..                    # Go up two directory levels
cd ~                        # Go to home directory
cd $HOME                    # Go to home directory (same as above)
cd -                        # Go back to the previous directory


# Tab completion: press Tab to autocomplete file/directory names
# Press Tab twice to see all matching options
```


### Absolute vs Relative Paths
```bash
# Absolute path: starts from root /
cd /home/users/username/gen711-811


# Relative path: starts from where you currently are
cd gen711-811/labs
cd ../../shell_data
```


---


## 4. Viewing & Searching Files


### Viewing file contents
```bash
cat filename.txt            # Print entire file to screen
head filename.txt           # Show first 10 lines
head -n 4 filename.txt      # Show first 4 lines
head -n 100 filename.txt    # Show first 100 lines
tail filename.txt           # Show last 10 lines
tail -n 4 filename.txt      # Show last 4 lines
less filename.txt           # View file interactively (press q to quit, space to scroll)
wc -l filename.txt          # Count number of lines
wc -c filename.txt          # Count number of characters/bytes
```


### Searching with grep
```bash
grep "PATTERN" filename.txt              # Search for pattern in file
grep -B 1 "PATTERN" filename.txt         # Show 1 line BEFORE each match
grep -A 1 "PATTERN" filename.txt         # Show 1 line AFTER each match
grep -B 1 -A 2 "PATTERN" filename.txt    # 1 before, 2 after
grep --no-group-separator -B 1 "PATTERN" filename.txt  # No -- separator between groups
grep -c "PATTERN" filename.txt           # Count matching lines
grep "PATTERN" file1.fastq file2.fastq  # Search multiple files


# Useful grep patterns for FASTQ files
grep "^@" file.fastq                     # Match lines starting with @  (headers)
grep -B 1 "NNNNNNNNNNNNNNN" file.fastq   # Find reads with 15+ Ns


# Search for a sequence and get the header line too
grep -B 1 "GNATNACCACTTCC" SRR098026.fastq --no-group-separator
```


### Finding files
```bash
ls /bin/c*           # List files in /bin starting with 'c'
ls /bin/*a*          # List files in /bin containing 'a'
ls /bin/*o           # List files in /bin ending with 'o'
ls *.fastq           # List all .fastq files in current directory
ls *.sh              # List all shell script files
```


---


## 5. Creating, Copying, Moving & Deleting


```bash
# Create directories
mkdir analysis                        # Make a new directory called 'analysis'
mkdir -p ~/gen711-811/data/untrimmed_fastq  # Make nested directories (with -p)


# Copy files
cp file.txt newfile.txt               # Copy a file
cp file.fastq backup/                 # Copy file into a directory
cp /tmp/gen711_2023/Sample1.fastq ~/analysis/  # Copy using absolute paths


# Move or rename files
mv file.txt newname.txt               # Rename a file
mv *.fastq backup/                    # Move all .fastq files to backup/
mv *.trim* ../trimmed_fastq/          # Move trimmed files to trimmed_fastq/


# Delete files and directories
rm file.txt                           # Delete a file
rm -r directoryname/                  # Delete a directory and its contents
rm -Rf backup                         # Force-delete backup directory (careful!)


# Make a file read-only
chmod a-w filename.txt                # Remove write permission for all users
```


### Basename — strip paths/extensions in scripts
```bash
basename SRR2584863_1.fastq.gz _1.fastq.gz   # Returns: SRR2584863
basename /path/to/file.txt                    # Returns: file.txt
```


---


## 6. Wildcards & Patterns


```bash
*           # Matches any number of any characters
?           # Matches exactly one character


ls *.fastq             # All files ending in .fastq
ls SRR*                # All files starting with SRR
ls *_1.fastq.gz        # All forward read files
ls *.{fastq,txt}       # All .fastq and .txt files


# What happens when a wildcard matches nothing?
ls *.fq                # Returns an error if no .fq files exist
```


---


## 7. Permissions


```bash
ls -l filename          # View permissions (shows rwxrwxrwx format)
```


### Permission format: `-rw-r--r--`
```
Position 1:    file type (- = file, d = directory)
Positions 2-4: owner permissions (r=read, w=write, x=execute, -=none)
Positions 5-7: group permissions
Positions 8-10: other users' permissions
```


```bash
# chmod: change file permissions
chmod a-w filename.fastq        # Remove write permission for all (make read-only)
chmod u+x script.sh             # Give owner execute permission
chmod ug+rwx filename.fastq     # Give owner and group read/write/execute
chmod 644 filename.txt          # Set permissions using numeric notation (owner=rw, others=r)
```


---


## 8. History & Shortcuts


```bash
history                   # View all recent commands
history | grep "ls"       # Search history for commands containing 'ls'
!123                      # Re-run command number 123 from history


# Keyboard shortcuts
↑ (up arrow)              # Scroll through previous commands
Tab                       # Autocomplete file/directory names
Tab Tab                   # Show all possible completions
Ctrl+C                    # Cancel a running command
Ctrl+A                    # Move cursor to beginning of line
Ctrl+E                    # Move cursor to end of line
```


---


## 9. Redirection & Pipes


```bash
command > file.txt         # Redirect output to file (OVERWRITES)
command >> file.txt        # Redirect output to file (APPENDS)
command_1 | command_2      # Pipe output of command 1 as input to command 2


# Examples
echo "Hello" > myfile.txt                   # Write to file
echo "World" >> myfile.txt                  # Append to file
cat myfile.txt | head -5                    # Show first 5 lines via pipe
grep "AAGTT" file.fastq | wc -l             # Count matching lines
wc -l *.fastq | sort                        # Count lines in all fastqs, then sort


# Count sequences in a FASTQ file (4 lines per read)
grep -c "^@" file.fastq
# OR
wc -l file.fastq   # then divide by 4


# Count reads with 3+ consecutive Ns
grep -c "NNN" SRR098026.fastq


# Convert FASTQ to FASTA (extract header + sequence lines)
grep -A 1 "^@SRR" file.fastq --no-group-separator > output.fasta


# md5sum: check file integrity
md5sum Sample1.fasta                          # Print checksum to screen
md5sum Sample1.fasta > my_md5sums.txt         # Save checksum to file
md5sum Sample2.fasta >> my_md5sums.txt        # Append second checksum
echo "Your Name" >> my_md5sums.txt            # Append your name
```


---


## 10. For Loops & Scripts


### Basic for loop syntax
```bash
for variable in list_of_items
do
  command $variable
done
```


### Common bioinformatics loop patterns
```bash
# Run fastqc on every fastq file
for fqname in *.fastq
do
  fastqc $fqname
done


# Print first 2 lines of each fastq file
for filename in *.fastq
do
  head -n 2 ${filename}
done >> ~/file.txt           # Append all output to one file


# Rename files using basename
for filename in *.fastq
do
  name=$(basename ${filename} .fastq)
  mv ${filename} ${name}_2026.txt
done


# Remove a suffix from all .txt files
for filename in *_2026.txt
do
  name=$(basename ${filename} _2026.txt)
  mv ${filename} ${name}.txt
done


# Trimmomatic loop for all paired-end samples
for infile in *_1.fastq.gz
do
  base=$(basename ${infile} _1.fastq.gz)
  trimmomatic PE ${infile} ${base}_2.fastq.gz \
               ${base}_1.trim.fastq.gz ${base}_1un.trim.fastq.gz \
               ${base}_2.trim.fastq.gz ${base}_2un.trim.fastq.gz \
               SLIDINGWINDOW:4:20 MINLEN:25 ILLUMINACLIP:NexteraPE-PE.fa:2:40:15
done
```


### Writing and running a script
```bash
# Create/open a script file
nano bad-reads-script.sh


# Inside the script:
#!/bin/bash
grep -B 1 "NNNNNNNNNN" SRR098026.fastq --no-group-separator > bad-reads.fastq
echo "Script finished!"


# Make script executable and run it
chmod +x bad-reads-script.sh
bash bad-reads-script.sh
```


---


## 11. Conda Environments


```bash
# Load/activate a conda environment
conda activate environmentname


# Deactivate the current environment
conda deactivate


# Find where a program is installed (after activating environment)
which fastqc
which trimmomatic


# List available environments
conda env list
```


---


## 12. FastQC


FastQC checks the quality of raw sequencing reads.


```bash
# Run FastQC on a single file
fastqc SRR097977.fastq


# Run FastQC on all fastq files in a directory
fastqc ~/gen711-811/data/untrimmed_fastq/*.fastq*


# Run FastQC on trimmed reads
fastqc ~/gen711-811/data/trimmed_fastq/*.fastq*


# Move FastQC output to results directory
mkdir -p ~/gen711-811/results/fastqc_untrimmed_reads
mv *.zip ~/gen711-811/results/fastqc_untrimmed_reads/
mv *.html ~/gen711-811/results/fastqc_untrimmed_reads/
```


### FastQC output files
| File | Description |
|------|-------------|
| `sample_fastqc.html` | Visual report — open in a browser |
| `sample_fastqc.zip` | Compressed folder of all outputs |
| `summary.txt` | Pass/Warn/Fail summary for each test |
| `fastqc_data.txt` | Raw data from each module |


### Quality score encoding (Phred33)
```
Quality encoding: !"#$%&'()*+,-./0123456789:;<=>?@ABCDEFGHIJ
                   |         |         |         |         |
Quality score:    01........11........21........31........41
```
Highest possible score = **41**


---


## 13. Trimmomatic


Trimmomatic filters low-quality reads and removes adapter sequences.


### Single-end example
```bash
trimmomatic SE input.fastq output.fastq SLIDINGWINDOW:4:20 MINLEN:25
```


### Paired-end example
```bash
trimmomatic PE SRR2589044_1.fastq.gz SRR2589044_2.fastq.gz \
              SRR2589044_1.trim.fastq.gz SRR2589044_1un.trim.fastq.gz \
              SRR2589044_2.trim.fastq.gz SRR2589044_2un.trim.fastq.gz \
              ILLUMINACLIP:NexteraPE-PE.fa:2:40:15 SLIDINGWINDOW:4:20 MINLEN:25
```


### Key Trimmomatic options
| Option | Meaning |
|--------|---------|
| `PE` / `SE` | Paired-end or single-end mode |
| `-threads 4` | Use 4 CPU threads (speeds things up) |
| `ILLUMINACLIP:adapters.fa:2:40:15` | Remove Illumina adapter sequences |
| `SLIDINGWINDOW:4:20` | Trim when avg quality in a 4-base window drops below Q20 |
| `LEADING:3` | Remove bases from start of read if quality < 3 |
| `TRAILING:3` | Remove bases from end of read if quality < 3 |
| `MINLEN:25` | Discard reads shorter than 25 bases after trimming |
| `CROP:100` | Cut read to 100 bases |
| `HEADCROP:10` | Remove first 10 bases of every read |


### Finding adapter files
```bash
# Use 'which' to locate trimmomatic, then find adapters nearby
which trimmomatic
ls $(dirname $(which trimmomatic))/../share/trimmomatic*/adapters/


# Available adapter files:
# NexteraPE-PE.fa   TruSeq2-SE.fa   TruSeq3-PE.fa
# TruSeq2-PE.fa     TruSeq3-PE-2.fa TruSeq3-SE.fa
```


### Compress/decompress files
```bash
gzip file.fastq           # Compress a file (creates file.fastq.gz)
gunzip file.fastq.gz      # Decompress a file
zcat file.fastq.gz | head # View compressed file without extracting
```


---


## 14. Downloading Files


```bash
# Download with wget
wget https://raw.githubusercontent.com/path/to/file.csv
wget ftp://ftp.ensemblgenomes.org/pub/release-37/bacteria/species_EnsemblBacteria.txt


# Download with curl (-O keeps the original filename)
curl -O ftp://ftp.sra.ebi.ac.uk/vol1/fastq/SRR258/004/SRR2589044/SRR2589044_1.fastq.gz
curl -O https://example.com/file.csv


# Download SRA FASTQ data (the 6 E. coli files used in labs)
curl -O ftp://ftp.sra.ebi.ac.uk/vol1/fastq/SRR258/004/SRR2589044/SRR2589044_1.fastq.gz
curl -O ftp://ftp.sra.ebi.ac.uk/vol1/fastq/SRR258/004/SRR2589044/SRR2589044_2.fastq.gz
curl -O ftp://ftp.sra.ebi.ac.uk/vol1/fastq/SRR258/003/SRR2584863/SRR2584863_1.fastq.gz
curl -O ftp://ftp.sra.ebi.ac.uk/vol1/fastq/SRR258/003/SRR2584863/SRR2584863_2.fastq.gz
curl -O ftp://ftp.sra.ebi.ac.uk/vol1/fastq/SRR258/006/SRR2584866/SRR2584866_1.fastq.gz
curl -O ftp://ftp.sra.ebi.ac.uk/vol1/fastq/SRR258/006/SRR2584866/SRR2584866_2.fastq.gz
```


---


## 15. Practical Exam Task Cheatsheet


Based on Practical-1.md these are the kinds of tasks you'll be asked to do:


### Task: Navigate and set up directories
```bash
cd $HOME                         # Start from home directory
mkdir analysis                   # Make the analysis directory
```


### Task: Copy files without changing directories
```bash
cp /tmp/gen711_2023/Sample1.fastq ~/analysis/
cp /tmp/gen711_2023/Sample2.fastq ~/analysis/
```


### Task: Navigate to a directory using absolute path
```bash
cd ~/analysis
# OR
cd /home/users/YOURUSERNAME/analysis
```


### Task: View the top 4 lines of a FASTQ file
```bash
head -n 4 Sample1.fastq
```


### Task: Convert FASTQ to FASTA (preview with pipe)
```bash
# FASTQ has 4 lines per read; FASTA only needs the header (@...) and sequence
grep -A 1 "^@SRR" Sample1.fastq --no-group-separator | head
```


### Task: Redirect FASTQ→FASTA to a new file
```bash
grep -A 1 "^@SRR" Sample1.fastq --no-group-separator > Sample1.fasta
grep -A 1 "^@SRR" Sample2.fastq --no-group-separator > Sample2.fasta
```


### Task: Count reads with 15+ consecutive Ns (no new file)
```bash
grep -c "NNNNNNNNNNNNNNN" Sample1.fastq
grep -c "NNNNNNNNNNNNNNN" Sample2.fastq
```


### Task: Make a directory and move files into it
```bash
mkdir to_blast
mv *.fasta to_blast/
```


### Task: List contents of a directory without changing into it
```bash
ls to_blast/
# OR using absolute path:
ls ~/analysis/to_blast/
```


### Task: Get a specific line number (e.g. line 100)
```bash
head -n 100 to_blast/Sample1.fasta | tail -n 1
```


### Task: Run md5sum and save/append to file
```bash
md5sum Sample1.fasta                      # Print to screen
md5sum Sample1.fasta > my_md5sums.txt     # Save to new file
md5sum Sample2.fasta >> my_md5sums.txt    # Append to file
echo "Firstname Lastname" >> my_md5sums.txt  # Append your name
```


### Task: Find reads with bad quality / many Ns
```bash
# Make a file of reads with 10+ Ns in a row
grep -B 1 "NNNNNNNNNN" SRR098026.fastq --no-group-separator > bad-reads.fastq
```


### Task: Count sequences in a FASTQ
```bash
# Method 1: count header lines
grep -c "^@" SRR098026.fastq


# Method 2: count all lines and divide by 4
wc -l SRR098026.fastq
```


### Task: Print file prefix of all .txt files
```bash
for filename in *.txt
do
  name=$(basename ${filename} .txt)
  echo $name
done
```


---


## Quick Reference Card


| Goal | Command |
|------|---------|
| Where am I? | `pwd` |
| What's here? | `ls` or `ls -lh` |
| Go home | `cd ~` or `cd $HOME` |
| Go up one level | `cd ../` |
| See hidden files | `ls -a` |
| View a file | `cat`, `less`, `head`, `tail` |
| Search a file | `grep "pattern" file` |
| Count lines | `wc -l file` |
| Copy a file | `cp source dest` |
| Move/rename | `mv source dest` |
| Delete a file | `rm file` |
| Delete a folder | `rm -r folder` |
| Make a folder | `mkdir foldername` |
| See permissions | `ls -l` |
| Change permissions | `chmod` |
| See command history | `history` |
| Where is a program? | `which programname` |
| Write output to file | `command > file` |
| Append to file | `command >> file` |
| Chain commands | `command1 \| command2` |
| Download a file | `wget URL` or `curl -O URL` |
| Check file integrity | `md5sum file` |
| Activate conda env | `conda activate envname` |
| Run FastQC | `fastqc file.fastq` |

