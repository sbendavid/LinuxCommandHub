# Part 5: Text Editors (Vim & Emacs)

[← Back to Part 4](./04-io-text-processing.md) | [Main Guide](../README.md) | [Next: Users & Processes →](./06-users-permissions-processes.md)

---

## Table of Contents

- [Vim Basics](#vim-basics)
- [Vim Navigation](#vim-navigation)
- [Vim Editing](#vim-editing)
- [Vim Saving and Exiting](#vim-saving-and-exiting)
- [Emacs Basics](#emacs-basics)
- [Emacs Editing](#emacs-editing)

---

## Vim Basics

**What is Vim:** A powerful, modal text editor ubiquitous in Unix-like systems.

**Start Vim:**

```bash
vim filename
```

### Understanding Modes

Vim has different modes for different tasks:

**1. Normal Mode (default)**

- Navigate and execute commands
- Press `Esc` to return here from any mode

**2. Insert Mode**

- Actually type text
- Press `i` to enter

**3. Visual Mode**

- Select text
- Press `v` to enter

**4. Command Mode**

- Execute commands like save, quit
- Press `:` to enter

---

### First Steps in Vim

**Open a file:**

```bash
vim myfile.txt
```

**Enter Insert mode:**

```
i
```

**Type some text:**

```
Hello, this is my first Vim file!
```

**Return to Normal mode:**

```
Esc
```

**Save and quit:**

```
:wq
```

**Congratulations!** You just used Vim.

---

### Vim Search Patterns

**Search forward:**

```
/pattern
```

Press `n` for next match, `N` for previous.

**Search backward:**

```
?pattern
```

**Example file:**

```
My pretty file is very pretty.
```

**Search for "pretty":**

```
/pretty
```

Vim highlights all matches. Press `n` to jump to next.

---

## Vim Navigation

**Basic movement (in Normal mode):**

```
h - Left
j - Down
k - Up
l - Right
```

**Or use arrow keys.**

**Word movement:**

```
w - Next word
b - Previous word
e - End of word
```

**Line movement:**

```
0 - Start of line
$ - End of line
^ - First non-blank character
```

**Screen movement:**

```
H - Top of screen
M - Middle of screen
L - Bottom of screen
```

**File movement:**

```
gg - Start of file
G  - End of file
50G - Go to line 50
```

**Page movement:**

```
Ctrl+f - Page forward
Ctrl+b - Page backward
Ctrl+d - Half page down
Ctrl+u - Half page up
```

---

## Vim Editing

### Entering Insert Mode

**Different ways to enter Insert mode:**

```
i - Insert before cursor
a - Append after cursor
I - Insert at beginning of line
A - Append at end of line
o - Open new line below
O - Open new line above
```

**Example:**

```
Current line|
```

Press `o`:

```
Current line
|
```

---

### Deleting Text

**In Normal mode:**

```
x - Delete character under cursor
dw - Delete word
dd - Delete line
d$ - Delete to end of line
D - Delete to end of line (same as d$)
```

**With counts:**

```
3dd - Delete 3 lines
2dw - Delete 2 words
```

---

### Changing Text

**Change commands delete AND enter Insert mode:**

```
cw - Change word
c$ - Change to end of line
cc - Change entire line
C - Change to end of line (same as c$)
```

**Example:**

```
The quick brown fox
```

Put cursor on "quick", press `cw`, type "slow":

```
The slow brown fox
```

---

### Copying and Pasting

**Yank (copy):**

```
yw - Yank word
yy - Yank line
y$ - Yank to end of line
```

**Put (paste):**

```
p - Paste after cursor/below line
P - Paste before cursor/above line
```

**Example workflow:**

```
1. Move to line you want to copy
2. Press yy
3. Move to destination
4. Press p
```

---

### Other Useful Edits

**Replace single character:**

```
r{char} - Replace with {char}
```

Example: On 'cat', press `rd` → 'dat'

**Replace mode:**

```
R - Enter Replace mode (overwrite text)
```

**Join lines:**

```
J - Join current line with next
```

**Repeat last change:**

```
. (dot)
```

This is incredibly powerful! Make a change once, repeat with `.`

---

### Visual Mode

**Enter Visual mode:**

```
v - Character-wise visual
V - Line-wise visual
Ctrl+v - Block-wise visual
```

**Once in Visual mode:**

1. Move to select text
2. Press `d` to delete, `y` to yank, `c` to change

**Example:**

```
1. Press V (line visual)
2. Press jjj (select 4 lines)
3. Press d (delete all 4 lines)
```

---

## Vim Saving and Exiting

**In Normal mode, press `:` then:**

```
:w          - Write (save) file
:q          - Quit Vim
:wq         - Write and quit
:q!         - Quit without saving (force)
:x          - Write and quit (only if changes)
ZZ          - Write and quit (shortcut)
```

**Saving with new name:**

```
:w newfile.txt
```

**Saving part of file:**

```
:5,10w partial.txt
```

Saves lines 5-10.

---

### Undo and Redo

```
u       - Undo last change
Ctrl+r  - Redo
```

**Undo multiple times:**

```
5u - Undo last 5 changes
```

---

### Vim Cheat Sheet

```
# Modes
Esc     - Normal mode
i       - Insert mode
:       - Command mode
v       - Visual mode

# Navigation
hjkl    - Left, down, up, right
w/b     - Next/previous word
0/$     - Start/end of line
gg/G    - Start/end of file

# Editing
dd      - Delete line
yy      - Copy line
p       - Paste
u       - Undo
.       - Repeat

# Save/Exit
:w      - Save
:q      - Quit
:wq     - Save and quit
:q!     - Quit without saving
```

---

### Vim Tips

**Tip 1:** Practice with `vimtutor`

```bash
vimtutor
```

**Tip 2:** Create a .vimrc for custom settings

```bash
vim ~/.vimrc
```

Add:

```vim
set number          " Show line numbers
set autoindent      " Auto indent
syntax on           " Syntax highlighting
```

**Tip 3:** The best way to learn Vim is to use it daily!

---

## Emacs Basics

**What is Emacs:** A highly customizable, extensible text editor.

**Start Emacs:**

```bash
emacs filename
```

### Key Notation

```
C-x  = Hold Ctrl and press x
M-x  = Hold Alt (Meta) and press x
```

**Common keys:**

- `Ctrl` = C-
- `Alt` = M- (Meta)
- `Shift` = S-

---

### File Operations

**Open file:**

```
C-x C-f
```

Then type filename and press Enter.

**Save file:**

```
C-x C-s
```

**Save as:**

```
C-x C-w
```

**Save all buffers:**

```
C-x s
```

---

### Buffer Management

**What are buffers:** Open files in Emacs.

**Switch buffer:**

```
C-x b
```

**List all buffers:**

```
C-x C-b
```

**Close buffer:**

```
C-x k
```

**Cycle through buffers:**

```
C-x → (right arrow)
C-x ← (left arrow)
```

---

### Window Management

**Split horizontally:**

```
C-x 2
```

**Split vertically:**

```
C-x 3
```

**Switch to other window:**

```
C-x o
```

**Close other windows:**

```
C-x 1
```

**Close current window:**

```
C-x 0
```

---

## Emacs Editing

### Navigation

**Basic movement:**

```
C-f  - Forward one character
C-b  - Backward one character
C-n  - Next line
C-p  - Previous line
```

**Word movement:**

```
M-f  - Forward one word
M-b  - Backward one word
```

**Line movement:**

```
C-a  - Beginning of line
C-e  - End of line
```

**Paragraph movement:**

```
M-}  - Forward paragraph
M-{  - Backward paragraph
```

**File movement:**

```
M-<  - Beginning of buffer
M->  - End of buffer
```

**Page movement:**

```
C-v  - Page down
M-v  - Page up
```

---

### Editing Text

**Delete:**

```
C-d      - Delete character forward
Backspace - Delete character backward
M-d      - Delete word forward
M-Backspace - Delete word backward
C-k      - Kill to end of line
```

**Kill and Yank (Cut and Paste):**

```
C-w  - Kill (cut) region
M-w  - Copy region
C-y  - Yank (paste)
```

**Marking regions:**

```
C-Space  - Set mark
```

Then move cursor to select, and use `C-w` to cut.

---

### Search and Replace

**Search forward:**

```
C-s
```

Type search term, press `C-s` again for next match.

**Search backward:**

```
C-r
```

**Replace:**

```
M-%
```

Then type:

1. String to find
2. String to replace with
3. Press `y` to replace, `n` to skip

---

### Undo

```
C-/  or  C-_  or  C-x u
```

---

### Exiting Emacs

**Exit Emacs:**

```
C-x C-c
```

If unsaved changes exist, Emacs asks to save.

**Cancel command:**

```
C-g
```

Use this when you pressed wrong keys.

---

### Getting Help in Emacs

**Help menu:**

```
C-h C-h
```

**Describe key:**

```
C-h k
```

Then press the key combination.

**Tutorial:**

```
C-h t
```

---

### Emacs Cheat Sheet

```
# Files
C-x C-f     - Open file
C-x C-s     - Save file
C-x C-w     - Save as

# Buffers
C-x b       - Switch buffer
C-x k       - Close buffer

# Windows
C-x 2       - Split horizontal
C-x 3       - Split vertical
C-x 1       - Close other windows
C-x o       - Switch window

# Editing
C-k         - Kill line
C-w         - Cut
M-w         - Copy
C-y         - Paste
C-/         - Undo

# Navigation
C-a/C-e     - Start/end of line
M-</M->     - Start/end of buffer
C-s         - Search

# Exit
C-x C-c     - Exit Emacs
```

---

## Vim vs Emacs

**Vim:**

- ✅ Modal editing (efficient)
- ✅ Lightweight and fast
- ✅ Available everywhere
- ✅ Steep learning curve, but powerful
- ✅ Great for quick edits

**Emacs:**

- ✅ Non-modal (more intuitive for beginners)
- ✅ Highly extensible
- ✅ Built-in features (email, IRC, games!)
- ✅ Gentler learning curve
- ✅ Great for long editing sessions

**The truth:** Both are excellent. Try both, pick what feels right!

---

## Summary

✅ **Vim** - Modal editor with powerful commands  
✅ **Vim modes** - Normal, Insert, Visual, Command  
✅ **Vim navigation** - hjkl and more  
✅ **Vim editing** - dd, yy, p, and more  
✅ **Emacs** - Extensible editor with key combinations  
✅ **Emacs buffers** - Multiple files open  
✅ **Both** - Professional-grade editors

---

## Practice Exercises

**Vim Exercises:**

```
1. Open a file with vim
2. Enter Insert mode and type 5 lines
3. Delete a line
4. Copy a line and paste it
5. Search for a word
6. Save and quit
```

**Emacs Exercises:**

```
1. Open a file with emacs
2. Type some text
3. Split window horizontally
4. Open another file in new window
5. Copy text from one to another
6. Save and exit
```

**Challenge:**

```
Edit the same file with both Vim and Emacs.
Which do you prefer?
```

---

## Next Steps

You now know the basics of both major Linux text editors!

**Continue to:** [Part 6: Users, Permissions & Processes](./06-users-permissions-processes.md)

---

[← Back to Part 4](./04-io-text-processing.md) | [Main Guide](../README.md) | [Next: Users & Processes →](./06-users-permissions-processes.md)
