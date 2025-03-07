---
title: Changelog
type: documentation
redirect_from:
  - /changelog/
  - /changelog
---

# Changelog

## 15.2 - 2022-03-04

* Added `t.hasProp`, `t.hasProps`, `t.hasOwnProp`, and `t.hasOwnProps`
* Made it possible to split snapshot output by setting `t.snapshotFile`
* Parser:
    * more correct handling of `#` and `\` characters
    * ensure that test point IDs are not repeated
    * catch invalid test point IDs when a trailing plan is used
    * pragmas keys can contain uppercase characters, numbers, `-` and `_`
    * treat tests with directives like `# skipped` as `skip` tests, as
      TAP13 specifies

## 15.1 - 2021-11-16

Updated treport to use react 17, new versions of ink and other internal
deps, so that tap could work on node v17 and get quiet some security
advisory nags.

## 15.0 - 2021-03-30

This is a major refactor of much of tap's internals, and a lot of new
features.

### BREAKING CHANGES

* Drop the use of the `@std/esm` module, in favor of native ES Modules.
* Drop the inclusion of `typescript` by default.  (Typescript still
  supported, but requires that you install it yourself.)
* `.jsx` files only run automatically when `--jsx` config is explicitly
  enabled.
* `--check-coverage` on by default.
* Drop support for node `<10`.
* Separate `t.has` from `t.match`, so these are distinct.
* Deprecate aliases.
* Do not report on test points filtered with `only` or `grep` options.
* Resolve `t.test()` promise to the child test results, rather than the
  parent test.
* Remove `callback` argument from `t.beforeEach` and `t.afterEach`. Return
  a promise if you wish these methods to be async.

### NEW FEATURES and BUG FIXES

* Restructure snapshot output folder, and change file extensions to `.cjs`.
* Add `t.compareOptions` object to pass options to all the methods that use
  `tcompare` (ie, `t.has`, `t.match`, `t.same`, etc.)
* Improved diffing and comparison output for long strings and buffers.
* Add `t.before` method.
* Add `t.mock()` API for mocking calls to `require()` in modules being
  tested.
* Inherit the `t.saveFixture` boolean option.
* Create fixtures symbolic links as junctions if pointing at directories.
* Set both `FORCE_COLOR` and `NO_COLOR` environment variables
  appropriately.
* Pull initial `TS_NODE_COMPILER_OPTIONS` from test environment.
* Run fixture cleanup aysnchronously on `t.teardown()` to minimize Windows
  folder locking issues.
* Load `.taprc.yml` and `.taprc.yaml` config files if present, and no
  `.taprc` is present.

### DEPENDENCIES and REFACTORING

