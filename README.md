# IFL Management Portal

## Project Overview
This repository contains a static web portal that aggregates four existing IFL modules (Attendance, Inventory, OD + Loop, Gate Pass) into a single UI. The portal and modules are pure client‑side HTML/JS/CSS and can be served by any static web host.

## Folder Structure
```
IFL Management System/
├─ index.html            # Portal shell with sidebar, KPI overview, theme toggle
├─ serve_portal.mjs      # Simple Node.js static server for local testing
├─ modules/
│   ├─ attendance.html
│   ├─ inventory.html
│   ├─ distribution.html
│   └─ gatepass.html
└─ modules/portal-seeds.js  # Seed data for the executive overview
```

## Local Development
1. **Prerequisites**: Node.js (v18+) installed and available in your `PATH`.
2. Start the dev server:
   ```powershell
   node serve_portal.mjs
   ```
3. Open a browser to `http://127.0.0.1:4173` to view the portal locally.

## Deploying to Static Hosts
The portal can be deployed to any static‑file hosting service. Below are quick guides for common platforms (all require that you push the entire `IFL Management System` folder).

### 1. GitHub Pages
1. Initialise a Git repository in the folder (if not already):
   ```powershell
   git init
   git add .
   git commit -m "Initial commit"
   ```
2. Create a new repository on GitHub and add it as a remote:
   ```powershell
   git remote add origin https://github.com/<USERNAME>/<REPO>.git
   git branch -M main
   git push -u origin main
   ```
3. In the GitHub repository settings enable **Pages** and set the source to the `main` branch (root). GitHub will publish the site at `https://<USERNAME>.github.io/<REPO>/`.

### 2. Netlify (Free tier works for static sites)
1. Sign‑up/in at https://app.netlify.com and click **New site from Git**.
2. Connect your GitHub repo containing the portal.
3. Set **Build command** to `echo "No build needed"` (or leave blank) and **Publish directory** to the repository root.
4. Deploy – Netlify will give you a URL like `https://<site‑name>.netlify.app`.

### 3. Azure Static Web Apps
1. Install the Azure CLI and log in:
   ```powershell
   az login
   ```
2. Create a Static Web App (choose **Free** SKU) and point it to your GitHub repo.
3. In the “Build Details” set **App location** to `/` and **Output location** to `/` (no build step).
4. Azure will provision a site at `https://<app-name>.azurestaticapps.net`.

### 4. Cloudflare Pages
1. Add a new project at https://pages.cloudflare.com and link your GitHub repository.
2. Use the default settings – **Build command** can be left empty, **Build output directory** is the repo root.
3. After the first deployment you’ll receive a `your‑site.pages.dev` URL.

## Important Notes for All Hosts
- The site is static; no server‑side processing is required.
- All resources (HTML, JS, CSS, images) must be present in the repository. Do **not** reference local file paths like `c:\...`.
- If you host the portal under a sub‑path (e.g., `https://example.com/ifl/`), update any absolute URLs in `index.html` and the module iframes to use relative paths (e.g., `src="modules/attendance.html"`).
- The backup/restore modal works entirely in the browser using `localStorage`. Users will need to export/import data when moving to a new host or device.

## Optional: Docker Container for Static Serving
If you prefer container deployment, you can use the following `Dockerfile`:
```Dockerfile
FROM node:20-alpine AS builder
WORKDIR /app
COPY . .
RUN npm install -g serve
EXPOSE 5000
CMD ["serve", "-s", ".", "-l", "5000"]
```
Build and run:
```powershell
docker build -t ifl-portal .
 docker run -p 5000:5000 ifl-portal
```
Then visit `http://localhost:5000`.

---
Feel free to ask if you need a more detailed walkthrough for a specific platform or any further customisation.
