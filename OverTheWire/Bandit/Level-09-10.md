# Bandit Level 9 → Level 10

## Objective

Find the password inside `data.txt`. It is embedded among binary data and appears in one of the few human-readable strings, preceded by several consecutive `=` characters.

## Commands Used

### 1. List the files

```bash
ls
ls -a
```

Confirmed that `data.txt` was present in the home directory.

### 2. Inspect the file type

```bash
man file
file -a
```

The `file -a` command failed because `-a` is not a valid option for this command.

### 3. Extract human-readable strings

```bash
strings data.txt
```

The command extracted readable text sequences from the binary file. Among the output were several strings containing consecutive `=` characters.

### 4. Filter strings containing consecutive equals signs

```bash
strings data.txt | grep '=\{2,\}'
```

This pipeline:
- `strings data.txt` — extracts human-readable strings from the file.
- `|` — passes the extracted strings to `grep`.
- `grep` — searches text for lines matching a pattern.
- `=\{2,\}` — matches two or more consecutive `=` characters using basic regular expression syntax.

The output contained four matching strings. Reading them in order revealed the message containing the password.

### 5. Exit the level

```bash
exit
```

## Troubleshooting

- `file -a` — failed because `-a` is not a supported option.
- `cat data.txt | sort | uniq -c | string` — failed because the command is `strings`, not `string`.
- `cat data.txt | sort | uniq -c | strings` — unnecessary processing. The correct approach is to extract readable strings directly from the original file.
- `grep '=\{2.\}'` — failed because the regular expression syntax was incorrect.
- `grep '=\{2,\}'` — corrected the pattern to match two or more consecutive equals signs.

## Concepts Learned

- **`strings`:** Extracts printable character sequences from binary files.
- **`grep`:** Searches text using patterns.
- **Regular expressions:** Allow matching patterns such as repeated characters.
- **Pipes (`|`):** Connect the output of one command to another.
- **Binary files:** May contain readable text mixed with non-readable data.

## Result

Successfully extracted the readable strings, identified the message containing the password, and proceeded to the next level.
