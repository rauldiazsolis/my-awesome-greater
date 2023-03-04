# My Awesome Greater - Peky

https://itnext.io/step-by-step-building-and-publishing-an-npm-typescript-package-44fe7164964c

## NPM publish scripts

For an awesome package, we should of course automate as much as possible. We’re about to dig into some scripts in npm: prepare, prepublishOnly, preversion, version and postversion

prepare will run both BEFORE the package is packed and published, and on local npm install. Perfect for running building the code. Add this script to package.json

```
"prepare" : "npm run build"
```

prepublishOnly will run BEFORE prepare and ONLY on npm publish. Here we will run our test and lint to make sure we don’t publish bad code:

```
"prepublishOnly" : "npm test && npm run lint"
```

preversion will run before bumping a new package version. To be extra sure that we’re not bumping a version with bad code, why not run lint here as well? 😃

```
"preversion" : "npm run lint"
```

Version will run after a new version has been bumped. If your package has a git repository, like in our case, a commit and a new version-tag will be made every time you bump a new version. This command will run BEFORE the commit is made. One idea is to run the formatter here so no ugly code will pass into the new version:

```
"version" : "npm run format && git add -A src"
```

Postversion will run after the commit has been made. A perfect place for pushing the commit as well as the tag.

```
"postversion" : "git push && git push --tags"
```

This is what my scripts section in package.json looks like:

## Publish

Log in to your NPM account

```
npm login
```

Publish

```
npm publish
```

NPM package will first be built by the prepare script, then test and lint will run by the prepublishOnly script before the package is published.

## Bumping a new version

Let’s bump a new patch version of the package:

```
npm version patch
```

or

```
npm version minor
```

or

```
npm version major
```

Our preversion, version, and postversion will run, create a new tag in git and push it to our remote repository. Now publish again:

```
npm publish
```

And now you have a new version
