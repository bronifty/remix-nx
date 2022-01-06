# Remix in NX Workspace

credit to [Brandon](https://github.com/brandonroberts/nx-remixed)

### Install and init the nx workspace and remix app

```
npx create-nx-workspace@latest
npx create-remix@latest ./apps/remix
yarn
yarn nx dev remix
(or nx dev remix if you have nx-cli installed globally)
```

### The secret sauce of nx workspace is the workspace.json folder in the root as well as the no-hoist element in package.json root (for projects like remix which aren't natively supported by plugins)

- workspace.json needs project with root and targets

```
/* ./workspace.json */
{
  "version": 2,
  "projects": {
    "remix": {
      "root": "apps/remix",
      "targets": {
        "dev": {
          "executor": "@nrwl/workspace:run-script",
          "options": {
            "script": "remix dev"
          }
        }
      }
    }
  }
}
```

- root / workspace package.json needs workspaces with packages and nohoist as well as private true and tslib dependency

```
/* ./package.json */
"workspaces": {
    "packages": [
      "apps/**"
    ],
    "nohoist": [
      "**/remix/",
      "**/remix/**/*"
    ]
  },
"private": true,
  "dependencies": {
    "tslib": "^2.0.0"
  },
```

- finally, remix app package.json needs a version, private true and name as well as @remix-run/serve dependency (the dependency should come standard with the install)

```
/* ./apps/remix/package.json */
{
 "private": true,
  "name": "remix",
  "description": "",
  "license": "",
  "version": "0.0.1",
"dependencies": {
"@remix-run/serve": "^1.1.1",
...
}
```
