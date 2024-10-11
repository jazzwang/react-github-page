# Deploy React App to Github Page

## 2024-10-09

- 緣起：小孩寫國語作業需要知道「部首」在國字裡的位置。經查詢，有一個字型可以突顯「部首」的位置。所以想試試看可否部屬一個 React App 到 Github Page
  - [中文如何做出「部首變色」效果？使用免費的課本生詞「曉聲通部首字型」。](https://toneoz.com/blog/2023/03/27/radical/)
  - https://app.intcome.com/public/jy 這個 React App 可以用有注音的字體顯示打出來的中文字
- 作法：
  - 請 Perplexity AI 幫忙：https://www.perplexity.ai/search/how-to-deploy-react-app-to-git-k4WtH02cRruugqs_5hLCTg#0

## 2024-10-10

- ( 2024-10-10 13:48:29 )
- 因為 Github Page 需要獨立的 repo，所以使用 Github CLI 來產生一個新的專案。然後用 CodeSpace 來做開發，就不用在本地端建立 node.js 環境 :P
```bash
~/git$ gh repo create
? What would you like to do? Create a new repository on GitHub from scratch
? Repository name react-github-page

? Repository name react-github-page
? Repository owner jazzwang
? Description Example React.js App deploy to Github Page

? Description Example React.js App deploy to Github Page
? Visibility Public
? Would you like to add a README file? No
? Would you like to add a .gitignore? No
? Would you like to add a license? No
? This will create "react-github-page" as a public repository on GitHub. Continue? Yes
✓ Created repository jazzwang/react-github-page on GitHub
? Clone the new repository locally? Yes
```
- 建立 Github CodeSpace
```bash
~/git/react-github-page$ gh cs create
? Repository: jazzwang/react-github-page
  ✓ Codespaces usage for this repository is paid for by jazzwang
? Branch (leave blank for default branch):
? Choose Machine Type: 2 cores, 8 GB RAM, 32 GB storage
reimagined-acorn-r47j4x5rwfw5wj
```
- 修改 Github CodeSpace 的 Display Name
```bash
~/git/react-github-page$ gh cs edit -d "react-github-page"
? Choose codespace:  [Use arrows to move, type to filter]
> jazzwang/react-github-page (main*): reimagined acorn
  MLOps-Courses/mlops-coding-course (main): mlops
  jazzwang/snippet (master*): snippet
```
- 用 VS Code 開啟 Github CodeSpace
```bash
~/git/react-github-page$ gh cs ssh
? Choose codespace:  [Use arrows to move, type to filter]
> jazzwang/react-github-page (main*): react-github-page
  MLOps-Courses/mlops-coding-course (main): mlops
  jazzwang/snippet (master*): snippet
```
- 測試是否有 `npm` 指令：
```bash
@jazzwang ➜ /workspaces/react-github-page (main) $ which npm
/home/codespace/nvm/current/bin/npm
@jazzwang ➜ /workspaces/react-github-page (main) $ which npx
/home/codespace/nvm/current/bin/npx
```
- Step 2: Set Up Your React Application
- 建立一個新的 React App
```bash
@jazzwang ➜ /workspaces/react-github-page (main) $ npx create-react-app base

Creating a new React app in /workspaces/react-github-page/base.

Installing packages. This might take a couple of minutes.
Installing react, react-dom, and react-scripts with cra-template...

added 1478 packages in 3m

262 packages are looking for funding
  run `npm fund` for details

Installing template dependencies using npm...

added 63 packages, and changed 1 package in 7s

262 packages are looking for funding
  run `npm fund` for details
Removing template package using npm...


removed 1 package, and audited 1541 packages in 4s

262 packages are looking for funding
  run `npm fund` for details

8 vulnerabilities (2 moderate, 6 high)

To address all issues (including breaking changes), run:
  npm audit fix --force

Run `npm audit` for details.

Success! Created base at /workspaces/react-github-page/base
Inside that directory, you can run several commands:

  npm start
    Starts the development server.

  npm run build
    Bundles the app into static files for production.

  npm test
    Starts the test runner.

  npm run eject
    Removes this tool and copies build dependencies, configuration files
    and scripts into the app directory. If you do this, you can’t go back!

We suggest that you begin by typing:

  cd base
  npm start

Happy hacking!
```
- Step 3: Install the gh-pages Package
```bash
@jazzwang ➜ /workspaces/react-github-page (main) $ 
@jazzwang ➜ /workspaces/react-github-page (main) $ cd base/
@jazzwang ➜ /workspaces/react-github-page/base (main) $ npm install gh-pages --save-dev

added 13 packages, and audited 1554 packages in 5s

263 packages are looking for funding
  run `npm fund` for details

8 vulnerabilities (2 moderate, 6 high)

To address all issues (including breaking changes), run:
  npm audit fix --force

Run `npm audit` for details.
@jazzwang ➜ /workspaces/react-github-page/base (main) $ 
```
- Step 4: Configure package.json
```bash
@jazzwang ➜ /workspaces/react-github-page/base (main) $ cp package.json package.json.orig
@jazzwang ➜ /workspaces/react-github-page/base (main) $ code package.json
```
- changes:
```bash
@jazzwang ➜ /workspaces/react-github-page/base (main) $ diff -Naur package.json.orig package.json
```
```diff
--- package.json.orig   2024-10-10 15:26:52.553554315 +0000
+++ package.json        2024-10-10 15:29:26.037562045 +0000
@@ -1,4 +1,5 @@
 {
+  "homepage": "https://jazzwang.github.io/react-github-page",
   "name": "base",
   "version": "0.1.0",
   "private": true,
@@ -15,7 +16,9 @@
     "start": "react-scripts start",
     "build": "react-scripts build",
     "test": "react-scripts test",
-    "eject": "react-scripts eject"
+    "eject": "react-scripts eject",
+    "predeploy": "npm run build",
+    "deploy": "gh-pages -d build"
   },
   "eslintConfig": {
     "extends": [
```
- Step 5: Deploy Your Application
```bash
@jazzwang ➜ /workspaces/react-github-page/base (main) $ git add .
@jazzwang ➜ /workspaces/react-github-page/base (main) $ git commit -m "Setup for GitHub Pages deployment"

[main 88ac4f7] Setup for GitHub Pages deployment
 18 files changed, 20251 insertions(+)
 create mode 100644 base/.gitignore
 create mode 100644 base/README.md
 create mode 100644 base/package-lock.json
 create mode 100644 base/package.json
 create mode 100644 base/public/favicon.ico
 create mode 100644 base/public/index.html
 create mode 100644 base/public/logo192.png
 create mode 100644 base/public/logo512.png
 create mode 100644 base/public/manifest.json
 create mode 100644 base/public/robots.txt
 create mode 100644 base/src/App.css
 create mode 100644 base/src/App.js
 create mode 100644 base/src/App.test.js
 create mode 100644 base/src/index.css
 create mode 100644 base/src/index.js
 create mode 100644 base/src/logo.svg
 create mode 100644 base/src/reportWebVitals.js
 create mode 100644 base/src/setupTests.js
@jazzwang ➜ /workspaces/react-github-page/base (main) $ 
@jazzwang ➜ /workspaces/react-github-page/base (main) $ git push -u origin main 
Enumerating objects: 24, done.
Counting objects: 100% (24/24), done.
Delta compression using up to 2 threads
Compressing objects: 100% (23/23), done.
Writing objects: 100% (23/23), 182.51 KiB | 3.80 MiB/s, done.
Total 23 (delta 0), reused 0 (delta 0), pack-reused 0 (from 0)
To https://github.com/jazzwang/react-github-page
   e57d8a3..88ac4f7  main -> main
branch 'main' set up to track 'origin/main'.
```
- run the deploy command:
```bash
@jazzwang ➜ /workspaces/react-github-page/base (main) $ npm run deploy

> base@0.1.0 predeploy
> npm run build


> base@0.1.0 build
> react-scripts build

Creating an optimized production build...
One of your dependencies, babel-preset-react-app, is importing the
"@babel/plugin-proposal-private-property-in-object" package without
declaring it in its dependencies. This is currently working because
"@babel/plugin-proposal-private-property-in-object" is already in your
node_modules folder for unrelated reasons, but it may break at any time.

babel-preset-react-app is part of the create-react-app project, which
is not maintianed anymore. It is thus unlikely that this bug will
ever be fixed. Add "@babel/plugin-proposal-private-property-in-object" to
your devDependencies to work around this error. This will make this message
go away.
  
Compiled successfully.

File sizes after gzip:

  46.59 kB  build/static/js/main.3a3034bd.js
  1.77 kB   build/static/js/453.ed57f822.chunk.js
  513 B     build/static/css/main.f855e6bc.css

The project was built assuming it is hosted at /react/.
You can control this with the homepage field in your package.json.

The build folder is ready to be deployed.

Find out more about deployment here:

  https://cra.link/deployment


> base@0.1.0 deploy
> gh-pages -d build

Published
```
- 結果：
  - 確實可以看得到一個新的 Github Page 佈署到 https://github.com/jazzwang/react-github-page
  - 但看原始碼，應該是因為預設認定路徑是 `/react/.` 實際上必須用相對路徑，而非絕對路徑。所以還需要微調一下。
  - 在 Github Repo 也可以看到新的 `gh-pages` branch
```bash
@jazzwang ➜ /workspaces/react-github-page/base (main) $ git pull -a
remote: Enumerating objects: 21, done.
remote: Counting objects: 100% (21/21), done.
remote: Compressing objects: 100% (21/21), done.
remote: Total 21 (delta 0), reused 21 (delta 0), pack-reused 0 (from 0)
Unpacking objects: 100% (21/21), 183.37 KiB | 7.97 MiB/s, done.
From https://github.com/jazzwang/react-github-page
 * [new branch]      gh-pages   -> origin/gh-pages
Already up to date.
```

## 2024-10-11

- remove `base` example project
```bash
@jazzwang ➜ /workspaces/react-github-page (main) $ git rm -r base
[main e197d44] remove example project
 18 files changed, 20251 deletions(-)
 delete mode 100644 base/.gitignore
 delete mode 100644 base/README.md
 delete mode 100644 base/package-lock.json
 delete mode 100644 base/package.json
 delete mode 100644 base/public/favicon.ico
 delete mode 100644 base/public/index.html
 delete mode 100644 base/public/logo192.png
 delete mode 100644 base/public/logo512.png
 delete mode 100644 base/public/manifest.json
 delete mode 100644 base/public/robots.txt
 delete mode 100644 base/src/App.css
 delete mode 100644 base/src/App.js
 delete mode 100644 base/src/App.test.js
 delete mode 100644 base/src/index.css
 delete mode 100644 base/src/index.js
 delete mode 100644 base/src/logo.svg
 delete mode 100644 base/src/reportWebVitals.js
 delete mode 100644 base/src/setupTests.js
```