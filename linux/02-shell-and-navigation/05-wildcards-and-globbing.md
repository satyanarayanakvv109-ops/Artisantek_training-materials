# Wildcards and File Globbing

When working in the Linux terminal, you'll often need to perform actions on multiple files at once. Instead of typing out each file name individually, you can use **wildcards** (also known as **globbing**) to match patterns.

## What is Globbing?

Globbing is a shell feature that allows you to specify a set of filenames with a single pattern using wildcard characters. The shell expands these patterns into a list of matching filenames *before* passing them to the command.

## Common Wildcards

### 1. The Asterisk (`*`)
The asterisk matches **zero or more characters** of any kind. This is the most frequently used wildcard.

**Examples:**
- `ls *.txt` : Lists all files ending in `.txt`.
- `rm file*` : Removes any file starting with `file` (e.g., `file1`, `file_old.txt`, `file`).
- `cp /var/log/*.log ~` : Copies all `.log` files from `/var/log` to your home directory.

### 2. The Question Mark (`?`)
The question mark matches **exactly one single character**.

**Examples:**
- `ls file?.txt` : Matches `file1.txt` or `fileA.txt`, but *not* `file10.txt` (since `10` is two characters).
- `rm image_0?.jpg` : Matches `image_01.jpg` through `image_09.jpg`.

### 3. Square Brackets (`[]`)
Square brackets allow you to match **any one character from a specific set or range**.

**Examples:**
- `ls file[123].txt` : Matches `file1.txt`, `file2.txt`, or `file3.txt`.
- `rm image_[a-z].png` : Matches any lowercase letter, e.g., `image_a.png`, `image_b.png`.
- `cp file[0-9].txt temp/` : Copies files ending in a single digit before `.txt`.

You can also negate a set using `!` or `^` inside the brackets:
- `ls file[!0-9].txt` : Matches `fileA.txt` but *not* `file1.txt`.

## How the Shell Handles Globbing

It's important to understand that the **shell** (like `bash` or `zsh`) expands the wildcard, not the command itself. 

If you type `ls *.txt`:
1. The shell looks at `*.txt`.
2. It expands it into a list: `doc1.txt doc2.txt notes.txt`.
3. It runs the command: `ls doc1.txt doc2.txt notes.txt`.

If no files match the pattern, the shell usually passes the literal wildcard string to the command, which will likely result in a "No such file or directory" error.

---

### 💡 Key Takeaways
- **Wildcards** save time when managing multiple files.
- `*` matches any number of characters (including zero).
- `?` matches exactly one character.
- `[]` matches one character from a specified set or range.
- The **shell** expands these patterns before the command even runs.

[⬅️ Previous: Relative vs Absolute Path](./04-relative-vs-absolute-path.md)
