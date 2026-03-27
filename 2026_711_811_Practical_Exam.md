## Gen711/811 Practical Exam 2026 (40 scaled points)
## NAME: MYNAME

Exam Instructions:
- Make sure to paste all the commands that you use below each of the tasks.
- Some tasks require you to copy your output here. All output that you paste is less than 10 lines.
- Do not spend too much time on a single prompt. It's important to get to all prompts.
- If you get stuck and do not know how to proceed: specifically ask for the answer.
- Help will get you to the next step, but you will lose points for that prompt.
- Questions on clarification of the question does not result in the loss of points.
- Replace MYNAME with your first and last name.
- Your exam must be pushed to your github before 5pm to get credit. You can push early, and push again at the end. 

Hints:
- Remember, if you want to re-run a command, but change it slightly to try to fix it, hit the up arrow and edit the last command that you ran.
- When typing out commands, filenames, or paths, hit the tab button a couple of times to see if it autocompletes for you.

---

1. Use an absolute path to change your current working directory to the 'prac_exam' directory that you just cloned (2 points). 
  
  
2. From 'prac_exam' directory, make the following directory structure in a single command: `data/untrimmed_fastq` (2 points, -1 point if you need to use 2 commands for this. Hint: There is a flag/option that lets you create nested directories all at once.)


3. Copy the two fastq files in the `/tmp/Gen711-811_data` directly into your `untrimmed_fastq` directory without changing your current directory. (2 points, partial credit if you need to change directories first. Multiple correct answers)
  
4. List all the hidden files in this repo. Paste the command below (2 points)

5. Use a relative path to change your current working directory to the `untrimmed_fastq` directory. (2 points)


6. These are paired-end FASTQ files from an *E. coli* long-term evolution experiment. To confirm the files look ok, view one of them and paste the top 4 lines below. (4 points, Hint: These files are gzip-compressed. Multiple correct answers)



7. How large (file size) are the two uncompressed fastq files? Use a single command with appropriate options to show the file sizes in a human-readable format (e.g., MB). Paste the command and output below. (2 points)



8. For each fastq, how many quality score lines have the '@' symbol in them? To answer this, use one line of piped bash commands for each fastq, and the output should be a single number.(2 points) 



9. How many reads have 15 or more uncalled bases (`NNNNNNNNNNNNNNN`) in `SRR2584863_1.fastq`? Count WITHOUT making a new file (4 points)



10. Make a single fasta file from the two fastqs using the reads found in the question above, and their respective info lines. Name the new file 'badreads.fasta' in the 'untrimmed_fastq' directory.  (4 points)

Hint: the first 4 lines of badreads.fasta should look similar (but maybe not exactly) to this:
```
@SRR2584863.1 HWI-ST957:244:H73TDADXX:1:1101:4712:2181/2
GGCGACATTACTGACCCGCNNNNNNNNNNNNNNNNNNNCGACNNNNNNNNNNNNNNNNNCCTGATNNNNNNNNNNNNNNNTCAGNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNN
@SRR2584863.2 HWI-ST957:244:H73TDADXX:1:1101:8571:2191/2
TCCCCGGAGTCAGCAGGGTGNNNNNNNNNNNNNNNNNATACATNNNNNNNNNNNNNNNGTTTTTGNNNNNNNNNNNNNNGCTGTCNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNN
```


11. Activate the conda 'genomics' environment that contains `fastqc` and confirm where `fastqc` is installed. Paste the command and its output below. (2 points)



12. Run `fastqc` on `SRR2584863_1.fastq`. Then, create a `results/fastqc_untrimmed_reads` directory and move both the `.zip` and `.html` output files into it — all without leaving your `untrimmed_fastq` directory. Paste all commands used. (4 points)



13. Without changing directories, what is the 100th line of the file `SRR2584863_1.trim.fastq` in your `trimmed_fastq` directory? (2 points)




14. Run `md5sum` on `SRR2584863_1.fastq`. Then run it again, redirecting the output to a new file called `my_md5sums.txt`. Next, run `md5sum` on `SRR2584863_2.fastq` and **append** it to `my_md5sums.txt`. Finally, append your name to the end of `my_md5sums.txt`. Paste all commands used. (4 points)


15. Push all of the new files that you created to your github repo using vscode's github side bar (4 points). 
---

