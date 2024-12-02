`.mjs` vs `.js`:
- `.mjs` explicitly indicates it's an ES Module file
- It's used when you need to ensure Node.js treats the file as an ES Module
- Useful when your `package.json` doesn't have `"type": "module"`
- You can change it to `.js` if you add `"type": "module"` in package.json