# The `find` Command: Using `-exec`

You can use `find` to search for files, but you can also use it to *do things* to the files it finds. This is what `-exec` does.

---

## 1. The `{}` Placeholder
Think of `{}` as a blank space. `find` will put the name of the file it found into that blank space.

```bash
# Print every .txt file on the screen
find . -name "*.txt" -exec cat {} \;
```

---

## 2. Ending the Command: `\;` vs `+`

Every `-exec` command **must** end with either `\;` or `+`. If you forget it, the command will crash.

### `\;` (One at a time)
If you use `\;`, the command runs **once for each file**.
- **Example:** If you find 100 files, it runs `rm` 100 times.
- **Speed:** Very slow.
```bash
find /var/log -name "*.log" -exec rm {} \;
```

### `+` (All at once)
If you use `+`, it groups all the files together and runs the command **only once**.
- **Example:** It runs `rm file1 file2 file3 ...` all in one go.
- **Speed:** Super fast! Always use this if you can.
```bash
find /var/log -name "*.log" -exec rm {} +
```

---

## 3. Why does `+` sometimes fail?

Because `+` dumps all the files at the *very end* of your command, it breaks commands that need a specific order.

For example, copying files:
```bash
# THIS FAILS ❌
find . -name "*.conf" -exec cp {} /backup/dir/ +
```
**Why?** It turns into: `cp /backup/dir/ file1 file2 file3`. The `cp` command gets confused because the destination folder must be at the *end*.

To fix it, use `\;` instead:
```bash
# THIS WORKS ✅
find . -name "*.conf" -exec cp {} /backup/dir/ \;
```

---

## Cheat Sheet

- **`-exec`** : Run a command on the files you found.
- **`{}`** : The file that was found.
- **`+`** : Fast! Groups files together. Use this normally.
- **`\;`** : Slow. One file at a time. Use only if `+` breaks your command.

---

**⬅️ Previous:** [Variables and Expansions](./06-variables-and-expansions.md) · **Next ➡️** [Data Streams — stdin, stdout, and stderr](../03-io-and-data-streams/01-data-streams.md)
