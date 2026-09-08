# Installing Git and Setting Up Git Credentials

A step-by-step guide for downloading Git and configuring credentials so you can push/pull from GitHub, GitLab, or similar platforms.

---

## 1. Download and Install Git

Go to [git-scm.com/downloads](https://git-scm.com/downloads) and grab the installer for your operating system.

- **Windows**: Run the `.exe` installer and accept the defaults. This also installs Git Bash and Git GUI.
- **macOS**: Open Terminal and run `git --version`. If Git isn't installed, macOS will prompt you to install the Xcode Command Line Tools, which include Git.
- **Linux**:
  - Debian/Ubuntu: `sudo apt install git`
  - Fedora: `sudo dnf install git`

---

## 2. Verify the Installation

Open a terminal (or Git Bash on Windows) and run:

```bash
git --version
```

You should see a version number printed back, confirming Git is installed and available on your PATH.

---

## 3. Set Your Identity

Git stamps every commit with a name and email. Set these globally so they apply to all repositories:

```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```

> Use the same email associated with your GitHub/GitLab/etc. account so your commits link to your profile.

---

## 4. Test It With a Clone or Push

Run a Git operation against a private repo:

```bash
git clone https://github.com/you/your-repo.git
```

When prompted for a username and password:
- **Username**: your account username
- **Password**: your credential (e.g. Personal Access Token, if required by the platform)

---

## Quick Reference

| Task | Command |
|---|---|
| Check Git version | `git --version` |
| Set name | `git config --global user.name "Your Name"` |
| Set email | `git config --global user.email "you@example.com"` |
| Clone a repo | `git clone <repo-url>` |