# Npm Publish

Exporting Library Repository To NPM  

## **Build and Publish**
- Run `npm install` to ensure all dependencies are installed.
- Run `npm run build` to generate `dist/`.
- Run `npm login` if you haven’t logged in to npm.
- Run `npm publish` to publish your package.

Now your package is live and ready for others to install via:

```
npm install event-emitter
```

## Common Errors

1. **Package Name Conflict (`event-emitter`)**  
   - The name **`event-emitter`** is likely already taken on npm. Try using a unique name like:
     ```
     npm info event-emitter
     ```
     If it's taken, rename your package to something unique in `package.json`, e.g.,  
     ```json
     {
       "name": "@atari-monk/event-emitter"
     }
     ```
     This scopes it under your username and avoids conflicts.

2. Try publishing again:  
   ```
   npm publish --access public
   ```
   (The `--access public` flag is needed for scoped packages.)
