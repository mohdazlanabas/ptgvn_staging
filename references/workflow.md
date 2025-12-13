# WordPress + Git + Server Workflow

This document describes the workflow for managing a WordPress project with three environments:

- **Local Machine (Mac)** – main place for coding
- **GitHub** – central source of truth for code
- **Server (`ptgvn.site`)** – live site, pulling latest code from GitHub

The key principle:

> **All code flows: Local ➜ GitHub ➜ Server**  
> Content (posts, pages, menus, most settings) stays in the WordPress database on the server.

---

## 1. Make changes in WordPress Admin (Server)

Use **WordPress Admin** on the server for:

- Installing / updating plugins
- Changing theme settings, menus, widgets, and other GUI elements
- Adjusting plugin configurations
- (Only when necessary) small edits via the Theme File Editor

These actions mainly affect:

- The **database** (content and settings) – *not tracked in Git*
- Sometimes the **filesystem** (plugin/theme files) – *can be tracked in Git*

---

## 2. Sync Server ➜ GitHub

After making plugin or GUI-related changes in WordPress Admin, capture any filesystem changes in Git on the server.

On the **server**:

```bash
cd ~/ptgvn.site

# See what changed
git status

# Stage all relevant changes (themes, plugins, etc.)
git add .

# Commit with a clear message
git commit -m "WP admin / plugin updates on server"

# Push to GitHub
git push origin main
```

Now **GitHub** matches the **server filesystem** (themes, plugins, custom code).  
Database changes are not in Git, which is expected.

---

## 3. Sync GitHub ➜ Local

On your **local machine (Mac)**, pull the latest server/GitHub state before doing new coding work.

```bash
cd ~/Projects/ptgvn_staging

# Pull latest changes from GitHub
git pull origin main
```

Now the **local code** is aligned with GitHub and the server.

This ensures you are always coding on top of the latest plugin/theme updates and any hotfixes performed on the server.

---

## 4. Code Locally (Primary Development)

Do all structured coding on your **local machine** using your editor (e.g. VS Code):

Typical changes:

- Custom theme development (`wp-content/themes/your-theme-or-child-theme`)
- Custom plugin development (`wp-content/plugins/your-custom-plugin`)
- CSS, JS, PHP templates, hooks, and logic
- Refactoring and new features

Local workflow:

```bash
cd ~/Projects/ptgvn_staging

# Check what changed
git status

# Stage files you modified
git add path/to/file1 path/to/file2
# or stage everything
git add .

# Commit with a descriptive message
git commit -m "Implement feature X / Update theme Y"

# Push to GitHub
git push origin main
```

Now **GitHub** has both:

- The WordPress Admin-driven changes (previously pushed from server)
- Your new local code changes

---

## 5. Deploy GitHub ➜ Server (Update Live Site)

To update the live site with the latest code from GitHub, pull on the server.

On the **server**:

```bash
cd ~/ptgvn.site

# Pull the latest code from GitHub
git pull origin main
```

This makes the server’s filesystem match GitHub.

If the WordPress site is served directly from `~/ptgvn.site`, the live site will reflect the new code immediately after the pull.

---

## 6. Summary of the Full Loop

Full end-to-end loop:

1. **Admin changes on server**
   - Use WordPress Admin for plugins, GUI, settings, menus, etc.

2. **Server ➜ GitHub**
   ```bash
   cd ~/ptgvn.site
   git add .
   git commit -m "WP admin / plugin changes"
   git push origin main
   ```

3. **GitHub ➜ Local**
   ```bash
   cd ~/Projects/ptgvn_staging
   git pull origin main
   ```

4. **Code on Local**
   ```bash
   # edit in your editor
   git add .
   git commit -m "Custom code changes"
   git push origin main
   ```

5. **GitHub ➜ Server (Deploy)**
   ```bash
   cd ~/ptgvn.site
   git pull origin main
   ```

At each stage, Git keeps a history of code changes, while the WordPress database continues to handle content and configuration.

---

## 7. Guidelines & Best Practices

- Use **WordPress Admin** primarily for:
  - Content (posts, pages)
  - Menus, widgets
  - Plugin settings
  - Plugin/theme updates

- Use **local development** primarily for:
  - Theme and plugin PHP code
  - CSS, JS, build processes
  - Any non-trivial refactoring or new features

- If you ever make emergency code edits directly on the server (via WP Theme Editor or SSH):
  1. Commit and push from the server.
  2. Pull those changes to local before doing further development.

This keeps all three environments:

- **Local**
- **GitHub**
- **Server**

in a controlled, predictable workflow.
