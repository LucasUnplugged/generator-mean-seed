Version numbers correspond to `package.json` version.

Versioning: See [http://semver.org/](http://semver.org/)
Development (WIP - Work In Progress) or small fixes versions (in-between full versions) are typically `*-x` versions. This allows rapid iteration and you can pull in updates (by updating your generator with `npm install -g generator-mean-seed`) as often as you like. Or you can just wait for the final release of the version.
The dashed `*-x` numbers PRECEDE the new version release (i.e. v1.0.9-1 is AFTER v1.0.8 but BEFORE v1.0.9)

Since this is a generator project, these changes refer to BOTH the generator itself AND to generated code so changes are SEPARATED and indicated where possible so people can find the changes that actually affect them (i.e. changes to the generator itself don't affect people just using the GENERATED code - end developers who aren't contributors to the generator just need to know about changes to the generators they're using).

Conventions / labels:

- have '**Internal**' (for interal changes that don't affect the end-generated code) and '**Generators**' sub-sections.
	- within the 'Generators' sub-sections, use brackets at the end to list the generators that were affected, i.e. [core-default, ng-route].
		- use '**[_all]**' for changes that affect ALL generators.
		- in general, try to order changes from most to least in terms of the number and size of how many generators were affected (i.e. put '_all' and 'core-*' changes at the top).


# 1.0.10 (WIP - 1.0.10-3)
## Features
### Generators
- add `vanityPortString` `config.json` optional key for forming publicPath if do not want to show/use a port (or want to use a different one) [core-default, core-scss]
- set the generated email (for social logins) to '@_fakeemail_.com' to easily differentiate it from real emails [core-default, core-scss]
- add Facebook and Twitter image profile pulling [core-default, core-scss]
	- set new `pull_pic` parameter to 0 to skip pulling images as the default is now to pull them and save images locally / on the server
- pull name from Twitter [core-default, core-scss]
- add Facebook and Twitter social sharing [core-default, core-scss]
	- update `config.json` to include `facebook.appSecret` and 'publish_actions' in `facebook.scope`
- support SSL CA certs in `config.json` [core-default, core-scss]


# 1.0.9 (2014-05-07)
## Features
### Internal
- fix versioning to match [semver](http://semver.org/) properly (1.0.9-3 is BEFORE 1.0.9, NOT after!)
	- updated from 1.0.8-2 to 1.0.9-3 appropriately

### Generators
- url: remove 'refresh=1' duplicate(s) [core-default, core-scss]
- nav: support icon buttons (3 total types now - text only, icon only, icon and text) [core-default, core-scss]
	- add login / logout header toggle based on logged in status
- add `node-controller` generator for easy creation of a new (CRUD) API backend route/controller  [core-default, core-scss, node-controller]
	- also have `basic` generator for just a scaffold of a backend route (to avoid copy-paste)
- add Twitter social login support (frontend and backend) and simplified button styles for better cross browser compatibility [core-default, core-scss]
- add backend auth userImport support for social login id matching - email or phone no longer required if have a social (facebook, google, twitter) id [core-default, core-scss]
- add `grunt test-backend-dev` task for quicker/dev running of backend tests [core-default, core-scss]
- update to `grunt-buildfiles` v0.3.6 to suppport `ifOpts` on `configPaths` as well as `moduleGroupsSkipPrefix` [core-default, core-scss]
- Gruntfile: replace `filePathsJsCustom` with `filePathsJsTest.karmaUnitCoverage` for karma test coverage. Also use new `testCov` variable instead of directly `cfgJson.test_coverage` so can modify coverage thresholds (i.e. per grunt task) if needed. [core-default, core-scss]
- remove `app.js` from karma coverage (since adding more routes can lower coverage below thresholds) [core-default, core-scss]
- add jQuery to (unit) TESTS only (for easier `.find()` selectors) [core-default, core-scss]
- switch backend API error to return full error object rather than just message (could be a semi-breaking change pending how you're currently using the error message returned) [core-default, core-scss]


## Breaking Changes
### Generators
- moved facebook auth to all server side [core-default, core-scss]
	- works with Chrome iOS (the FB JS SDK has a bug and does not)
	- removed angular-facebook-auth service and Facebook JS SDK - saved 160k in frontend loading size!
- rename `twitter-auth-callback` to `callback-twitter-auth` for consistency (make sure to update `config.json` and dev.twitter.com application settings accordingly) [core-default, core-scss]
- added https support [core-default, core-scss]
	- `config.json` changes
		- new `server.scheme` key for https
		- new `server.httpPort` for http port to run BOTH https & http port and redirect http to https
		- new `server.httpRedirectPort` that defaults to `server.port` but can be `false` for a no port redirect from http to https
		- socket.io now MUST listen on the same port (socketPort is default set to the same port as serverPort and should not be changed)
		- new `server.socketIOEnabled` boolean key since io.listen call moved to `server.js` and bundled with express app server / SSL credentials
	- new `optServerHttpPort` and `optTestServerHttpPort` keys in `yo-configs/default.json`
- changed class `a-div-color` to `a-block` [core-default, core-scss]
- updated to protractor v0.16.1 (from v0.10.0) [core-default, core-scss]
	- `selenium` installation directory (which has `chromedriver.exe` and `selenium-server-standalone-2.39.0.jar`) moved into `./node_modules/protractor` (instead of in the root folder)
	- to update:
		- delete `selenium` folder (from root folder)
		- run `./node_modules/protractor/bin/webdriver-manager update`
- updated Concrete CI to jackrabbitsgroup fork to fix a bug where after 50 runs the recent ones wouldn't show up anymore. NOTE: the most recent runs now show up on the BOTTOM rather than the top - eventually we may re-write the CI on top of mean-seed to allow for more robust frontend displays with Angular, etc. But for now it gets the job done.
	- to update:
		- uninstall concrete then reinstall with the new fork - follow updated instructions in [deploy.md](core-default-node/templates/docs/setup-running/deploy.md)


## Fixes
### Generators
- fix auth social login to add/update other user fields (i.e. first_name, last_name, email) [core-default, core-scss]
- socialLogin now always returns user data and sess_id (even after initial create/login) [core-default, core-scss]
- nav: header: add `{{button.classes.button}}` to all buttons, including html only buttons [core-default, core-scss]
- fix google login by updating plusone js script [core-default, core-scss]
- fix facebook login by updating facebook.all.min.js script [core-default, core-scss]
- fix IE <=9 `ie.html` redirect to work no matter the (nested) path [core-default, core-scss]



# 1.0.8 (2013-12-28)
## Features
### Internal
- CHANGELOG: add 'Internal' and 'Generator' separation sub-headers
- update unit test templates [ng-route, ng-directive, ng-service]
- add roadmap.md doc with to do items

### Generators
- LOTS of frontend karma-unit tests added - minimum coverage threshold set to 85%
- add `version.txt` file with this generator version [core-default, core-scss]
- faster development (building, testing)- add grunt watch/dev tasks (with livereload for build): `grunt dev`, `grunt dev-test`, `grunt dev-karma-cov`, `grunt dev-build` for faster/auto building and testing during development [core-default, core-scss]
	- switch to keeping selenium server (and karma server) running with grunt and then just connecting to it rather than completely starting up and shutting down each time
	- karma unit tests (in dev mode) run without coverage to fix the bug that you can't see test details on the console
- fix node-coverage by adding grunt-exit task at end to ensure converage outputs and fails if below thresholds
- update Angular to 1.2.5 [core-default, core-scss]
- support multiple timeoutTrigs instances in http service so timeouts do not conflict with or clear each other if there are multiple http calls running at once [core-default, core-scss]
- add layout directive (move resize / DOM stuff from LayoutCtrl to here instead) [core-default, core-scss]
- package.json - move grunt, etc. to devDependencies [core-default, core-scss]
- add backend realtime service with socket.io for out of the box web socket / realtime [core-default, core-scss]
	- uncomment socket.io frontend index.html include
	- add `dev-test/socketio` frontend page with demo chat using socket.io
	- add `angular-socket-io` frontend bower dependency and corresponding `appSocket` service
- namespace / re-org frontend development / test pages (design, test) to `dev-test` folder [core-default, core-scss]


## Breaking Changes
### Generators
- refactor `appConfig` service to not have duplicate fields from `config.json` (so now just use `appConfig.cfgJson` directly instead) [core-default, core-scss]
- refactor `appAuth` service to support more robust authentication checking - no more `noLoginRequired` or `loginPage`; instead use `auth` object with `loggedIn` key [core-default, core-scss]



# 1.0.7 (2013-12-02)
## Features
### Generators
- support defaults for prompts even with yo-config *.json files so they do not all have to be specified **[_all]**
- buildfiles - allow E2E tests in any directory (not just test) and distinguish unit from e2e tests with new `testUnit` (for Karma Unit tests) and `testE2E` (for Protractor end-to-end tests) keys for what files to include for each test runner **[_all]**
	- update `ng-route`, `ng-directive` and `ng-service` generators accordingly to switch to "testUnit" key
- handle core updates (after initial generation) by running core generators on a SEPARATE 'yo-[core-name]' branch so changes can be MERGED in (with git) after they're updated - otherwise couldn't really update a seed once it was already generated since re-running the generator would just either overwrite or not change the changed files - neither would work. **[core-default, core-scss]**
	- add `optGitBranch` prompt option (defaults to 'master')
- modularize / separate `nav.js` to `nav.js` and `nav-config.js` where the config file has the custom / site-specific components and pages objects. **[core-default, core-scss]**

### Internal
- add `helper-core-merge` and `helper-core-commands-init` (sub)generators	
- add `common/commands` to keep running commands DRY
- add `common/prompts` for keeping prompt extending defaults/setting DRY


## Bug Fixes
### Generators
- force karma-coverage npm package to be 0.1.2 (new 0.1.3 doesn't install on Windows) [core-default, core-scss]


# 1.0.6 (2013-11-27)
## Breaking Changes
### Generators
- rename / merge 'svc' and 'dtv' module namespace to single 'app' namespace **[core-default, core-scss]**
	- namespacing is very important to avoid conflicts but there's no reason to take up 2 separate namespaces.
- rename socialAuth directive to socialAuthBtn to not conflict with socialAuth service **[core-default, core-scss]**
	- you should remove the now outdated `app/src/modules/directives/socialAuth` folder if still there
- added `opt` prefix to all prompts for namespacing - i.e. to avoid conflicts with Yeoman properties/methods (such as npmInstall) **[_all]**
- changed prompts from skipInstall to install (from negative to positive - defaults to NOT run now), specifically: **[core-default, core-scss]**
	- `skipNpmInstall` to `optNpmInstall`
	- `skipBowerInstall` to `optBowerInstall`
	- `skipGrunt` to `optGruntQ`
	- `skipSeleniumInstall` to `optSeleniumInstall`
	
## Bug Fixes
### Generators
- add `grunt-contrib-clean` for removing coverage folders first **[core-default, core-scss]**
	- this would sometimes cause the grunt task to fail if not cleaned up first

### Internal	
- add `bower update` to optBowerInstall helper command (could cause errors otherwise if outdated bower packages)

## Features
### Internal
- add `ng-directive` (sub)generator
- add `ng-service` (sub)generator
- add SCSS support to `ng-route` generator

- add in 'core-scss' (sub)generator for using SASS/SCSS instead of LESS
- change to core generators
	- rename 'main' generator to 'core-default'
- modularize core generators to use/call frontend and backend generators to allow for greater code re-use (since LESS and SCSS both use the SAME backend code, we should not duplicate it)
	- the check in each generator checks an array for `indexOf()` now so multiple this.optSubgenerators can be active at once.
	- add in 'helper' generators:
		- `helper-commands`
		- `helper-log-next-steps`

- add `common` folder for private functions / modules to use in/share across generators to keep things modularized and DRY
	- add `buildfiles` for updating buildfilesModules.json file and use this in ng-route generator
	- add `array` for array operations

- add grunt-jasmine-node-coverage-validation and grunt-contrib-clean for jasmine node tests and coverage

- add `docs` folder with generator documentation

- rename `coverage` folder to `coverage-angular` for frontend test coverage output


# 1.0.5 (2013-11-23)
## Features
- update ng-route subgenerator to support nested (sub)folders
- style: fix buildfilesModules.json to use strict proper json syntax (i.e. have spaces after colons) for less git diff when auto-generating an updated version
- add grunt for dev (linting)
- documentation update - modularize into folders
- move protractor test specs to buildfilesModules


# 1.0.4 (2013-11-20)
## Features
- add test coverage (via Istanbul) for jasmine-node and angular karma unit tests (not yet for Protractor frontend E2E tests) and auto fail Grunt if below coverage thresholds
- self-run test server (run.js) for tests so no longer need a 2nd window/process with `node run.js config=test` to run tests; just one step now: `grunt`
	- remove grunt-wait task / npm install
- switch to Mandrill for email as default, remove email-templates (more complicated)
- add Continuous Integration failed and worked ci.js script to auto rollback to last Git commit on failure and email the person who made the commit as well as (optionally) any other emails as set in config.json in ci.emails_failed array


# 1.0.3 (2013-11-13)
## Features
- added demo and links to generated code and Continuous Integration server
- documentation update
	- deploy.md added
	- cloning.md added


# 1.0.2
## Features
- add `docs` directory to `main` subgenerator

## Bug Fixes
- removed karma.conf.js copy command as this is a grunt generated file and does not exist originally


# 1.0.1

## Features
- support Angular 1.2.0 - switch all directives from `compile` to `template` function
- add `docs` folder with documentation / readme's


# 1.0.0

## Features

## Bug Fixes

## Breaking Changes