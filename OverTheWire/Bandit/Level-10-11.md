# Bandit Level 10 → Level 11

## Objective

Decode the Base64-encoded content stored in `data.txt` to reveal the next level's password.

## Commands Used

### 1. Read the Base64 manual

```bash
man based64
```

This failed because the command name was misspelled.

```bash
man base64
```

Opened the correct manual page to learn about the command and its options.

### 2. Decode the file

```bash
base64 -d data.txt
```

This command:
- `base64` — encodes or decodes data using Base64.
- `-d` — enables decoding mode.
- `data.txt` — specifies the file containing the encoded data.

The decoded output contained a message followed by the next level's password.

### 3. Exit the level

```bash
exit
```

## Troubleshooting

- `man based64` — failed because the command name was misspelled. The correct command is `base64`.

## Concepts Learned

- **Base64 encoding:** Converts binary data into a text-based representation using a defined character set.
- **`base64`:** Linux utility for encoding and decoding Base64 data.
- **`-d`:** Option used to decode Base64 input.
- **Manual pages (`man`):** Help documentation for understanding Linux commands and their options.

## Result

Successfully decoded the contents of `data.txt` and retrieved the next level's password.
