# HostMatrix

HostMatrix is a static directory for discovering and comparing free hosting platforms. It is a single-page HTML app built with vanilla JavaScript and Tailwind CSS, with Font Awesome icons and Google Fonts loaded from CDNs.

## Run locally

Open `index.html` in a browser, or serve this directory with a local static server:

```sh
python3 -m http.server 8000
```

Then visit <http://localhost:8000>.

## Deploy to Vercel

This project has no build step or backend. The project root is the directory containing `index.html`.

1. Push this repository to GitHub.
2. In Vercel, choose **Add New Project** and import the GitHub repository.
3. Use the project root as the Root Directory and select **Other** as the Framework Preset.
4. Leave the Build Command and Output Directory unset, then deploy.

The app loads Tailwind CSS, Font Awesome, and Google Fonts from external CDNs, so those services must be reachable in the visitor's browser.

Hosting plan details in the directory can change; verify current limits and terms with each provider before relying on them.