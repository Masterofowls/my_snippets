# VS Code Extension Guide: Build, Package, Publish

A practical reference based on this project (`pp33q7rm2m4n6.my-react-snippets`).

## 1. Prerequisites

- Node.js (LTS or newer) and npm
- VS Code
- A Microsoft account (for the Marketplace)
- `vsce` is run through `npx @vscode/vsce ...`, so no global install is needed

## 2. Build an extension from scratch

### 2.1 Snippets-only extension (no code)

A snippet extension needs only a manifest and a snippet file:

```text
my-extension/
├── package.json
├── README.md
├── LICENSE
├── icon.png
├── .vscodeignore
└── snippets/
    └── react.code-snippets
```

Minimal `package.json`:

```json
{
  "name": "my-react-snippets",
  "displayName": "My React Snippets",
  "description": "Useful React snippets",
  "version": "0.0.1",
  "publisher": "your-publisher-id",
  "license": "MIT",
  "icon": "icon.png",
  "repository": { "type": "git", "url": "https://github.com/you/repo" },
  "engines": { "vscode": "^1.80.0" },
  "categories": ["Snippets"],
  "keywords": ["react", "snippets"],
  "contributes": {
    "snippets": [
      { "language": "javascriptreact", "path": "./snippets/react.code-snippets" },
      { "language": "typescriptreact", "path": "./snippets/react.code-snippets" }
    ]
  }
}
```

Snippet format (`*.code-snippets`, JSON):

```json
{
  "Snippet name": {
    "prefix": "comp",
    "body": [
      "export default function ${1:Name}() {",
      "  return <div>$0</div>;",
      "}"
    ],
    "description": "Shown in IntelliSense"
  }
}
```

Snippet syntax:

| Syntax | Meaning |
| --- | --- |
| `$1`, `$2` | Tab stops, visited in order |
| `$0` | Final cursor position |
| `${1:default}` | Tab stop with placeholder text |
| `${1\|a,b,c\|}` | Choice list |
| `\"` | Escaped quote inside the JSON string |

Rules to remember:

- Every prefix must be unique, or snippets collide in IntelliSense.
- Reusing the same number (for example `${1:item}`) mirrors the text across all occurrences.
- Avoid template-literal `${...}` in a body; VS Code reads it as a snippet variable. Use string concatenation instead.
- Snippets insert text only. They cannot add imports.

### 2.2 Extension with code (commands, UI, etc.)

Scaffold with Yeoman:

```powershell
npx --package yo --package generator-code -- yo code
```

Choose "New Extension (TypeScript)". Then:

- `src/extension.ts` holds `activate()` and `deactivate()`.
- Commands are declared under `contributes.commands` in `package.json` and registered with `vscode.commands.registerCommand`.
- Press **F5** to start an Extension Development Host and test.
- Build with `npm run compile` before packaging.

## 3. Test locally

1. Open the extension folder in VS Code.
2. Press **F5**. A new window opens with the extension loaded.
3. Open a file of the target language and type a snippet prefix.
4. After changing files, reload the host window (`Ctrl+R`).

To install the packaged file instead:

```powershell
code --install-extension .\my-react-snippets-0.0.1.vsix
```

## 4. Package

```powershell
npx @vscode/vsce package
```

This creates `<name>-<version>.vsix`. Check the printed file list.

Use `.vscodeignore` to keep junk out of the package:

```text
.git/**
.vscode/**
docs/**
*.vsix
*.jpg
```

Notes:

- `icon.png` must be a PNG, ideally 128x128 or larger. SVG is not allowed.
- The README becomes the Marketplace page. Relative image links need a `repository` field.
- Missing `repository` or `LICENSE` produces warnings, not errors.

## 5. Publish to the VS Code Marketplace

### 5.1 One-time setup

1. **Create a publisher** at https://marketplace.visualstudio.com/manage. The publisher ID is permanent and must match `"publisher"` in `package.json`.
2. **Create a Personal Access Token (PAT)** at https://dev.azure.com:
   - User settings, then Personal access tokens, then New Token.
   - Organization: **All accessible organizations**.
   - Scopes: **Custom defined**, then **Marketplace**, then **Manage**.
   - Copy the token immediately. It is shown only once. Never commit it.
3. **Log in**:

   ```powershell
   npx @vscode/vsce login <publisher-id>
   ```

### 5.2 Publish and update

```powershell
npx @vscode/vsce publish          # publishes the version in package.json
npx @vscode/vsce publish patch    # bumps 0.0.1 -> 0.0.2, then publishes
npx @vscode/vsce publish minor
npx @vscode/vsce publish major
```

You can also upload a `.vsix` manually on the manage page: **New extension, then Visual Studio Code**.

### 5.3 After publishing

- The listing shows "Verifying" for a few minutes (sometimes up to an hour). The public page returns 404 until verification passes.
- Public URL: `https://marketplace.visualstudio.com/items?itemName=<publisher>.<name>`
- Check status from the terminal:

  ```powershell
  npx @vscode/vsce show <publisher>.<name>
  ```

- Installs may take a little longer to appear in the VS Code Extensions view.

## 6. Publish to Open VSX (VSCodium, Cursor, etc.)

```powershell
npx ovsx create-namespace <publisher-id> -p <open-vsx-token>
npx ovsx publish my-react-snippets-0.0.1.vsix -p <open-vsx-token>
```

Create an account and token at https://open-vsx.org.

## 7. Release checklist

- [ ] Bump `version` in `package.json` (a published version can never be overwritten)
- [ ] Update the README prefix table for any new snippets
- [ ] Validate that the snippet JSON parses and prefixes are unique
- [ ] Run `npx @vscode/vsce package` and inspect the file list
- [ ] Test the `.vsix` with `code --install-extension`
- [ ] Run `npx @vscode/vsce publish`
- [ ] Commit and tag the release (`git tag v0.0.2`)

## 8. Common errors

| Error | Cause and fix |
| --- | --- |
| `already exists` | That version is already published. Bump `version`. |
| `Missing publisher name` | Add `"publisher"` to `package.json`. |
| `401` / `403` on publish | The PAT is wrong, expired, or lacks **Marketplace: Manage** scope. Log in again. |
| Public page shows 404 | The extension is still verifying. Wait and check the manage page. |
| Icon is rejected | Use PNG, not SVG. |
| Snippet does not appear | Check that the language ID in `contributes.snippets` matches the file, that the prefix is unique, and that the JSON is valid. |
| Package is too large | Add paths to `.vscodeignore`. |

## 9. Useful links

- Extension API: https://code.visualstudio.com/api
- Snippet syntax: https://code.visualstudio.com/docs/editing/userdefinedsnippets
- Publishing guide: https://code.visualstudio.com/api/working-with-extensions/publishing-extension
- Manage publishers: https://marketplace.visualstudio.com/manage
