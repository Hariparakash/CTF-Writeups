# Bandit Level 11 → Level 12

## Objective

Decode the contents of `data.txt`. The file uses ROT13, a substitution technique that rotates every uppercase and lowercase English letter by 13 positions.

## Commands Used

### 1. List the files

```bash
ls
```

Confirmed that `data.txt` was present in the home directory.

### 2. Read the `tr` manual

```bash
man tr
```

Opened the manual page to understand how `tr` translates characters.

### 3. Attempt to use `tr`

```bash
tr data.txt
```

This failed because `tr` requires two character sets when performing translation: a set of characters to replace and the corresponding replacement characters.

### 4. Decode the ROT13 text

```bash
tr 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz' 'NOPQRSTUVWXYZABCDEFGHIJKLMnopqrstuvwxyzabcdefghijklm' < data.txt
```

This command:
- `tr` — translates characters from one set to another.
- First string — contains the original uppercase and lowercase alphabets.
- Second string — contains the corresponding letters rotated by 13 positions.
- `< data.txt` — redirects the contents of `data.txt` into the command as standard input.

The decoded output revealed the next level's password.

### 5. Exit the level

```bash
exit
```

## Troubleshooting

- `tr data.txt` — failed because `tr` requires two character sets for character translation. The file must be supplied through input redirection or another appropriate method.

## Concepts Learned

- **`tr`:** Translates or deletes characters from standard input.
- **Character translation:** Maps characters from one set to corresponding characters in another set.
- **Input redirection (`<`):** Supplies a file's contents as standard input to a command.
- **ROT13:** A substitution cipher that rotates each alphabetic character by 13 positions.
- **ROT13 symmetry:** Applying ROT13 twice restores the original text.

## Result

Successfully decoded the ROT13-encoded contents of `data.txt` and retrieved the next level's password.
