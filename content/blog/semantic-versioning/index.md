---
title: Understanding semantic versioning with tilde (~) or caret (^) in package.json
date: "2020-08-09T22:40:32.169Z"
description: Know when to use tilde (~) or caret (^) in package.json so that it doesn't break your application
---

If you use npm to manage packages in your JavaScript application, you’re probably familiar with the package.json file.
As a JavaScript developer, understanding the basics of package.json is one of the important pieces that makes your development experience better. This file can contains a lot of meta-data about your project. But mostly it will be used for two things:

- Managing dependencies of your project
- Scripts, that helps in generating builds, running tests and other stuff in regards to your project

We see two types of dependencies

1. dependencies
2. devDependencies

### dependencies

A list of modules that this project depends upon are defined here.

### devDependencies

The modules that assist you in creating a build, testing your code, deploying your code and other development level modules.

If you look at these dependencies in your package.json file, you'll see all of the dependencies listed like this.

>  <pre> 
> "dependencies": {
>   "@angular/animations": "^7.2.16",
>   "@angular/cdk": "~7.3.7",
>   "@angular/common": "~7.2.0",
>   "@angular/compiler": "~7.2.0",
>   "@angular/core": "~7.2.0",
>   "@angular/forms": "~7.2.0",
>   "@angular/material": "^7.3.7",
>   "@angular/platform-browser": "~7.2.0",
>   "@angular/platform-browser-dynamic": "~7.2.0",
>   "@angular/router": "~7.2.0",
> },
> 
> "devDependencies": {
>   "@angular-devkit/build-angular": "~0.13.0",
>   "@angular/cli": "~7.3.4",
>   "@angular/compiler-cli": "~7.2.0",
>   "@angular/language-service": "~7.2.0",
>   "@types/node": "~8.9.4",
>   "tslint": "~5.11.0",
>   "typescript": "~3.2.2"
> }

> </pre>

We have the package name followed by the version number. But frequently, you'll also see the tilde (~) or caret (^) prefacing the version specified. The tilde or the carat aren't required. You can specify your version like this and that will satisfy the dependency for your application.

This is also going to lock in the exact version of the dependency, meaning that for **@angular/compiler** only version, **7.2.0**is going to be installed, regardless of what the latest version is. If instead I specify a tilde in front of it, it's going to lock the dependency to the specified minor version.

This means that if the package dependency updates to version 7.2.1, your node modules directory will update to that version the next time you run npm install or npm update. That's going to remain true for all patch-level upgrades, if the latest release version increments to 7.1.2, we'll get it, or 7.1.3 or 7.1.4, and so on, but it will not upgrade to version 7.2.

The carat, on the other hand, is used for locking the major version, meaning that all release versions in version 1 release cycle meet the dependency requirements. That's going to include version 7.1, version 7.2, version 7.3, and so on, but it will stop once the package version increments to version 8.0 The remaining question becomes, "Who cares?" and the answer lies within **semantic versioning**.

According to semantic versioning rules, the rightmost number is used for the patch level. This is primarily used for releasing bug fixes for the current version, and it doesn't break backwards compatibility.

The middle number, known as the minor version, is used to indicate bug fixes and new features that, again, don't break backwards compatibility and should be safe to apply.

The first number in semantic versioning indicates a major version and it indicates that there are breaking changes with previous versions, and so, it should be heavily tested before updating your application dependencies.

The caveat here is these rules of semantic versioning rules only apply if the developer of the package you're depending on uses semantic versioning rules.

### Summary

- tilde lock's the dependency to the specified minor version.
- The carat is used for locking the major version
- We can find the patch level, minor version and major version using the semnatic versioning rules

If you have any questions regarding this post or anything I should add, correct or remove, feel free to get in touch at my instagram handle [@init.js](https://www.instagram.com/init.js/)

Thanks !!!
