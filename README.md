# React Router SPA Template

A minimal React 19 + Vite + React Router v7 single-page application template, ready for deployment to GitHub Pages.

## Features

- React 19, Vite, and SWC for fast development
- React Router v7 with configurable basename for subpath hosting
- Centralized dark theme styling (`src/styles.css`)
- Three demo pages: Home, About, Contact, plus a custom 404 page
- Image usage examples (import, public folder, external)
- GitHub Actions workflow for automatic deployment to GitHub Pages
- SPA routing support on GitHub Pages (404 fallback)

## Project Structure

```
src/
  App.jsx           # Main app with routes
  main.jsx          # Entry, sets up BrowserRouter
  styles.css        # All styles in one file
  assets/           # Example image assets
  components/
    Navbar.jsx      # Navigation bar with NavLink
  pages/
    HomePage.jsx
    AboutPage.jsx
    ContactPage.jsx
    NotFoundPage.jsx
public/
  logo.webp         # Example public image
  # (No 404.html needed; see deployment notes)
index.html
vite.config.js
.github/
  workflows/
    deploy.yml      # GitHub Actions deployment workflow
```

## Usage

### Development

```sh
npm install
npm run dev
```

### Image Usage in React

1. **Import from `src/assets`**  
   Import at the top of your component. Vite will bundle and hash the file.
   ```jsx
   import logo from "../assets/logo.svg";
   <img src={logo} alt="Logo" />;
   ```
2. **Public folder**  
   Place the image in `/public` and reference by path (e.g. `logo.webp`).
   ```jsx
   <img src="logo.webp" alt="Logo from public" />
   ```
3. **External URL**  
   Use a full URL for images from the internet.

### Routing

- All routes are defined in `src/App.jsx`.
- Navigation uses `NavLink` for active link styling.
- The 404 page is shown for unknown routes.

## Deployment to GitHub Pages

- Set your deployment base path in `package.json`:
  ```json
  {
    "base": "/react-router-spa/"
  }
  ```
- The Vite config reads this value for production builds:
  ```js
  // vite.config.js
  import pkg from "./package.json";
  export default defineConfig(({ command }) => ({
    base: command === "serve" ? "/" : pkg.base,
    plugins: [react()]
  }));
  ```
- `BrowserRouter` uses the correct basename:
  ```jsx
  <BrowserRouter basename={import.meta.env.BASE_URL}>
    <App />
  </BrowserRouter>
  ```
- The GitHub Actions workflow (`.github/workflows/deploy.yml`) builds and deploys automatically on push to `main`.
- The workflow copies `index.html` to `404.html` in the build output, so SPA routing works on GitHub Pages.

**If your repository path changes, update `base` in `package.json` and redeploy.**

## SPA 404 Fallback

GitHub Pages does not support client-side routing out of the box. The deployment workflow copies `index.html` to `404.html` in the build output, so deep links and refreshes work. You do **not** need a 404.html in your public folder.

## Customization

- Edit or add pages in `src/pages/`.
- Update navigation in `src/components/Navbar.jsx`.
- Change styles in `src/styles.css`.

---

## React Compiler

The React Compiler is currently not compatible with SWC. See [this issue](https://github.com/vitejs/vite-plugin-react/issues/428) for tracking the progress.

## Expanding the ESLint configuration

If you are developing a production application, we recommend using TypeScript with type-aware lint rules enabled. Check out the [TS template](https://github.com/vitejs/vite/tree/main/packages/create-vite/template-react-ts) for information on how to integrate TypeScript and [`typescript-eslint`](https://typescript-eslint.io) in your project.
