<h1 align="center">🔒 ft_lock Customization Guide</h1>

<p align="center">
  <b>Customize your 42 / 1337 session lock screen — no sudo required.</b>
</p>

<p align="center">
  <a href="https://profile.intra.42.fr/users/yel-hbar"><img src="https://img.shields.io/static/v1?message=INTRA&logo=42&label=&color=0c5254&logoColor=white&style=for-the-badge" alt="42 Intra" /></a>
  <a href="https://instagram.com/yaakoubelhbari"><img src="https://img.shields.io/static/v1?message=Instagram&logo=instagram&label=&color=E4405F&logoColor=white&style=for-the-badge" alt="Instagram" /></a>
  <a href="https://linkedin.com/in/yaakoubelhbari"><img src="https://img.shields.io/static/v1?message=LinkedIn&logo=data:image/svg%2bxml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCAyNCAyNCIgZmlsbD0id2hpdGUiPjxwYXRoIGQ9Ik0yMC40NDcgMjAuNDUyaC0zLjU1NHYtNS41NjljMC0xLjMyOC0uMDI3LTMuMDM3LTEuODUyLTMuMDM3LTEuODUzIDAtMi4xMzYgMS40NDUtMi4xMzYgMi45Mzl2NS42NjdIOS4zNTFWOWgzLjQxNHYxLjU2MWguMDQ2Yy40NzctLjkgMS42MzctMS44NSAzLjM3LTEuODUgMy42MDEgMCA0LjI2NyAyLjM3IDQuMjY3IDUuNDU1djYuMjg2ek01LjMzNyA3LjQzM2MtMS4xNDQgMC0yLjA2My0uOTI2LTIuMDYzLTIuMDY1IDAtMS4xMzguOTItMi4wNjMgMi4wNjMtMi4wNjMgMS4xNCAwIDIuMDY0LjkyNSAyLjA2NCAyLjA2MyAwIDEuMTM5LS45MjUgMi4wNjUtMi4wNjQgMi4wNjV6bTEuNzgyIDEzLjAxOUgzLjU1NVY5aDMuNTY0djExLjQ1MnpNMjIuMjI1IDBIMS43NzFDLjc5MiAwIDAgLjc3NCAwIDEuNzI5djIwLjU0MkMwIDIzLjIyNy43OTIgMjQgMS43NzEgMjRoMjAuNDUxQzIzLjIgMjQgMjQgMjMuMjI3IDI0IDIyLjI3MVYxLjcyOUMyNCAuNzc0IDIzLjIgMCAyMi4yMjIgMGguMDAzeiIvPjwvc3ZnPgo=&label=&color=0A66C2&logoColor=white&style=for-the-badge" alt="LinkedIn" /></a>
  <a href="https://discord.com/users/949399987001905203"><img src="https://img.shields.io/static/v1?message=Discord&logo=discord&label=&color=5865F2&logoColor=white&style=for-the-badge" alt="Discord" /></a>
</p>

---

## 📖 How It Works

The campus lock screen (`/usr/share/42/ft_lock`) reads per‑user settings from the **X11 Resource Database** (`~/.Xresources`).  
You edit that file, reload it, and lock — done.

