---
layout: post
title:  "Managing Dependencies and Versioning in JavaScript Projects with package-lock.json"
date:   2023-01-22 13:10:28 +0100
categories: JavaScript, NoeJS, Node, NPM, Package.json, Package-lock.json
desc: ""
---

Package-lock.json is a file that is created when you run `npm install` in your project. It contains a detailed record of all the packages and their dependencies that were installed, as well as the version numbers of each package. This is important because it ensures that the same versions of packages are installed on every machine that runs `npm install`, which can prevent issues caused by different versions of packages being installed.

One issue that package-lock.json addresses is the use of suffixes such as `^` and `~` in the dependencies listed in package.json. These suffixes tell npm to install a new version of the dependency when you run `npm install`, with `^` allowing for updates to new minor versions (indicated by the <strong>Mn</strong> in the version number `Mj.Mn.P`) and `~` allowing for updates to new patches (indicated by the <strong>P</strong> in the version number). Without these suffixes, npm will not update the dependency even if a new patch is available, which can cause issues if the patch includes important fixes.

### Example of package.json and package-lock.json
- Package.json
```json
{
  "name": "my-project",
  "version": "1.3.0", <!--- This is the version of the project, 1 major version, 3 is the minor versions, 0 is the patch version --->
  "dependencies": {
    "lodash": "^4.17.21", <!--- This is the version of the lodash package, 4 major version, 17 minor versions, 21 patches, the ^ means that the minor version can be updated --->
    
    "moment": "~2.29.1" <!--- This is the version of the moment package, 2 major version, 29 minor versions, 1 patch. The ~ means that the patch version can be updated --->
  }
}
```
- Existing package-lock.json

```json
{
  "name": "my-project",
  "version": "1.3.0",
  "lockfileVersion": 2,
  "requires": true,
  "dependencies": {
    "lodash": {
      "version": "4.17.21",
      ...
      ...
    },
    "moment": {
      "version": "2.29.1",
      ...
      ...
    }
  }
}
```

- Package-lock.json when running npm install (Note that the versions of the packages are different from the versions specified in package.json)

```json
{
  "name": "my-project",
  "version": "1.3.0",
  "lockfileVersion": 2,
  "requires": true,
  "dependencies": {
    "lodash": {
      "version": "4.18.21", <!--- The actual version of the lodash package that was installed after running npm install, 4.18.21 is the latest version of lodash when npm install was run --->
      ...
      ...
    },
    "moment": {
      "version": "2.29.3", <!--- The actual version of the moment package that was installed after running npm install, 2.29.3 is the latest version of moment when npm install was run --->
      ...
      ...
    }
  }
}
```

To ensure that the correct versions of packages are installed, it is recommended to use `npm ci` instead of `npm install`. `npm ci` stands for "clean install" and it will always respect the versions specified in the package-lock.json file. If package-lock.json is not present, `npm ci` will throw an error.

### Example using npm ci
- Package.json

```json
{
  "name": "my-project",
  "version": "1.3.0",
  "dependencies": {
    "lodash": "^4.17.21",
    
    "moment": "~2.29.1"
  }
}
```
- Existing package-lock.json

```json
{
  "name": "my-project",
  "version": "1.3.0",
  "lockfileVersion": 2,
  "requires": true,
  "dependencies": {
    "lodash": {
      "version": "4.17.21",
      ...
      ...
    },
    "moment": {
      "version": "2.29.1",
      ...
      ...
    }
  }
}
```
- New package-lock.json after running `npm ci` (note that the versions of lodash and moment are the same as the existing package-lock.json)

```json
{
  "name": "my-project",
  "version": "1.3.0",
  "lockfileVersion": 2,
  "requires": true,
  "dependencies": {
    "lodash": {
      "version": "4.17.21",
      ...
      ...
    },
    "moment": {
      "version": "2.29.1",
      ...
      ...
    }
  }
}
```
- If no package-lock.json is present, running `npm ci` will throw an error

> **Error**:</br>npm <span style="color:red">WARN</span> prepare removing existing node_modules/ before installation</br>npm <span style="color:red">ERR!</span> cipm can only install packages with an existing package-lock.json or npm-shrinkwrap.json with lockfileVersion >= 1. Run an install with npm@5 or later to generate it, then try again..

### Versioning convention in software engineering

In addition to the suffixes mentioned above, version numbers in software follow a convention to indicate the level of changes made to the software. The version number is typically written in the format `Mj.Mn.P`, where Mj is the major version, <strong>Mn</strong> is the minor version, and <strong>P</strong> is the patch. A change in the major version indicates significant changes to the software, such as changes to its functionalities or API. A change in the minor version indicates some functionalities have changed, and a change in the patch indicates that bug fixes have been made but no functionalities have changed.

## Conclusion
If you are working on a JavaScript project, it is recommended to use package-lock.json to ensure that the same versions of packages are installed on every machine that runs `npm install`. This can prevent issues caused by different versions of packages being installed. To ensure that the correct versions of packages are installed, it is recommended to use `npm ci` instead of `npm install`. `npm ci` stands for "clean install" and it will always respect the versions specified in the package-lock.json file. If package-lock.json is not present, `npm ci` will throw an error.



