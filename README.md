# 🔍 What Is Git?
At its core, Git tracks changes to files and coordinates work among multiple developers. Unlike centralized systems, Git allows each developer to have a complete copy of the project history, enabling offline work and reducing reliance on a central server. This decentralized approach enhances collaboration and resilience.​

---

# 🧠 How Git Works: Snapshots, Not Deltas

Git operates by capturing snapshots of your project at specific points in time. Each commit represents a snapshot of the entire project, not just the differences from the previous version. If files haven't changed, Git doesn't store them again but references the existing files. This method ensures data integrity and efficiency.


---

## 🧠 **Git Behind the Scenes: How It Really Works**

Git is not just a bunch of commands — it's a **content-addressable versioned file system**. Here’s a breakdown of its **core concepts and inner mechanisms**:

---

### 🔹 1. **.git Directory – The Heart of Git**

Every Git repo has a hidden folder:

```bash
.git/
```

This contains all the actual data and metadata Git uses to manage your project. Key subfolders:

- `.git/objects/` → Stores all content (blobs, trees, commits)
- `.git/refs/` → Stores pointers (branches, tags)
- `.git/HEAD` → Points to the current branch/ref
- `.git/index` → Staging area (what’s about to be committed)
- `.git/config` → Local repo config settings

---

### 🔹 2. **Git Stores Snapshots, Not Diffs**

Most version control systems store **differences** between file versions.

**Git stores complete snapshots** of file trees at each commit — but it’s smart:
- If a file didn’t change, Git reuses the old one via hashing.

---

### 🔹 3. **Core Object Types in Git**

Everything in Git is stored as an object (compressed & hashed):

| Type    | Description                           |
|---------|---------------------------------------|
| **Blob**  | File content (no metadata)             |
| **Tree**  | Directory – holds blobs and other trees |
| **Commit**| Snapshot + metadata (author, message) |
| **Tag**   | Pointer to a specific commit or object |

Each object is stored in `.git/objects/` and is addressed by its **SHA-1 hash**.

---

### 🔹 4. **Hashing Is Key**

Git uses SHA-1 hashes for **content-based addressing**.

Example:
```bash
echo "hello" | git hash-object --stdin
```
This will give you a SHA-1 hash like `b6fc4c620b67d95f953a5c1c1230aaab5db5a1b0`.

Git uses this hash to store and find objects.

---

### 🔹 5. **The Index (Staging Area)**

- Stored in `.git/index`
- When you `git add`, files are added to the index.
- When you `git commit`, it uses the index to create a **tree** object and a new **commit** object.

---

### 🔹 6. **How a Commit Works**

```bash
git commit -m "Message"
```

What happens:
1. Git looks at `.git/index`
2. Builds a **tree** of all staged files
3. Creates a new **commit object**
4. Links to parent commit(s)
5. Stores it in `.git/objects/`
6. Updates `.git/refs/heads/branchname` with new commit hash

---

### 🔹 7. **Branches Are Just Pointers**

A branch is just a file in `.git/refs/heads/` containing the commit hash it points to.

For example:
```bash
cat .git/refs/heads/main
```
This gives you the SHA-1 of the latest commit in `main`.

---

### 🔹 8. **HEAD – The Tip of the Tree**

`.git/HEAD` points to the current branch you're working on:
```bash
ref: refs/heads/main
```

If you're in a detached HEAD state (e.g., checked out a commit directly), `.git/HEAD` will contain the SHA-1 hash directly.

---

### 🔹 9. **Git Garbage Collection**

Unreferenced commits and objects (like after a `rebase` or `reset`) aren’t deleted immediately.

You can force cleanup with:
```bash
git gc
```

Git periodically compresses loose objects into pack files for performance.

---

### 🔹 10. **Viewing Internals with Commands**

- View all objects:
  ```bash
  git cat-file -p <hash>
  ```

- Inspect commit history:
  ```bash
  git log --oneline --graph --all
  ```

- Show the structure of the tree:
  ```bash
  git ls-tree <commit hash>
  ```

---