* Extract most of the internal functionality to
  [`libtap`](https://npm.im/libtap).
* Update `nyc` to version 15.
* Conditional exports to limit diving into tap's internals except via
  supported APIs.

## 14.10 - 2019-11-20

* Fragment large diffs with `@@ ... @@` sections to only show the relevant
  bits, making large object diffs much more manageable.
* Allow iterables to be matched against arrays, and vice versa, and each
  other, without treating their entries as `undefined`.
* Exit with a yaml parse error on badly formatted rc files.

## 14.9 - 2019-10-30

* Add `--before` and `--after` options to the CLI.

## 14.8 - 2019-10-20

* Update the default `--test-regex` config so that a top-level `test.js` or
  `tests.js` file will be included.
* Add [`t.hasStrict()`](/docs/api/#thasstrict) method.

## 14.7 - 2019-10-14

* Add the [`t.testdir()`](/docs/api/#ttestdirfixtures) and
  [`t.fixture()`](/docs/api/#tfixturetype-content) methods.  See [testing
  with fixtures](/docs/api/fixtures/).
* Capture the stack trace more helpfully in "subtest after end" errors.
* Expose timing info on all test objects
* Expose error origin on `t.error()` meta info so it can be shown in report
  output.
* Always exclude test files from NYC coverage

## 14.6 - 2019-08-03

* Add the `--no-coverage-map` config flag to turn off a previously-set
  `coverage-map` config.
* Friendlier output on invalid argument assertion failures from the tap
  CLI.

## 14.5 - 2019-07-28

* Support [`t.formatSnapshot`](/docs/api/#tformatsnapshot--function)
  returning a non-string value.

## 14.4 - 2019-07-02

* Add the `cls` repl command to clear the screen
* Consistently output repl process statuses in YAML rather than
  `util.inspect`.

## 14.3 - 2019-06-25

* Update the test runner's timeout value when a child process calls
  `t.setTinmeout(n)` on the top-level tap object.

## 14.2 - 2019-05-28

* Add the `--flow` tag to automatically strip [flow
  types](https://flow.org/) from test files.

## 14.1 - 2019-05-20

* Add support for handling config aliases like `--100` in .taprc and
  package.json files.  So, you can put `"tap": { "100": true, "B": true }`
  in a package.json file and it will parse the option names as it would on
  the command line.

## 14.0 - 2019-05-17

* Add the `--ts` and `--jsx` flags to control whether or not tap's built-in
  TypeScript and JSX parsing should be used.
* Make `--coverage-report` a list option, which can be set multiple times
  to run multiple NYC coverage reports.
* Add support for `*.cjs` as a JavaScript file extension.
* Only parse stdin when `-` is explicitly set as a command line option,
  regardless of whether stdin is a TTY or not.

## 13.1 - 2019-04-28

* Add [repl](/docs/watch/) for controlling `--watch` behavior.
* Add `t.cleanSnapshot` and `t.formatSnapshot` for customizing snapshot
  formatting.

## 13.0 - 2019-04-25

Faster, prettier, and more powerful.  Major enhancements and quite a few
breaking changes.  Most tests should continue to work fine, it's worth reading
through this changelog if you use previous versions of tap more than casually.

### Reporting

The [reporting engine](/docs/reporting/) has gotten a massive overhaul.

* Brand new reporter [treport](http://npm.im/treport), built using React and
  [ink](http://npm.im/ink), which reports on parallel tests in progress, and
  features code highlighting with [cardinal](http://npm.im/cardinal),
  beautifully accessible diffs, more signal and less noise all around.
* Add source context when showing the source line in errors.
* Prettier formats for snapshot files, and diffs for all matchers, using
  [tcompare](http://npm.im/tcompare)
* Support for passing a module name program to the command line, so `tap -R
  my-reporter-module` works, whether that is a CLI program, a stream module, or
  a treport-style React component.

### API Updates

* The `t.expectUncaughtException()` method, for testing that expected uncaught
  exceptions are thrown.
* Add the test object as a second argument to `t.beforeEach` and `t.afterEach`
  handlers.
* Add `t.context` object which inherits from its parent test.
* `t.throws()` returns the thrown error on success.
* Add `t.resolveMatchSnapshot()`, and do not clutter up promise
  resolving/rejecting assertion output with an extra subtest.
* `t.teardown(fn)` handler functions can return a Promise to perform
  asynchronous actions.
* Errors thrown in `t.beforeEach()` functions will no longer abort the entire
  test process.

### CLI and Runner Changes

* Add `--changed` (or `-n`) to only run test files where the test file or one
  of the covered files have been updated since the last test run.
* Add `--watch` (or `-w`) to watch test files and program for changes, running
  relevant tests on each update.
* Support the `--show-process-tree` to have [NYC](http://npm.im/nyc) show a
  process tree.
* Load a default set of files (instead of waiting on stdin) if `tap` is invoked
  with no arguments and `stdin` is a TTY.
* Create snapshots with the `--snapshot` flag, or by naming an npm test `snap`
  or `snapshot`.  So you can have `"scripts":{"test":"tap","snap":"tap"}` in
  package.json, and it'll do the right thing in both cases.
* Add `--test-regex` and `--test-ignore` options to control which files are
  loaded by default if no args are provided.  (Note that `node_modules` and
  `.git` are always excluded by the default file lookup.)
* Add `--test-env=key=value` option to set (or remove) environment variables in
  tests.
* Default to `--jobs-auto` style parallelization, where the number of parallel
  jobs defaults to the number of CPUs.
* Reorganized CLI usage output and argument parsing, using
  [jackspeak](http://npm.im/jackspeak)
* Sort and present filenames more cleanly in runner.
* Add support for running typescript on Windows.
* Automatically load `.jsx` and `.tsx` files, using
  [import-jsx](http://npm.im/import-jsx) and TypeScript's built-in JSX
  capabilities.
* "Run" `.tap` files by catting them.

### Coverage Related Things

* Default to [coverage](/docs/coverage/) being turned on.  (Defaulting to
  `check-coverage` at 100% will come in v14.)
* Add support for [coverage maps](/docs/coverage/coverage-map/) for
  specifying which test file should cover which (or any) program file(s).

### Configuration

* Pull tap configs from `tap` object in package.json
* Load `.taprc` file from the current working directory, not from `$HOME`.

### Low Level Stuff

* New YAML parser [tap-yaml](http://npm.im/tap-yaml) which uses
  [YAML](http://npm.im/yaml) and adds support for Domains, Errors, Symbols, and
  other JS-isms.
* Abandon domains in favor of
  [`async_hooks`](https://nodejs.org/api/async_hooks.html) with
  [async-hook-domain](http://npm.im/async-hook-domain) for error trapping.
* Surface the counts and lists of relevant (ie, non-child-test reporting) test
  points, for use in reporters and the like.
* Spawn: Emit `preprocess` event so extensions and reporters can tinker with
  process options.
* Implicitly end tests when they bail out.  (This was being done previously,
  but only by virtue of the fact that the root TAP object ended its children
  when it saw a bailout.)

## 12.6 2019-03-06

* Add --no-esm flag to disable '-r esm' behavior

## 12.5 2019-01-29

Add support for ES Modules in all tap test scripts using `esm`.

## 12.4 2019-01-22

Add support for loading typescript `.ts` files with `ts-node`.

## 12.3 2019-01-22

Add support for loading `.mjs` files with the experimental module
syntax flag set.

## 12.2 2019-01-22

Add `--comments` to print all `t.comment()` messages to stderr.

Add `TAP_CHILD_ID` in the environment of test scripts, so that they
can differentiate themselves when spinning up servers and such.

## 12.1 2018-11-12

Updates to make tap compatible with running in web browsers using
browserify or webpack.

## 12.0 2018-05-16

Breaking change to support deep matching and pattern matching of
objects in `Set` collections.  (Previously, `Set` contents would only
match if they were equal.)

## 11.0 2017-11-26

Significant refactoring and speed improvements.

Add [`t.skip()`](/api/#tskipname-options-function) and
[`t.todo()`](/api/#tskipname-options-function) methods.

Add
[`t.resolves(promise)`](/asserts/#tresolvespromise--fn-message-extra)
to assert that a Promise object (or function that returns a Promise)
will resolve.

Add [`t.resolveMatch(promise,
pattern)`](/asserts/#tresolvematch-promise--fn-wanted-message-extra)
to assert that a Promise object (or function that returns a Promise)
will resolve to a value matching the supplied pattern.

Add support for [snapshot testing](/snapshots/).

Improved implementation of [Mocha-like DSL](/docs/api/mochalike/)

### BREAKING CHANGES:

- Classes are true ECMAScript classes now, so constructors cannot be
  called without `new`.
- Unnamed subtests are not given the name `(unnamed test)`
- The `t.current()` method is removed

## 10.7 2017-06-24

Add support for [filtering tests using 'only'](/docs/api/only).

Don't show grep/only skips in the default reporter output.

## 10.6 2017-06-23

Add support for [filtering tests using regular expressions](/docs/api/grep).

## 10.5 2017-06-20

Add support for Maps and Sets in `t.match()`, `t.same()`, and
`t.strictSame()`.

## 10.4 2017-06-18

Add
[`t.rejects()`](/asserts/#trejectspromise--fn-expectederror-message-extra)
assertion.

## 10.3 2017-03-01

* Add `-o` `--output-file` to put the raw TAP to a file.
* Never print Domain objects in YAML.  They're humongous.
* Don't lose error messages in doesNotThrow

## 10.2 2017-02-18

Variety of minor cleanup fixes, and a debug mode.

* Respond to TAP_DEBUG and NODE_DEBUG environs
* Catch errors thrown in teardown handlers
* Improve root-level thrown error reporting
* don't let an occupied test slip past endAll
* Handle unhandledRejection as a root TAP error
* better inspect data
* If results are synthetically set, don't clobber when parser ends
* monkeypatch exit as well as reallyExit

## 10.1 2017-02-07

Added support for source maps.  Stack traces in your jsx and
coffeescript files will now be helpful!

Added the `-J` option to auto-calculate the number of cores on your
system, and run that many parallel jobs.

## 10.0 2017-01-28

Full rewrite to support [parallel tests](/docs/api/parallel-tests/).  Pass `-j4` on
[the command-line](/docs/cli/) to run 4 test files at once in parallel.

This also refactors a lot of the grimier bits of the codebase, splits
the one mega-Test class into a proper OOP hierarchy, and pulls a bunch
of reusable stuff out into modules.

Somehow, in the process, it also fixed an odd timing bug with
`beforeEach` functions that returned promises.

It truly is a luxury to have a massive pile of tests when it's time to
refactor.

The [mocha-like DSL](/docs/api/mochalike/) is now much more functional, and
documented.

Now supports passng `-T` or `--timeout=0` to the [CLI](/docs/cli/) to not
impose a timeout on tests.

## 9.0 2017-01-07

Buffered subtests!

This adds support for outputting subtests in the
[buffered](/docs/api/docs/api/subtests) format, where the summary test point _precedes_
the indented subtest output, rather than coming afterwards.

This sets the stage for parallel tests, coming in v10.  Mostly, it's
just an update to [tap-parser](http://npm.im/tap-parser), and a lot of
internal clean-up.

Update [nyc](http://npm.im/nyc) to v10, which includes some fixes for
covering newer JavaScript language features, and support for implicit
function names.

Also: remove a lot of excess noise and repetitive stack traces in yaml
diagnostics.

## 8.0 2016-10-25

Update `tmatch` to version 3.  This makes regular expressions test
against the stringified versions of the test object in all `t.match()`
methods.  It's a breaking change because it can cause tests to pass
that would have failed previously, or vice versa.  However, it is more
expected, and strongly recommended.

Handle unfinished promise-awaiting tests when the process exits.

Show yaml diagnostics for the first "missing test" failure when a plan
is not met, so that the plan can be more easily debugged.
(Diagnostics are still excluded for the additional "missing test"
failures that are generated, to reduce unnecessary noise.)

Make coverage MUCH FASTER by turning on nyc caching.

## 7.1 2016-09-06

Remove a race condition in how `Bail out!` messages got printed when
setting the "bail on failure" option in child tests.  Now, whether
it's a child process or just a nested subtest, it'll always print
`Bail out!` at the failure level, then again at the top level, with
nothing in between.

Support `{ diagnostic: false }` in the options object for failing
tests to suppress yaml diagnostics.

Diagnostics are now shown when a synthetic `timeout` failure is
generated for child test processes that ignore `SIGTERM` and must be
killed with `SIGKILL`.

## 7.0 2016-08-27

Move `# Subtest` commands to the parent level rather than the child
level, more like Perl's `Test2` family of modules.  This is more
readable for humans.

Update to version 2 of the tap parser.  (This adds support for putting
the `# Subtest` commands at the parent level.)

Support use of a `--save` and `--bail` together.  Any test files that
were skipped due to a bailout are considered "not yet passed", and so
get put in the save file.

Forcibly kill any spawned child process tests when the root test exits
the parent process, preventing zombie test processes.

Handle `SIGTERM` signals sent to the main process after the root test
has ended.  This provides more useful output in the cases where the
root test object has explicitly ended or satisfied its plan, but a
timeout still occurs because of pending event loop activity.

Prevent `for..in` loops from iterating inherited keys in many cases,
providing resilience against `Object.prototype` mutations.

Add the `--100` flag to set statements, functions, lines, and branches
to 100% coverage required.

## 6.3 2016-07-30

Let `t.doesNotThrow` take a string as the first argument.

Bump `nyc` up to version 7.

The tap `lib/` folder is excluded from all stack traces.

## 6.2 2016-07-15

Add the `--test-arg=<argument>` option.

## 6.1 2016-07-01

Add support for `{diagnostic: true}` in test and assert options, to
force a YAML diagnostic block after passing child tests and
assertions.

## 6.0 2016-06-30

Only produce output on stdout if the root TAP test object is
interacted with in some way.  Simply doing `require('tap')` no longer
prints out the minimum TAP output, which means that you can interact
with, for example, `var Test = require('tap').Test` without causing
output side effects.

Add `~/.taprc` yaml config file support.

Add the `--dump-config` command line flag to print out the config
options.

Document environment variables used.

Built-in CodeCov.io support has been removed.  If you were relying on this, you
can add `codecov` as a devDependency, and then add `"posttest": "tap
--coverage-report=lcov | codecov"` to the `scripts` section in your
package.json file.

## 5.8 2016-06-24

Make coverage piping errors non-fatal.

Clean up argument ordering logic in `t.throws()`.  This now works for
almost any ordering of arguments, which is unfortunately necessary for
historical reasons.  Additionally, you can now pass in an `Error`
class to verify the type, which would previously not work properly in
some cases.

## 5.7 2016-02-22

Report timeout errors in child test scripts much more diligently.

On Unix systems, the child process handles `SIGTERM` signals by
assuming that things are taking too long, dumping a report of all
active handles and requests in process, and exiting in error.

On Windows systems (where `SIGTERM` is always uncatchably fatal), or
if a Unix child test process doesn't exit within 1 second (causing a
fatal `SIGKILL` to be sent), the parent generates more comprehensive
output to indicate that the child test exited due to a timeout.

## 5.6 2016-02-17

Update `tmatch` to version 2.  You can now test objects by supplying
their constructor, so `t.match(x, { foo: Function, name: String })`
would verify that the object has a `name` string and a `foo` method.

## 5.5 2016-02-15

Add the `t.assertAt` and `t.assertStack` properties, to override where
an assertion was effectively called from.

## 5.4 2016-01-31

Support passing in a class to `t.throws`, rather than an Error sample
object.

## 5.3 2016-01-31

Return a `Promise` object from `t.test()`, `t.spawn()`, and
`t.stdin()`.

## 5.2 2016-01-26

Adds `t.beforeEach()` and `t.afterEach()`.

## 5.1 2016-01-16

All about the cli flags!

Support `--node-arg=...` and `--nyc-arg=...` command line flags.

Add support for coverage checking using `--statements=95` etc.

Test for executable-ness more consistently across platforms.

## 5.0 2016-01-03

Make it impossible to `try/catch` out of plan/end abuses.  Calling
`t.end()` more than once, or having a number of tests that doesn't
match the `plan()` number, is always a failure.

Push thrown errors to the front of the action queue.  This means that,
even if other things are pending, an uncaught exception or a plan/end
bug, will always take the highest priority in what gets output.

Many updates to nyc, spawn-wrap, and foreground-child, so that tap now
reliably works on Windows (and a [ci to prove
it](https://ci.appveyor.com/project/isaacs/node-tap).)

Moved into the [tapjs org](https://github.com/tapjs).

## 4.0 2015-12-30

Raise an error if `t.end()` is explicitly called more than once.  This
is a breaking change, because it can cause previously-passing tests to
fail, if they did `t.end()` in multiple places.

Support promises returned by mochalike functions.

## 3.1 2015-12-29

Support sending coverage output to both codecov.io and coveralls.io.

## 3.0 2015-12-29

Upgrade to nyc 5.  This means that `config.nyc.exclude` arrays in
`package.json` now take globs instead of regular expressions.

## 2.3 2015-11-18

Use the name of the function supplied to `t.test(fn)` as the test name
if a string name is not provided.

Better support for sparse arrays.

## 2.2 2015-10-23

Add support for Codecov.io as well as Coveralls.io.

Catch failures that come after an otherwise successful test set.

Fix timing of `t.on('end')` so that event fires *before* the next
child test is started, instead of immediately after it.

`t.throws()` can now be supplied a regexp for the expected Error
message.

## 2.1 2015-10-06

Exit in failure on root test bailout.

Support promises returned by `t.test(fn)` function.

## 2.0 2015-09-27

Update matching behavior using [tmatch](http://npm.im/tmatch).  This
is a breaking change to `t.match`, `t.similar`, `t.has`, etc., but
brings them more in line with what people epirically seem to expect
these functions to do.

Deal with pending handles left open when a child process gets a
`SIGTERM` on timeout.

Remove domains in favor of more reliable and less invasive state and
error-catching bookkeeping.

## 1.4 2015-09-02

Add `t.contains()` alias for `t.match()`.

Use `deeper` for deep object similarity testing.

Treat unfinished tests as failures.

Add support for pragmas in TAP output.

## 1.3 2015-06-23

Bind all Test methods to object.

Add `t.tearDown()`, `t.autoend()`, so that the root export is Just
Another Test Object, which just happens to be piping to stdout.

Support getting an error object in bailout()

## 1.2 2015-05-26

Better support for exit status codes.

## 1.1 2015-05-20

Add coverage using nyc.

If a `COVERALLS_REPO_TOKEN` is provided, then run tests with coverage,
and pipe to coveralls.

## 1.0 2015-05-06

Complete rewrite from 0.x.

Child tests implemented as nested TAP output, similar to Perl's
`Test::More`.

## 0.x

The 0.x versions used a "flattened" approach to child tests, which
requires some bookkeeping.

It worked, mostly, but its primary success was inspiring
[tape](http://npm.im/tape) and tap v1 and beyond.
