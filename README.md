# Git Hooks Setup
## **What Are Git Hooks?**
Git hooks are scripts that Git runs automatically when certain actions are performed — like committing or pushing code. They allow us to automate checks and rules before code gets added into the main project.

Think of Git hooks as local guards that help you or your team avoid pushing broken, insecure code.

### **Where Are Git Hooks Located?**
Once you initialize Git in a project using:

```
git init
```

A hidden .git directory is automatically created in your root folder. Inside that folder is a hooks directory where all hook scripts live.

You can check it by running:

```
ls .git
```

This will list folders and files like:

<img width="1367" alt="Screenshot 2025-04-15 at 15 37 21" src="https://github.com/user-attachments/assets/44582bb3-b459-46fc-bf5d-663b140d239c" />

Now move into the hooks directory:

```
cd .git/hooks
ls
```

You'll see default sample files like:

<img width="1360" alt="Screenshot 2025-04-15 at 15 38 49" src="https://github.com/user-attachments/assets/3d67b647-c8d4-4224-86dc-2c6c888791e0" />

### **These are templates. To use them:**

- Rename or create new files without the .sample extension (e.g., pre-commit)

- Make them executable using chmod +x

- Then write your script inside

## What This Setup Does
We created two Git hooks to improve code quality:

- pre-commit — Blocks bad code before it’s committed

- commit-msg — Enforces good commit message format

### Let’s break them down 

### **1. pre-commit Hook**

```
#!/bin/sh

echo "Running pre-commit checks..."

# Scan for console.log, debugger, or TODO
if git diff --cached --name-only | grep -E '\.js$' > /dev/null; then
  FILES=$(git diff --cached --name-only | grep -E '\.js$')
  
  for FILE in $FILES; do
    if grep -qE "console\.log|debugger|TODO" "$FILE"; then
      echo "❌ Commit rejected! Found 'console.log', 'debugger', or 'TODO' in $FILE"
      exit 1
    fi
  done
fi

echo "All checks passed."
exit 0
```
What it checks:

Before code is committed, this script scans any JavaScript files you're about to commit. It blocks the commit if it finds:

- console.log

- debugger

- TODO comments

Why we use it:

These are often left in by mistake during development. Blocking them early helps us:

- Keep our codebase clean

- Avoid pushing insecure or debug code to production

**How it works:**

When you run:

```
git commit
```

Git will trigger this hook and run the script first. If it finds anything bad, it cancels the commit and shows an error message.

**📂 File path:**

```
.git/hooks/pre-commit
```
**Example**
Good (commit allowed):
```
function greet() {
 return greeting;
}
```
<img width="1194" alt="Screenshot 2025-04-15 at 15 46 26" src="https://github.com/user-attachments/assets/aba7a3f8-8f14-4aff-9626-4c8adb972b37" />


**Example:**

Bad (commit blocked):
```
// script.js
console.log("Checking something");
```

<img width="1226" alt="Screenshot 2025-04-15 at 15 48 23" src="https://github.com/user-attachments/assets/8bdf6640-9627-4d80-96bb-cf68cc6e4ded" />

### **How to Bypass the Hook (if needed)**
Sometimes, you might want to skip a hook temporarily — maybe you’re in a hurry or the hook is being debugged.

To do that, use:

```
git commit -m "your message" --no-verify
```

This will skip all hooks, including pre-commit and commit-msg, for that commit only.

<img width="1058" alt="Screenshot 2025-04-15 at 15 56 24" src="https://github.com/user-attachments/assets/5fa81903-313c-44f8-be79-655ddf6e88c2" />

### **2. commit-msg Hook**
**What it checks:**
It ensures commit messages follow a consistent structure:

```
type(scope): message
```

Examples:

- ✅ feat(auth): add login

- ✅ fix(ui): correct layout

- ❌ final update

- ❌ made changes

Allowed types: feat, fix, chore, style, docs, test, refactor, perf

**Why we use it:**
Helps keep your commit history clean and easy to understand — which matters a lot when collaborating with others.

**How it works:**
This runs automatically when you commit:

```
git commit -m "your message"
```

If the message is wrong, it cancels the commit.

**📂 File path:**

```
.git/hooks/prepare-commit-msg
```

```
#!/bin/bash
# Get commit message from argument
COMMIT_MSG_FILE=$1
COMMIT_MSG=$(cat "$COMMIT_MSG_FILE")

# Regex pattern to match type(scope): message
PATTERN='^(feat|fix|chore|docs|style|refactor|test|perf)\([a-zA-Z0-9_-]+\): .+'

if ! echo "$COMMIT_MSG" | grep -Eq "$PATTERN"; then
  echo "❌ Commit message format is invalid."
  echo "➡️  Expected format: type(scope): message"
  echo "   Example: feat(auth): add login validation"
  exit 1
fi

echo "✅ Commit message format is valid."
```

**Example:**

```
git commit -m "added a new file"
# ❌ Commit blocked: invalid message format
```
<img width="941" alt="Screenshot 2025-04-15 at 16 08 47" src="https://github.com/user-attachments/assets/030457a8-7a12-4818-bb00-77e26319100b" />

```
git commit -m "chore(setup): add git hooks"
# ✅ Commit accepted
```
<img width="987" alt="Screenshot 2025-04-15 at 16 10 41" src="https://github.com/user-attachments/assets/79864fe2-63a9-4f6e-a185-5742cd4c46f6" />

### **How to Bypass This Hook**
Just like the pre-commit, if you ever want to skip this check:

```
git commit -m "some message" --no-verify
```

**This is handy when:**

- You're doing hotfixes in a rush

- You’re debugging the hook script

- The rule isn’t relevant at the moment (though use with caution)

### **How to Add & Use These Hooks**
1. Navigate to your project folder

2. Initialize Git (if not already done):

```
git init
```
3. Go into the hooks directory:

```
cd .git/hooks
```

4. Create the pre-commit and commit-msg files (remove the .sample if you're using those)

5. Paste your shell scripts into each file

6. Make them executable:

```
chmod +x pre-commit
chmod +x commit-msg
```

7. Test them by committing code!

### **Test Summary**
- Add console.log to any JS file → Commit should be blocked

- Write a bad commit message → Commit should be blocked

- Fix issues → Commit should go through

- Use --no-verify → Commit bypasses hooks



