# Git Conflict Resolution

## Goal

The goal of this exercise is to **intentionally create a Git conflict** and demonstrate how to resolve it manually.

---

## Steps Taken

### 1. Created a Basic `index.html` File on `main`

```
<!DOCTYPE html>
<html>
  <body>
    <h2>HTML Forms</h2>
    <form action="/action_page.php">
      <label for="fname">First name:</label><br />
      <input type="text" id="fname" name="fname" value="John" /><br />
      <label for="lname">Last name:</label><br />
      <input type="text" id="lname" name="lname" value="Doe" /><br /><br />
      <input type="submit" value="Submit" />
    </form>

    <p>
      If you click the "Submit" button, the form-data will be sent to a page
      called "/action_page.php".
    </p>
  </body>
</html>
```

<img src="images/Screenshot 2025-04-17 at 17.45.42.png" alt="index.html screenshot">

### 2. Modified main Branch

- Edited the <h2> tag from:

```
<h2>HTML Forms</h2>
```

To:

```
<h2>HTML Forms for Beginners</h2>
```

- Saved, committed, and pushed the change to the remote **main** branch.

### 3. Created and Switched to a New Branch

```
git branch feature/change-header
git switch feature/change-header
```

<img src="images/Screenshot 2025-04-17 at 17.47.35.png" alt="feature branch screenshot">

### 4. Modified the Header Again on feature/change-header

- Changed the <h2> tag to:

```
<h2>HTML Forms for Advance</h2>
```

- Saved the file.

### 5. Switched Back to **main** and Merged It into the Feature Branch

```
git switch main
git merge feature/change-header
```

### 6. Encountered a Merge Conflict

- Git highlighted a conflict in the index.html file.
- Conflict looked something like:

```
<<<<<<< HEAD
<h2>HTML Forms for Beginners</h2>
=======
<h2>HTML Forms for Advance</h2>
>>>>>>> feature/change-header
```

<img src="images/Screenshot 2025-04-17 at 17.50.19.png" alt="conflick screenshot">

## Conflict Resolution

- I manually edited the conflicted file.
- Removed the main branch’s version (HTML Forms for Beginners) and kept the feature branch’s version:

```
<h2>HTML Forms for Advance</h2>
```

<img src="images/Screenshot 2025-04-17 at 17.50.52.png" alt="conflict resolution screenshot">

- After resolving:

```
git add index.html
git commit -m "Resolved merge conflict in index.html"
git push origin main
```

## Result

- The conflict was successfully resolved.

- The final version of index.html reflects the changes from the feature/change-header branch.

- The resolution is now part of the main branch history.

## Conclusion

Merge conflicts are common when collaborating. The key is understanding what each branch is trying to do and making a decision that aligns with your project’s goal.
