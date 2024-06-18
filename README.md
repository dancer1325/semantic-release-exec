# @semantic-release/exec

[**semantic-release**](https://github.com/semantic-release/semantic-release) plugin to execute custom shell commands.

[![Build Status](https://github.com/semantic-release/exec/workflows/Test/badge.svg)](https://github.com/semantic-release/exec/actions?query=workflow%3ATest+branch%3Amaster) [![npm latest version](https://img.shields.io/npm/v/@semantic-release/exec/latest.svg)](https://www.npmjs.com/package/@semantic-release/exec)
[![npm next version](https://img.shields.io/npm/v/@semantic-release/exec/next.svg)](https://www.npmjs.com/package/@semantic-release/exec)
[![npm beta version](https://img.shields.io/npm/v/@semantic-release/exec/beta.svg)](https://www.npmjs.com/package/@semantic-release/exec)


* Check [semantic-release steps](https://github.com/dancer1325/semantic-release?tab=readme-ov-file#release-steps)
* Semantic release steps / implemented by the plugin `semantic-release/exec`

| Step               | Description                                                                                                   |
|--------------------|---------------------------------------------------------------------------------------------------------------|
| `verifyConditions` | Execute a shell command -- to -- verify if the release should happen.                                         |
| `analyzeCommits`   | Execute a shell command -- to -- determine the type of release.                                               |
| `verifyRelease`    | Execute a shell command -- to -- verifying a release that was determined before and is about to be published. |
| `generateNotes`    | Execute a shell command -- to -- generate the release note.                                                   |
| `prepare`          | Execute a shell command -- to -- prepare the release.                                                         |
| `publish`          | Execute a shell command -- to -- publish the release.                                                         |
| `success`          | Execute a shell command -- to -- notify of a new release.                                                     |
| `fail`             | Execute a shell command -- to -- notify of a failed release.                                                  |

## Install

```bash
$ npm install @semantic-release/exec -D
```

## Usage

* Check [**semantic-release** configuration file](https://github.com/semantic-release/semantic-release/blob/master/docs/usage/configuration.md#configuration)
* _Example_
    ```json
    {
      "plugins": [
        "@semantic-release/commit-analyzer",
        "@semantic-release/release-notes-generator",
        ["@semantic-release/exec", {
          "verifyConditionsCmd": "./verify.sh",
          "publishCmd": "./publish.sh ${nextRelease.version} ${branch.name} ${commits.length} ${Date.now()}"
        }],
      ]
    }
    ```
  * `./verify.sh` will be executed on the [verifyConditions](https://github.com/semantic-release/semantic-release#release-steps)
  * `./publish.sh 1.0.0 master 3 870668040000` (for the release of version `1.0.0` from branch `master` with `3` commits on `August 4th, 1997 at 2:14 AM`) will be executed on the [publish](https://github.com/semantic-release/semantic-release#release-steps)

## Configuration

### Options

| Options               | Description                                                                                                                                                                                                                                                                                                              |
|-----------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `verifyConditionsCmd` | The shell command to execute during the `verifyConditions` step. See [verifyConditionsCmd](#verifyconditionscmd).                                                                                                                                                                                                        |
| `analyzeCommitsCmd`   | The shell command to execute during the `analyzeCommits` step. See [analyzeCommitsCmd](#analyzecommitscmd).                                                                                                                                                                                                              |
| `verifyReleaseCmd`    | The shell command to execute during the `verifyRelease` step. See [verifyReleaseCmd](#verifyreleasecmd).                                                                                                                                                                                                                 |
| `generateNotesCmd`    | The shell command to execute during the `generateNotes` step. See [generateNotesCmd](#generatenotescmd).                                                                                                                                                                                                                 |
| `prepareCmd`          | The shell command to execute during the `prepare` step. See [prepareCmd](#preparecmd).                                                                                                                                                                                                                                   |
| `addChannelCmd`       | The shell command to execute during the `addChannel` step. See [addChannelCmd](#addchannelcmd).                                                                                                                                                                                                                          |
| `publishCmd`          | The shell command to execute during the `publish` step. See [publishCmd](#publishcmd).                                                                                                                                                                                                                                   |
| `successCmd`          | The shell command to execute during the `success` step. See [successCmd](#successcmd).                                                                                                                                                                                                                                   |
| `failCmd`             | The shell command to execute during the `fail` step. See [failCmd](#failcmd).                                                                                                                                                                                                                                            |
| `shell`               | The shell to use to run the command. See [execa#shell](https://github.com/sindresorhus/execa#shell).                                                                                                                                                                                                                     |
| `execCwd`             | The path to use as current working directory when executing the shell commands. Relative to the path from which **semantic-release** is running. _Example:_ if **semantic-release** runs from `/my-project` and `execCwd` is set to `buildScripts` then the shell command will be executed from `/my-project/buildScripts` |

* Shell commands are generated with [Lodash template](https://lodash.com/docs#template)
  * All the objects passed to the [semantic-release plugins](https://github.com/semantic-release/semantic-release#plugins) -- are available as -- template options.

## verifyConditionsCmd

* Shell command to verify if the release should happen.

| Command property | Description                                                              |
|------------------|--------------------------------------------------------------------------|
| `exit code`      | `0` if the verification is successful, or any other exit code otherwise. |
| `stdout`         | Write only the reason for the verification to fail.                      |
| `stderr`         | Common uses for logging.                                                 |

## analyzeCommitsCmd

| Command property | Description                                                                                                                                                |
|------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `exit code`      | If !=  `0` == unexpected error, -> `semantic-release` execution is stopped with an error.                                                                  |
| `stdout`         | Only the release type (`major`, `minor` or `patch` etc..) can be written to `stdout`. If no release has to be done the command must not write to `stdout`. |
| `stderr`         | Common uses for logging.                                                                                                                                   |

## verifyReleaseCmd

| Command property | Description                                                              |
|------------------|--------------------------------------------------------------------------|
| `exit code`      | `0` if the verification is successful, or any other exit code otherwise. |
| `stdout`         | Only the reason for the verification to fail can be written to `stdout`. |
| `stderr`         | Common uses for logging.                                                 |

## generateNotesCmd

| Command property | Description                                                                                                         |
|------------------|---------------------------------------------------------------------------------------------------------------------|
| `exit code`      | If !=  `0` == unexpected error, -> `semantic-release` execution is stopped with an error. |
| `stdout`         | Only the release note must be written to `stdout`.                                                                  |
| `stderr`         | Common uses for logging.                                                                                            |

## prepareCmd

| Command property | Description                                                                                                         |
|------------------|---------------------------------------------------------------------------------------------------------------------|
| `exit code`      | If !=  `0` == unexpected error, -> `semantic-release` execution is stopped with an error. |
| `stdout`         | Can be used for logging.                                                                                            |
| `stderr`         | Common uses for logging.                                                                                            |

## addChannelCmd

| Command property | Description                                                                                                                                                                                                                                    |
|------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `exit code`      | If !=  `0` == unexpected error, -> `semantic-release` execution is stopped with an error.                                                                                                                            |
| `stdout`         | The `release` information can be written to `stdout` as parseable JSON (_Example_ `{"name": "Release name", "url": "http://url/release/1.0.0"}`). If the command write non parseable JSON to `stdout` no `release` information will be returned. |
| `stderr`         | Common uses for logging.                                                                                                                                                                                                                       |

## publishCmd

| Command property | Description                                                                                                                                                                                                                                        |
|------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `exit code`      | If !=  `0` == unexpected error, -> `semantic-release` execution is stopped with an error.                                                                                                                                |
| `stdout`         | The `release` information can be written to `stdout` as parseable JSON (_Example_ `{"name": "Release name", "url": "http://url/release/1.0.0"}`). If the command write non parseable JSON to `stdout` no `release` information will be returned. |
| `stderr`         | Common uses for logging.                                                                                                                                                                                                                           |

## successCmd

| Command property | Description                                                                                                         |
|------------------|---------------------------------------------------------------------------------------------------------------------|
| `exit code`      | If !=  `0` == unexpected error, -> `semantic-release` execution is stopped with an error. |
| `stdout`         | Common uses for logging.                                                                                            |
| `stderr`         | Common uses for logging.                                                                                            |

## failCmd

| Command property | Description                                                                                                         |
|------------------|---------------------------------------------------------------------------------------------------------------------|
| `exit code`      | If !=  `0` == unexpected error, -> `semantic-release` execution is stopped with an error. |
| `stdout`         | Common uses for logging.                                                                                            |
| `stderr`         | Common uses for logging.                                                                                             |
