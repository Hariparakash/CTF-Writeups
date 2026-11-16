# Bandit Level 8 → Level 9

## Objective

Find the only line in `data.txt` that appears exactly once. All other lines occur multiple times.

## Commands Used

### 1. List the files

```bash
ls
```

Confirmed that `data.txt` was present.

### 2. Inspect the file

```bash
cat data.txt
```

The file contained many lines of text, making it difficult to identify the unique line manually.

### 3. Sort and count occurrences

```bash
cat data.txt | sort | uniq -c
```

This command:
- `cat data.txt` — outputs the file contents.
- `|` — passes the output to the next command.
- `sort` — sorts the lines alphabetically, placing identical lines together.
- `uniq -c` — counts consecutive identical lines and displays each line with its occurrence count.

The output showed that most lines occurred 10 times, while one line occurred only once.

### 4. Exit the level

```bash
exit
```

## Troubleshooting

- `sort uniq data.txt` — incorrect syntax. `sort` treated `uniq` as a filename rather than another command.
- `cat -sort -uniq data.txt` — incorrect because `cat` does not provide sorting or duplicate-counting options.
- `cat data.tx | sort | uniq` — failed because the filename was misspelled. The correct filename is `data.txt`.
- `cat data.txt | sort | uniq` — sorted and removed consecutive duplicates, but did not show how many times each line appeared. Using `uniq -c` made the occurrence counts visible.

## Concepts Learned

- **Pipes (`|`):** Pass the output of one command as input to another.
- **`sort`:** Sorts lines of text.
- **`uniq`:** Identifies consecutive duplicate lines.
- **`uniq -c`:** Displays the number of occurrences of each consecutive line.
- **Command chaining:** Combining multiple Linux commands into one processing pipeline.

## Improved Command

An alternative that directly displays lines occurring exactly once is:

```bash
sort data.txt | uniq -u
```

This is an additional approach, not the command used in my original terminal session.

## Result

Successfully identified the only line occurring once and retrieved the next level's password.
