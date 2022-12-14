# skeleton
Conventional commits friendly template for SOME nodejs projects.

## Build-up

## Basic project setup
Create git remote project & clone the remote repo locally.

### Initialize release-it project
```bash
$ npm init release-it
# Publish a GitHub Release with every release? Yes or 
# Where to add the release-it config? › .release-it.json
```

### Configure release-it.json and place the release-it as script in package.json
```json
# release-it.json
{
  "git": {
    "commitMessage": "chore: release v${version}",
    "changelog": "npx auto-changelog --stdout --commit-limit false -u --template https://raw.githubusercontent.com/release-it/release-it/master/templates/changelog-compact.hbs",
    "requireBranch": "main"
  },
  "hooks": {
    "after:bump": "npx auto-changelog -p"
  },
  "github": {
    "release": true
  },
  "npm": {
    "publish": false
  }
}
```

```json
# package.json
 "scripts": {
    "release": "release-it"
  }

```

### Install Dev dependencies
```bash
$ npm i -D @commitlint/{config-conventional,cli} husky auto-changelog
```

### Generate commitlint.config.js
```bash
$ echo "module.exports = { extends: ['@commitlint/config-conventional'] };" > commitlint.config.js
```

### Execute husky
```bash
$ npx husky install
```

### Generate commit-msg hook
```bash
$ npx husky add .husky/commit-msg 'npx commitlint --edit $1'
```

### Globally install commitizen & adapter
```bash
$ npm install -g commitizen cz-conventional-changelog
$ echo '{ "path": "cz-conventional-changelog" }' > ~/.czrc
```

### Commit changes
```bash
$ git add . 
$ git cz # commitizen
```

### Generate changelog and/or push to remote repo
```bash
$ npm run release
```

### Auto releases using changelog
- Create `release.yml` in __.github/workflows__ directory.
- The scripts will generate releases automatically after the code has been pushed to Github.
- Refer to this [link](https://github.com/marketplace/actions/release-with-changelog#commit-template) for the sample workflow.
