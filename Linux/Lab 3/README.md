# 🧪 Lab 3: Editing Files with `vim` and `nano`

This lab covers how to install and use two essential text editors in Linux: **vim** and **nano**. You'll learn how to open, navigate, and edit files using both editors.

---

## 📦 Installing `vim` and `nano`

### On Amazon Linux 2

```bash
sudo yum update -y
sudo yum install vim nano -y
```

### On Amazon Linux 2023

```bash
sudo dnf update -y
sudo dnf install vim nano -y
```

### On Ubuntu

```bash
sudo apt update
sudo apt install vim nano -y
```

---

## 📝 Editing Files with `nano`

### Launch `nano`

```bash
nano filename.txt
```

### Basic Controls

| Action            | Key Combo      |
|-------------------|----------------|
| Save file         | `Ctrl + O`     |
| Exit editor       | `Ctrl + X`     |
| Cut line          | `Ctrl + K`     |
| Paste line        | `Ctrl + U`     |
| Search text       | `Ctrl + W`     |
| Move to line      | `Ctrl + _`     |

> 💡 Help menu is available at the bottom of the editor.

---

## ✍️ Editing Files with `vim`

### Launch `vim`

```bash
vim filename.txt
```

### Modes in `vim`

- **Normal Mode:** Default mode (for navigation)
- **Insert Mode:** For editing text (`i` to enter, `Esc` to leave)
- **Command Mode:** For saving/exiting (`:` commands)

### Basic Navigation and Commands

| Action                 | Command         |
|------------------------|-----------------|
| Enter Insert Mode      | `i`             |
| Save file              | `:w`            |
| Quit vim               | `:q`            |
| Save and Quit          | `:wq` or `ZZ`   |
| Quit without saving    | `:q!`           |
| Move cursor left/right | `h`, `l`        |
| Move cursor up/down    | `k`, `j`        |
| Search text            | `/keyword`      |
| Go to line number      | `:20`           |

---

## 🧪 Practice Tasks

1. Create and edit a file called `practice.txt` using both `nano` and `vim`.
2. Write the line: `Hello, this is a test file.`
3. Save and close the file.
4. Reopen the file and add a second line.
5. Search for the word "test" in both editors.

---

## 📌 Tips

- If you're new to `vim`, start with `nano` for quick edits.
- You can exit `vim` anytime by pressing `Esc` and typing `:q!` to quit without saving.

---

Happy editing! ✏️
