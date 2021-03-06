# Primeng Sonarqube Angular 2 Starter
Angular2 starter with Continuous Integration, Jenkins DevOps, Automated Test Coverage using Sonarqube and  [Primeng](http://www.primefaces.org/primeng/#/)- Rich angular2 UI component library


[![Build Status](https://travis-ci.org/sanketsw/primeng-aungular-starter.svg)](https://travis-ci.org/sanketsw/primeng-aungular-starter)
[![Build status](https://ci.appveyor.com/api/projects/status/d5b3a9nnxnv5bxa5/branch/master?svg=true)](https://ci.appveyor.com/project/sanketsw/primeng-aungular-starter/branch/master)
[![Coverage Status](https://coveralls.io/repos/github/sanketsw/primeng-aungular-starter/badge.svg?branch=master)](https://coveralls.io/github/sanketsw/primeng-aungular-starter?branch=master)
[![Dependency Status](https://david-dm.org/sanketsw/primeng-aungular-starter.svg)](https://david-dm.org/sanketsw/primeng-aungular-starter)
[![devDependency Status](https://david-dm.org/sanketsw/primeng-aungular-starter/dev-status.svg)](https://david-dm.org/sanketsw/primeng-aungular-starter#info=devDependencies)


> Angular 2 is still in **Release Candidate** stage, please **don't** use this in production

> Follow Angular 2 Changelog [here](https://github.com/angular/angular/blob/master/CHANGELOG.md)


## Table of Content
* [Introduction](#introduction)
* [Installation](#installation)
* [Start](#start)
* [Testing](#testing)
* [Production](#production)
* [DevOps] (#DevOps) 
* [Running-on-IBM-Bluemix](#Running-on-IBM-Bluemix)
* [Extension](#extension)
* [Contributing](#contributing)
* [Special thanks](#special-thanks)

## Introduction
Welcome to Primeng Angular 2 Starter!
This starter contains almost everything you need to start developing [Angular 2](https://angular.io/):
* [NPM](https://www.npmjs.com/) for package manager
* [TypeScript](http://www.typescriptlang.org/) for the base language
  * with [Typings](https://github.com/typings/typings) for TypeScript definition manager
* [Gulp](http://gulpjs.com/) for workflow (from *serve*, *watch*, *compile*, *test* to *build*)
* [Browsersync](https://www.browsersync.io/) for development server & reload capability
* [SystemJS](https://github.com/systemjs/systemjs) for module loader
* [Codelyzer](https://github.com/mgechev/codelyzer) for static code analyzer
* [Karma](http://karma-runner.github.io/) for test-runner
* [Jasmine](http://jasmine.github.io/) for test framework
* [Istanbul](https://github.com/gotwarlost/istanbul) for test coverage
  * with [Remap Istanbul](https://github.com/SitePen/remap-istanbul) for remapping Javascript to TypeScript coverage
* [SystemJS Builder](https://github.com/systemjs/builder) or [Webpack](https://webpack.github.io/) for module bundling in production

* [Primeng](http://www.primefaces.org/primeng/#/) integration with a sample accordion on the homepage and event handling
* [Sonarqube] (http://www.sonarqube.org/) inetgartion for static code analysis using [TSLint](https://palantir.github.io/tslint/) and Code coverage of Karma Tests using [SonarTsPlugin] (https://github.com/Pablissimo/SonarTsPlugin)


## Installation
Firstly, you need to have [Node.js](https://nodejs.org/en/)
- For v4, please use v4.3.x (LTS) or higher (**highly** recommended)
- For v5, please use v5.6.x or higher, here is [why](https://nodejs.org/en/blog/vulnerability/february-2016-security-releases/)
- Ready for v6

> You need v4.x or higher for [Protractor](https://angular.github.io/protractor/#/)

Get the starter from [releases page](https://github.com/antonybudianto/angular2-starter/releases)

Then, install these packages globally:
```bash
npm install -g gulp
```

After that, go to the starter directory and just run:
```bash
npm install
```


## Start
Let's start up the server, run:
`gulp` or `gulp serve-dev`

and done! The browser will popup and you can start trying Angular 2!
Every changes to the file will refresh the browser automatically
and it'll also compile your changed TypeScripts files to Javascript files.

## Testing
This starter comes with testing gulp workflow

### Unit testing
Just run
```bash
gulp test
```
and it'll compile all TypeScript files, start Karma, then remap Istanbul coverage so that it shows TypeScript coverage, not the transpiled JavaScript coverage.

![Coverage result](http://s33.postimg.org/w7m9ckdkf/Screen_Shot_2016_06_04_at_8_15_53_AM.png)

### E2E testing
Firstly start the server:
```
gulp serve-dev
```
To begin testing, run:
```bash
gulp e2e
```
and it'll compile all E2E spec files in `/src/test/e2e/*.spec.ts`, boot up Selenium server then launches Chrome browser.

## Production
> All build tasks will run the `gulp test`, the bundle will only be created if the test passed.

You can create production build by running:
```bash
gulp build
```
or you can create production build and then serve it using Browsersync by running:
```bash
gulp serve-build
```
The starter defaults to bundle using [SystemJS Builder extension](https://github.com/ngstarter/systemjs-extension).
There is [Webpack extension](https://github.com/ngstarter/webpack-extension) available too, feel free to swap it as you like.   

Run following in case you run into issues during gulp build
```
npm install typings -g
typing install
```

## DevOps
**Jenkins integration**   
Execute Shell   
```
npm install
gulp build
sed -i -- 's/\/source/src/g' report/remap/lcov.info
```
Because lcov.info has incorrect source path, it needs to be changed   
http://stackoverflow.com/questions/37164545/how-to-upload-karma-test-report-to-sonar/38290092#38290092

**SonarQube Integration**   
sonar-project.properties in the code repo:   
```
sonar.projectKey=primeng-angular
sonar.projectName=primeng-angular
sonar.projectVersion=1.0
sonar.souceEncoding=UTF-8

sonar.ts.excludetypedefinitionfiles=true
sonar.ts.tslintconfigpath=tslint.json

sonar.sources=src/
sonar.exclusions=**/test/**/*,**/*.d.ts,**/*.spec.ts,**/*.routes.ts,**/tmp/**/*

sonar.ts.lcov.reportpath=report/remap/lcov.info
sonar.ts.tslintpath=node_modules/tslint/bin/tslint
```

In Jenkins Job: Execute SonarQube Scanner: Path to project properties: `sonar-project.properties`

**Artifactory Integration**   
Generic Artifactory Integration   
Published artifacts: `**/dist/*.zip`


## Running-on-IBM-Bluemix
On gulp build, artifact produced is `primeng-angular2-starter-1.0.0-rc.4.zip` in the dist folder. This zip file contains source code and `build` folder. Build folder are compiled, compressed html, js and css contents that are used to deploy a production level app. 

To push to bluemix execute:   
`cf push primeng.angular -p dist/primeng-angular2-starter-1.0.0-rc.4.zip `

Bluemix will use nodejs_buildpack and execute `npm install` followed `npm start`
`npm start` executes server.js and launches the html and css contetns from `build` folder

Hosted: http://primengangular.mybluemix.net/

## Extension
You can extend this starter with many extensions built by the community. Browse the extensions [here](https://github.com/ngstarter)

## Contributing
Feel free to submit a PR if there are any issues or new features, please read [this](https://github.com/antonybudianto/angular2-starter/wiki/Contributing) before

## Special thanks
* Please visit the [antonybudianto angular2-starter wiki](https://github.com/antonybudianto/angular2-starter/wiki) for more details.


