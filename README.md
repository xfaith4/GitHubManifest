### BEGIN FILE: README.md
# GitHub Repository Manifest

An interactive dashboard that dynamically lists all your GitHub repositories — including pinned ones — and allows you to view each repository’s README inline.

Built with **vanilla JavaScript**, **TailwindCSS**, and **GitHub’s GraphQL API**.

---

## ✨ Features

- 🧩 Fetches **pinned + public repositories** via the GitHub GraphQL API  
- 🔒 Supports **private repositories** when authenticated with a personal access token  
- 📖 Displays each repo’s **README** inline (rendered from Markdown)  
- 🧭 **Search, sort, and filter** your projects  
- 📊 Sort by stars, last update, name, or pin status  
- 🧱 Toggle between **grid and list** views  
- ⚡ No frameworks or build steps — pure browser JavaScript  

---

## 🚀 Getting Started

### 1️⃣ Clone or Fork
```bash
git clone https://github.com/xfaith4/github-manifest.git
cd github-manifest
```

### 2️⃣ Configure
Open `index.html` in a text editor and find the configuration section near line 56. Set your GitHub username:
```javascript
// In index.html, around line 56:
const username = "your-github-username";  // <-- Replace with your GitHub username
```

### 3️⃣ (Optional) Add a Personal Access Token
For private repositories and enhanced API limits, add a token in `index.html`:
```javascript
// In index.html, around line 57:
const token = "your-personal-access-token";  // <-- Add your GitHub PAT here
```

### 4️⃣ Open in Browser
Simply open `index.html` in your browser — no build step required!

---

## 🗺️ Roadmap

We have exciting plans to enhance this dashboard with additional features including:

- 🔀 **Pull Requests** - View open PRs across all repositories
- 🐛 **Issues** - Track open issues and their status
- 📊 **Projects** - Display GitHub Projects and progress
- 📈 **Advanced Reporting** - Analytics and activity timelines

See [ROADMAP.md](ROADMAP.md) for the full enhancement plan and implementation details.

---

## 📄 License

This project is licensed under the GNU General Public License v3.0 - see the [LICENSE](LICENSE) file for details.