> [!IMPORTANT]
> **`~/.Xresources` is missing by default!**  
> On fresh campus accounts, `~/.Xresources` does not exist. Commands like `sed -i` will fail if the file does not exist. Run this once before configuring anything:
> ```bash
> touch ~/.Xresources
> ```
>
> **Rules to remember:**
> - Every option uses the prefix `ft_lock.<option>:` in `~/.Xresources`.
> - Background images **must** be `.jpg` / `.jpeg` with an **absolute path** (`/home/<login>/…`, not `~/…`). **Never use `.png` or invalid paths** — this breaks locking completely (GNOME lock icon and terminal command stop working). See [Troubleshooting](#troubleshooting).
> - After any change run `xrdb ~/.Xresources`, then press <kbd>Super</kbd> + <kbd>L</kbd> to test.

---

## ⚡ Quick Start

```bash
# 0. Ensure ~/.Xresources exists
touch ~/.Xresources

# 1. Set a background image
sed -i '/^ft_lock\.image_file:/d' ~/.Xresources
echo "ft_lock.image_file: /home/<your_login>/Pictures/lock.jpg" >> ~/.Xresources

# 2. Center the password prompt (1920x1080)
sed -i '/^ft_lock\.aff_geometry:/d' ~/.Xresources
echo "ft_lock.aff_geometry: +960+540" >> ~/.Xresources

# 3. Change the subtitle text
sed -i '/^ft_lock\.text:/d' ~/.Xresources
echo "ft_lock.text: Don't touch my PC 🐱" >> ~/.Xresources

# 4. Apply & test
xrdb ~/.Xresources
```

Then press <kbd>Super</kbd> + <kbd>L</kbd> to see your new lock screen!

---

## 🗂️ Options Reference

| Key | Description | Default |
| :--- | :--- | :---: |
| `ft_lock.image_file` | Absolute path to `.jpg` background (**JPG only, NOT PNG**) | 42 logo |
| `ft_lock.aff_geometry` | Center anchor of the prompt (`+X+Y`) | top‑left |
| `ft_lock.text` | Subtitle above the password box | `Back sOOn..` |
| `ft_lock.xft_colorname` | Text & box color (`#RRGGBB` or X11 name) | grey |
| `ft_lock.xft_fontname` | Xft font `Family:style=S:size=N` | system default |
| `ft_lock.passwordtext` | Mask characters while typing | `*` |
| `ft_lock.bkg_color` | Fallback fill if image doesn't cover screen | black |
| `ft_lock.transparent` | Enable transparency (`true` / `false`) | `false` |
| `ft_lock.geometry` | Window size & offset (`WxH+X+Y`) | fullscreen |
| `ft_lock.iconify` | Minimize windows on lock (`true` / `false`) | `false` |
| `ft_lock.title` | X11 window title | `ft_lock` |

---

## 🎨 Customizations

Every section below shows how to **set** an option and how to **delete** it (restoring the default).

> [!TIP]
> The **Set** pattern is always the same — delete the old line, append the new one, reload:
> ```bash
> sed -i '/^ft_lock\.KEY:/d' ~/.Xresources
> echo "ft_lock.KEY: VALUE" >> ~/.Xresources
> xrdb ~/.Xresources
> ```

---

### 🖼️ Background Image

Must be a `.jpg` / `.jpeg` with a full absolute path.

> [!WARNING]
> **🚨 Using `.png` or an invalid/missing image breaks `ft_lock` entirely!**  
> If `ft_lock.image_file` points to a `.png` or an invalid/corrupted `.jpg`:
> - ❌ The desktop **cannot be locked**.
> - ❌ The lock icon **stops working**.
>
> **Quick Fix:** Remove the background variable line or replace it with a valid `.jpg` image (see [Troubleshooting](#troubleshooting)).

<details>
<summary><b>💡 Tip — convert & resize any image to 1920×1080 JPG</b></summary>

`ft_lock` draws the image at native resolution without scaling, so convert it first:

```bash
convert ~/Pictures/your-image.png \
  -resize 1920x1080^ -gravity center -extent 1920x1080 \
  ~/Pictures/Wallpapers/lock.jpg
```

Then point `ft_lock.image_file` to `/home/<your_login>/Pictures/Wallpapers/lock.jpg`.

</details>

**Set**
```bash
sed -i '/^ft_lock\.image_file:/d' ~/.Xresources
echo "ft_lock.image_file: /home/<your_login>/Pictures/Wallpapers/lock.jpg" >> ~/.Xresources
xrdb ~/.Xresources
```

**Delete** *(restores default 42 logo background)*
```bash
sed -i '/^ft_lock\.image_file:/d' ~/.Xresources
xrdb -remove && xrdb -merge ~/.Xresources
```

---

### 📍 Prompt Position

`aff_geometry` is the center anchor (`+X+Y`) of the password prompt.  
On a 1920×1080 monitor, `+960+540` is the exact center.

**Set (1920×1080)**
```bash
sed -i '/^ft_lock\.aff_geometry:/d' ~/.Xresources
echo "ft_lock.aff_geometry: +960+540" >> ~/.Xresources
xrdb ~/.Xresources
```

<details>
<summary><b>🖥️ Auto‑center on any resolution</b></summary>

```bash
bash -c '
read W H <<< $(xrandr --current | awk "/\*/ {split(\$1,a,\"x\"); print a[1], a[2]; exit}")
X=$(( W / 2 ))
Y=$(( H / 2 ))
sed -i "/^ft_lock\.aff_geometry:/d" ~/.Xresources
echo "ft_lock.aff_geometry: +${X}+${Y}" >> ~/.Xresources
xrdb ~/.Xresources
'
```

</details>

**Delete**
```bash
sed -i '/^ft_lock\.aff_geometry:/d' ~/.Xresources
xrdb -remove && xrdb -merge ~/.Xresources
```

---

### ✏️ Subtitle Text

Controls the `"Back sOOn.."` line shown on the lock screen.

**Set**
```bash
sed -i '/^ft_lock\.text:/d' ~/.Xresources
echo "ft_lock.text: Your custom text here" >> ~/.Xresources
xrdb ~/.Xresources
```

**Hide completely** — leave the value blank:
```bash
sed -i '/^ft_lock\.text:/d' ~/.Xresources
echo "ft_lock.text:" >> ~/.Xresources
xrdb ~/.Xresources
```

**Delete** *(restores `Back sOOn..`)*
```bash
sed -i '/^ft_lock\.text:/d' ~/.Xresources
xrdb -remove && xrdb -merge ~/.Xresources
```

---

### 🎨 Text / Box Color

Accepts hex (`#RRGGBB`) or an X11 color name (e.g. `white`, `cyan`, `tomato`).

**Set**
```bash
sed -i '/^ft_lock\.xft_colorname:/d' ~/.Xresources
echo "ft_lock.xft_colorname: #FFFFFF" >> ~/.Xresources
xrdb ~/.Xresources
```

**Delete**
```bash
sed -i '/^ft_lock\.xft_colorname:/d' ~/.Xresources
xrdb -remove && xrdb -merge ~/.Xresources
```

---

### 🔤 Font

Format: `Family:style=<Style>:size=<N>:antialias=true`

**Set**
```bash
sed -i '/^ft_lock\.xft_fontname:/d' ~/.Xresources
echo "ft_lock.xft_fontname: Ubuntu:style=Regular:size=8:antialias=true" >> ~/.Xresources
xrdb ~/.Xresources
```

> [!NOTE]
> Other examples: `Ubuntu:style=Bold:size=14:antialias=true`  
> List installed fonts: `fc-list : family style`

**Delete**
```bash
sed -i '/^ft_lock\.xft_fontname:/d' ~/.Xresources
xrdb -remove && xrdb -merge ~/.Xresources
```

---

### 🔑 Password Mask Characters

Characters displayed while you type your password.

**Set**
```bash
sed -i '/^ft_lock\.passwordtext:/d' ~/.Xresources
echo "ft_lock.passwordtext: ********" >> ~/.Xresources
xrdb ~/.Xresources
```

**Delete**
```bash
sed -i '/^ft_lock\.passwordtext:/d' ~/.Xresources
xrdb -remove && xrdb -merge ~/.Xresources
```

---

### 🌈 Fallback Background Color

Used when the image doesn't cover the full screen.

**Set**
```bash
sed -i '/^ft_lock\.bkg_color:/d' ~/.Xresources
echo "ft_lock.bkg_color: #000000" >> ~/.Xresources
xrdb ~/.Xresources
```

**Delete**
```bash
sed -i '/^ft_lock\.bkg_color:/d' ~/.Xresources
xrdb -remove && xrdb -merge ~/.Xresources
```

---

### 👻 Transparency

**Set**
```bash
sed -i '/^ft_lock\.transparent:/d' ~/.Xresources
echo "ft_lock.transparent: true" >> ~/.Xresources
xrdb ~/.Xresources
```

**Delete**
```bash
sed -i '/^ft_lock\.transparent:/d' ~/.Xresources
xrdb -remove && xrdb -merge ~/.Xresources
```

---

### 📐 Window Geometry

Format: `WxH+X+Y`

**Set**
```bash
sed -i '/^ft_lock\.geometry:/d' ~/.Xresources
echo "ft_lock.geometry: 1920x1080+0+0" >> ~/.Xresources
xrdb ~/.Xresources
```

**Hide the lock screen entirely (render 1x1 off-screen)**

Renders the lock window at 1x1 pixel far off-screen so the lock screen is completely hidden while still locking your session:
```bash
grep -q "^ft_lock\.geometry:" ~/.Xresources \
  && sed -i 's/^ft_lock\.geometry:.*/ft_lock.geometry: 1x1+9999+9999/' ~/.Xresources \
  || echo "ft_lock.geometry: 1x1+9999+9999" >> ~/.Xresources
xrdb ~/.Xresources
```

**Delete**
```bash
sed -i '/^ft_lock\.geometry:/d' ~/.Xresources
xrdb -remove && xrdb -merge ~/.Xresources
```

---

### 📦 Iconify Windows

Minimizes all open windows when the screen locks.

**Set**
```bash
sed -i '/^ft_lock\.iconify:/d' ~/.Xresources
echo "ft_lock.iconify: true" >> ~/.Xresources
xrdb ~/.Xresources
```

**Delete**
```bash
sed -i '/^ft_lock\.iconify:/d' ~/.Xresources
xrdb -remove && xrdb -merge ~/.Xresources
```

---

### 🏷️ Window Title

**Set**
```bash
sed -i '/^ft_lock\.title:/d' ~/.Xresources
echo "ft_lock.title: My Lock" >> ~/.Xresources
xrdb ~/.Xresources
```

**Delete**
```bash
sed -i '/^ft_lock\.title:/d' ~/.Xresources
xrdb -remove && xrdb -merge ~/.Xresources
```

---

## 🚨 Troubleshooting: Lock Screen Won't Lock? <a id="troubleshooting"></a>

> [!CAUTION]
> **Symptoms:**
> - ❌ Clicking the **lock icon** does nothing.
> - ❌ Running `ft_lock` in the terminal silently exits without locking.
> - ❌ The desktop **cannot be locked at all**!

### Why does this happen?
`ft_lock` crashes silently when it fails to load the background image. The most common causes are:
1. **Using a `.png` image:** `ft_lock` only supports `.jpg` / `.jpeg`.
2. **Using a relative path:** Using `~/...` instead of the full absolute path (`/home/<your_login>/...`).
3. **Invalid or non-existent file path:** Misspelled filename or directory.

---

### How to fix it

#### Remove the background line (Instant Fix)
Delete the `ft_lock.image_file` line from `~/.Xresources` and reload the X11 database to restore the default 42 logo lock screen:

```bash
sed -i '/^ft_lock\.image_file:/d' ~/.Xresources
xrdb -remove && xrdb -merge ~/.Xresources
```

Press the lock icon — your lock screen will work again immediately!

---

## 🔄 Reset Everything to 42 Defaults

Remove all `ft_lock.*` entries and clear the active database to restore the original lock screen:

```bash
sed -i '/^ft_lock\./Id' ~/.Xresources
xrdb -remove && xrdb -merge ~/.Xresources
```

Verify (should print nothing):
```bash
xrdb -query | grep -i ft_lock
```

---

## 🔍 Verify Your Settings

At any time, check what's currently loaded:

```bash
xrdb -query | grep ft_lock
```

---

<p align="center">
  Made with ❤️ for 42 / 1337 students
</p>
