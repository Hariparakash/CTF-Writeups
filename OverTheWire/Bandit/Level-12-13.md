# Bandit Level 12 → Level 13

## Objective

Recover the password from `data.txt`. The file is a hexadecimal dump of data that has been compressed and archived multiple times.

## Commands Used

### 1. Create a temporary working directory

```bash
mktemp -d
cp ~/data.txt /tmp/tmp.iPLyU1o0Ua
cd /tmp/tmp.iPLyU1o0Ua
ls
```

A temporary directory keeps the extraction work separate from the original file.

### 2. Convert the hexadecimal dump back into binary data

```bash
file data.txt
xxd data.txt
xxd -r data.txt > reversed_data
file reversed_data
```

`file` identified the original file as ASCII text. Inspecting it with `xxd` showed the hexadecimal dump.

`xxd -r` reversed the dump and reconstructed the original binary data. The resulting file was identified as gzip-compressed data.

### 3. Decompress the gzip and bzip2 layers

```bash
mv reversed_data reversed_data.gz
gzip -d reversed_data.gz
file reversed_data

bzip2 -d reversed_data
file reversed_data.out

mv reversed_data.out reversed_data.gz
gzip -d reversed_data.gz
file reversed_data
```

The file passed through these formats:

- Gzip-compressed data
- Bzip2-compressed data
- Gzip-compressed data again

The `mv` commands gave compressed files the extensions expected by the decompression utilities.

### 4. Extract the tar archives

```bash
tar --extract -f reversed_data
ls
file data5.bin

tar --extract -f data5.bin
ls
file data6.bin

bzip2 -d data6.bin
file data6.bin.out

tar --extract -f data6.bin.out
ls
file data8.bin
```

This revealed another sequence:

- A tar archive containing `data5.bin`
- Another tar archive containing `data6.bin`
- A bzip2-compressed file that decompressed into a tar archive
- A final tar extraction that produced `data8.bin`

### 5. Decompress the final gzip layer and read the result

```bash
mv data8.bin data8.bin.gz
gzip -d data8.bin.gz
file data8.bin
cat data8.bin
```

The final file was identified as ASCII text. Reading it with `cat` revealed the password.

### 6. Exit

```bash
exit
```

## Troubleshooting

- `gzip -d reversed_data` failed because the filename did not have a recognized gzip extension. Renaming it to `reversed_data.gz` allowed decompression.
- `bzip2 -d reversed_data` produced `reversed_data.out` because the utility could not determine the original filename.
- `tar -x reversed_data` and `tar --extract reversed_data` failed because the archive file was not specified using the `-f` option.
- `tar -extract reversed_data` failed because `-extract` was not valid option syntax. The correct long option is `--extract`.
- `xxd -r data8.bin > rev_data8.bin` produced an empty file because `data8.bin` was ordinary ASCII text, not a hexadecimal dump. Reading it with `cat` was the correct approach.

## Concepts Learned

- **`mktemp -d`:** Creates a temporary directory.
- **`cp`:** Copies files.
- **`mv`:** Renames or moves files.
- **`file`:** Identifies file types.
- **`xxd`:** Displays hexadecimal dumps and can reverse them with `-r`.
- **`gzip` / `bzip2`:** Decompress files.
- **`tar`:** Extracts archived files.
- **`cat`:** Displays text file contents.
- **File extensions:** Some utilities rely on filename extensions to determine output filenames or recognize compressed files.

## Result

Successfully reconstructed the binary data, identified and unpacked the nested compression and archive layers, and retrieved the password from the final text file.
