# Shell Variables and Expansions

Working with variables in the Linux shell can be confusing at first. Sometimes you use a `$` sign, sometimes you don't. Sometimes there are parentheses `()`, and sometimes there are double parentheses `(())`. 

Let's break down exactly how the shell handles variables and expansions.

---

## 1. When to use the Dollar Sign (`$`)

The golden rule for standard variables in `bash` is:
- **NO dollar sign** when *creating* or *modifying* a variable.
- **USE a dollar sign** when *reading* or *referencing* the variable.

### Setting a Variable (No `$`)
```bash
# Correct ✅ (No spaces around the equals sign!)
NAME="Alice"
AGE=25

# Incorrect ❌
$NAME="Alice"
NAME = "Alice"
```

### Reading a Variable (Use `$`)
```bash
# Correct ✅
echo "Hello, my name is $NAME and I am $AGE years old."
# Output: Hello, my name is Alice and I am 25 years old.

# Incorrect ❌ (Prints the literal word "NAME")
echo "Hello, my name is NAME"
```

> 💡 **Why does it work this way?** When you use `$`, you are telling the shell to **expand** the variable into its actual value before running the command.

---

## 2. Command Substitution: `$(command)` vs `` `command` ``

Often, you want to run a command and save its output into a variable. This is called **Command Substitution**.

### The Modern Way: `$(...)`
To capture the output of a command, wrap it in `$()`. 

```bash
# Get the current date and save it to a variable
CURRENT_DATE=$(date +%Y-%m-%d)

echo "Today is $CURRENT_DATE"
# Output: Today is 2023-10-25
```

### The Old Way: Backticks `` `...` ``
You will often see older scripts using backticks (`` ` ``) instead of `$()`. 

```bash
CURRENT_DATE=`date +%Y-%m-%d`
```

**Which should you use?**
Always use the modern `$(...)` syntax. Backticks are visually confusing (they look too much like single quotes `' '`) and they are very difficult to nest (putting one command substitution inside another). `$()` is much cleaner and is the undisputed standard today.

---

## 3. Arithmetic Expansion: `$((...))`

By default, the shell treats everything as text (strings). If you try to add two numbers normally, it just glues the text together:

```bash
# String concatenation, NOT addition!
RESULT=5+5
echo $RESULT
# Output: 5+5
```

To tell the shell "Please do math here", you use **Arithmetic Expansion**, which is denoted by **double parentheses**: `$((...))`.

```bash
# Correct ✅
RESULT=$((5 + 5))
echo $RESULT
# Output: 10
```

Inside the `$(( ))`, you can reference variables *without* the `$` sign. Using the `$` sign is also allowed for simple variables, but omitting it is generally cleaner.

```bash
X=10
Y=20

# Both of these work, but the first one is preferred:
TOTAL=$((X + Y))
TOTAL=$(($X + $Y))
```

### The C-Style `for` Loop Trap

There is one critical scenario where using a `$` inside double parentheses will **break your code**: the C-style `for` loop `(( ... ))`.

When using double parentheses for loop logic, the shell evaluates it as a raw arithmetic statement. If you use a `$` when trying to assign or increment a variable, the shell expands it *before* evaluating, causing an error.

```bash
# Correct ✅ (No $ on the loop variable 'i')
for (( i=1; i<=3; i++ ))
do
    echo "Count is $i"
done

# Incorrect ❌ 
for (( $i=1; $i<=3; $i++ ))
```

**Why does the incorrect one fail?**
If `$i` happens to be `1`, the shell expands `$i++` into `1++`. You can't increment the literal number 1! But `i++` tells the shell to increment the *variable* `i`. 

> 💡 **Rule of thumb for math and loops:** When you are inside double parentheses `(( ))` or `$(( ))`, just **drop the `$`**.

---

## Summary of Syntax

If you get confused, refer to this cheat sheet:

| Syntax | Name | What it does | Example |
|--------|------|--------------|---------|
| `VAR=val` | **Assignment** | Sets a variable (No spaces, No `$`) | `OS="Linux"` |
| `$VAR` | **Variable Expansion** | Reads the value of a variable | `echo $OS` |
| `$(cmd)` | **Command Substitution** | Runs a command and captures the output | `USER=$(whoami)` |
| `$((1+1))` | **Arithmetic Expansion**| Evaluates math expressions | `AGE=$((20 + 5))` |

---

**⬅️ Previous:** [Wildcards and File Globbing](./05-wildcards-and-globbing.md) · **Next ➡️** [Mastering the Find Command](./07-mastering-find.md)
