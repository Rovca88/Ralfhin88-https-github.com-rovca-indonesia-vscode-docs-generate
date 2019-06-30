---
Order: 13
Area: python
TOCTitle: Settings Reference
ContentId: d256dc5c-95e9-4c02-a82f-947bf34a3517
PageTitle: Settings Reference for Python
DateApproved: 04/18/2019
MetaDescription: Settings Reference for the Python extension in Visual Studio Code
MetaSocialImage: images/tutorial/social.png
---
# Python settings reference

The Python Extension for Visual Studio Code is highly configurable. This page describes the key settings you can work with.

Refer to [User and workspace settings](/docs/getstarted/settings.md) to find our more about working with settings in VS Code generally.

## General settings

| Setting | Default | Description |
| --- | --- | --- |
| python.condaPath | `"conda"` | Path to the `conda` executable. |
| python.pythonPath | `"python"` | Path to the python interpreter, or the path to a folder containing the Python interpreter. Can use variables like `${workspaceFolder}` and `${workspaceFolder}/.venv`. Using a path to a folder allows anyone working with a project to create an environment in the `.venv` folder as appropriate to their operating system, rather than having to specify an exact platform-dependent path. The `settings.json` file can then be included in a source code repository. |
| python.pipenvPath | `"pipenv"` | Path to the pipenv executable to use for activation. |
| python.disableInstallationCheck | `false` | If set to `true`, disables a warning from the extension if no Python interpreter is installed. On macOS, also disables a warning that appears if you're using the OS-installed Python interpreter. It's generally recommended to install a separate interpreter on macOS. |
| python.venvPath | `""` | Path to a folder, where virtual environments are created. Depending on the virtualization tool used, it can be the project itself: `${workspaceFolder}`, or separate folder for all virtual environments located side by side: `.\envs`, `~/.virtualenvs`, and so on. |
| python.envFile | `"${workspaceFolder}/.env"` | Absolute path to a file containing environment variable definitions. See [Configuring Python environments - environment variable definitions file](/docs/python/environments.md#environment-variable-definitions-file). |
| python.globalModuleInstallation | `false` | Specifies whether to install packages for the current user only using the `--user` command-line argument (the default), or to install for all users in the global environment (when set to `true`). Ignored when using a virtual environment. For more information on the `--user`argument, see [pip - User Installs](https://pip.pypa.io/en/stable/user_guide/#user-installs). |
| python.poetryPath | `"poetry"` | Specifies the location of the [Poetry dependency manager](https://poetry.eustace.io/) executable, if installed. The default value `"poetry"` assumes the executable is in the current path. The Python extension uses this setting to install packages when Poetry is available and there's a `poetry.lock` file in the workspace folder. |
| python.terminal.launchArgs | `[]` | Launch arguments that are given to the Python interpreter when you run a file using commands such as **Python: Run Python File in Terminal**. In the `launchArgs` list, each item is a top-level command-line element that's separated by a space (quoted values that contain spaces are a single top-level element and are thus one item in the list). For example, for the arguments `--a --b --c {"value1" : 1, "value2" : 2}`, the list items should be `["--a", "--b", "--c", "{\"value1\" : 1, \"value2\" : 2}\""]`. Note that Visual Studio code ignores this setting when debugging because it instead uses arguments from your selected debugging configuration in `launch.json`. |
| python.terminal.executeInFileDir | `false` | Indicates whether to run a file in the file's directory instead of the current folder. |
| python.terminal.activateEnvironment | `true` | Indicates whether to automatically activate the environment you select using the **Python: Select Interpreter** command. For example, when this setting is `true` and you select a virtual environment, the extension automatically runs the environment's *activate* command (`source env/bin/activate` on macOS/Linux; `env\scripts\activate` on Windows). |

## Workspace symbol (tags) settings

Workspace symbols are symbols in C source code generated by the ctags tool (described on [Wikipedia](https://en.wikipedia.org/wiki/Ctags) and on [ctags.sourceforge.net](http://ctags.sourceforge.net/ctags.html)). To quote Wikipedia, ctags "generates an index (or tag) file of names found in source and header files of various programming languages." Where Python is concerned, ctags makes it easier to jump to defined functions and other symbols in C/C++ extension modules.

| Setting<br/>(python.workspaceSymbols.) | Default | Description |
| --- | --- | --- |
| tagFilePath | `"${workspaceFolder}/.vscode/tags"` | Fully qualified path to tag file (an exuberant ctag file), used to provide workspace symbols. |
| enabled | `true` | Specifies whether to enable the Workspace Symbol provider. |
| rebuildOnStart | `true` | Specifies whether to rebuild the tags file on start. |
| rebuildOnFileSave | `true` | Specifies whether to rebuild the tags file on when saving a Python file. |
| ctagsPath | `"ctags"` | Fully qualified path to the ctags executable; default value assumes it's in the current environment. |
| exclusionPatterns | `["**/site-packages/**"]` | Pattern used to exclude files and folders from ctags. |

## Code analysis settings

### IntelliSense engine settings

| Setting | Default | Description |
| --- | --- | --- |
| python.jediEnabled | `true` | Indicates whether to use Jedi as the IntelliSense engine (true) or the [Microsoft Python Language Server](https://devblogs.microsoft.com/python/introducing-the-python-language-server) (false). Note that the language server requires a platform that [supports .NET Core 2.1 or newer](https://docs.microsoft.com/dotnet/core/rid-catalog). |
| python.jediPath | `""` | Path to folder containing the Jedi library (folder should contain a `jedi` subfolder). |
| python.jediMemoryLimit | 0 | Memory limit for the Jedi completion engine in megabytes. Zero (the default) means 1024 MB. -1 disables the memory limit check. |

### Python Language Server settings

The language server settings apply when `python.jediEnabled` is `false`.

| Setting<br/>(python.analysis.) | Default | Description |
| --- | --- | --- |
| diagnosticPublishDelay | `1000` | The number of milliseconds to wait before transferring diagnostic messages to the problems list. |
| disabled<br/>errors<br/>warnings<br/>information | `[]` | List of diagnostics messages to suppress or show as errors, warnings, or information. See below for applicable values. The classification of messages affects both what's shown in the Problems window and in the editor (such as the color of the underlining). |
| logLevel | `"Error"` | Defines the types of log messages that language server writes into the Problems window, one of "Error", "Warning", "Information", and "Trace". The "Warning" level implicitly includes "Error"; "Information" implicitly includes "Warning" and "Error"; "Trace" includes all messages. |
| openFilesOnly | `true` | When true, shows only errors and warnings for open files rather than for the entire workspace. |
| symbolsHierarchyDepthLimit | `10` | Limits the depth of the symbol tree in the document outline. |
| typeshedPaths | `[]` | Paths to look for [typeshed modules](https://github.com/python/typeshed/) (github.com). |

The `disabled`, `errors`, `warnings`, and `information` settings can contain the following values:

| Value | Default type | Description or message text |
| --- | --- | --- |
| "not-callable" | Warning | (object may not be callable) |
| "undefined-variable" | Warning | (unknown variable '{0}') |
| "unresolved-import" | Warning | "Unable to resolve 'module_name'. IntelliSense may be missing for this module." |
| "too-many-function-arguments" | Warning | Too many arguments have been provided to a function call. |
| "too-many-positional-arguments-before-star" | Warning | Too many arguments have been provided before a starred argument. |
| "positional-argument-after-keyword" | Warning | A positional argument has been provided after a keyword argument. |
| "unknown-parameter-name" | Warning | The keyword argument name provided is unknown. |
| "parameter-already-specified" | Warning | A argument with this name has already been specified. |
| "parameter-missing" | Warning | A required positional argument is missing. |

To suppress the "undefined-variable" messages, for example, use the setting `"python.analysis.disabled": ["undefined-variable"]`. To suppress those messages and "too-many-function-arguments" messages as well, use the setting `"python.analysis.disabled": ["undefined-variable", "too-many-function-arguments"]`.

## AutoComplete settings

| Setting<br/>(python.autoComplete.) | Default | Description | See also |
| --- | --- | --- | --- |
| addBrackets | `false` | Specifies whether VS Code automatically adds parentheses (`()`) when autocompleting a function name. | [Editing](/docs/python/editing.md#autocomplete-and-intellisense) |
| extraPaths | `[]` | Specifies locations of additional packages for which to load autocomplete data. | [Editing](/docs/python/editing.md#autocomplete-and-intellisense) |

## Formatting settings

| Setting<br/>(python.formatting.) | Default | Description | See also |
| --- | --- | --- | --- |
| provider | `"autopep8"` | Specifies the formatter to use, either "autopep8", "black", or "yapf". |[Editing - Formatting](/docs/python/editing.md#formatting) |
| autopep8Path | `"autopep8"` | Path to autopep8 | [Editing - Formatting](/docs/python/editing.md#formatting) |
| autopep8Args| `[]` | Arguments for autopep8, where each top-level element that's separated by a space is a separate item in the list. | [Editing - Formatting](/docs/python/editing.md#formatting) |
| blackPath | `"black"` | Path to black | [Editing - Formatting](/docs/python/editing.md#formatting) |
| blackArgs| `[]` | Arguments for black, where each top-level element that's separated by a space is a separate item in the list. | [Editing - Formatting](/docs/python/editing.md#formatting) |
| yapfPath | `"yapf"` | Path to yapf | [Editing - Formatting](/docs/python/editing.md#formatting) |
| yapfArgs| `[]` | Arguments for yapf, where each top-level element that's separated by a space is a separate item in the list. | [Editing - Formatting](/docs/python/editing.md#formatting) |

## Refactoring - Sort Imports settings

| Setting<br/>(python.sortImports.) | Default | Description | See also |
| --- | --- | --- | --- |
| path | `""` | Path to isort script | [Editing - Refactoring - Sort Imports](/docs/python/editing.md#sort-imports) |
| args | `[]` | Arguments for isort, each argument as a separate item in the array. | [Editing - Refactoring - Sort Imports](/docs/python/editing.md#sort-imports) |

## Linting settings

### General

| Setting<br/>(python.linting.) | Default | Description | See also |
| --- | --- | --- | --- |
| enabled | `true` | Specifies whether to enable linting in general. | [Linting](/docs/python/linting.md) |
| lintOnSave | `true` | Specifies whether to line when saving a file. | [Linting](/docs/python/linting.md) |
| maxNumberOfProblems | `100` | Limits the number of linting messages shown. | [Linting](/docs/python/linting.md) |
| ignorePatterns | `[".vscode/*.py", "**/site-packages/**/*.py"]` | Exclude file and folder patterns. | [Linting](/docs/python/linting.md) |

### Pylint

| Setting<br/>(python.linting.) | Default | Description | See also |
| --- | --- | --- | --- |
| pylintEnabled | `true` | Specifies whether to enable Pylint. | [Linting](/docs/python/linting.md) |
| pylintArgs | `[]` | Additional arguments for Pylint, where each top-level element that's separated by a space is a separate item in the list. | [Linting](/docs/python/linting.md) |
| python.linting.pylintUseMinimalCheckers | `true` | Specifies whether to use the default value for pylintArgs. | [Linting](/docs/python/linting.md) |
| pylintPath | `"pylint"` | The path to Pylint. | [Linting](/docs/python/linting.md) |
| pylintCategorySeverity.convention | `"Information"` | Mapping for Pylint convention message to VS Code type. | [Linting](/docs/python/linting.md) |
| pylintCategorySeverity.refactor | `"Hint"` | Mapping for Pylint refactor message to VS Code type. | [Linting](/docs/python/linting.md) |
| pylintCategorySeverity.warning | `"Warning"` | Mapping for Pylint warning message to VS Code type. | [Linting](/docs/python/linting.md) |
| pylintCategorySeverity.error | `"Error"` | Mapping for Pylint error message to VS Code type. | [Linting](/docs/python/linting.md) |
| pylintCategorySeverity.fatal | `"Error"` | Mapping for Pylint fatal message to VS Code type. | [Linting](/docs/python/linting.md) |

### Pep8/pycodestyle

| Setting<br/>(python.linting.) | Default | Description | See also |
| --- | --- | --- | --- |
| pep8Enabled | `false` | Specifies whether to enable pep8. | [Linting](/docs/python/linting.md) |
| pep8Args | `[]` | Additional arguments for pep8, where each top-level element that's separated by a space is a separate item in the list. | [Linting](/docs/python/linting.md) |
| pep8Path | `"pep8"` | The path to pep8. | [Linting](/docs/python/linting.md) |
| pep8CategorySeverity.W | `"Warning"` | Mapping for pep8 W message to VS Code type.| [Linting](/docs/python/linting.md) |
| pep8CategorySeverity.E | `"Error"` | Mapping for pep8 E message to VS Code type.| [Linting](/docs/python/linting.md) |

### Flake8

| Setting<br/>(python.linting.) | Default | Description | See also |
| --- | --- | --- | --- |
| flake8Enabled | `false` | Specifies whether to enable flake8. | [Linting](/docs/python/linting.md) |
| flake8Args | `[]` | Additional arguments for flake8, where each top-level element that's separated by a space is a separate item in the list. | [Linting](/docs/python/linting.md) |
| flake8Path | `"flake8"` | The path to flake8. | [Linting](/docs/python/linting.md) |
| flake8CategorySeverity.F | `"Error"` | Mapping for flake8 F message to VS Code type.| [Linting](/docs/python/linting.md) |
| flake8CategorySeverity.E | `"Error"` | Mapping for flake8 E message to VS Code type.| [Linting](/docs/python/linting.md) |
| flake8CategorySeverity.W | `"Warning"` | Mapping for flake8 W message to VS Code type.| [Linting](/docs/python/linting.md) |

### mypy

| Setting<br/>(python.linting.) | Default | Description | See also |
| --- | --- | --- | --- |
| mypyEnabled | `false` | Specifies whether to enable mypy. | [Linting](/docs/python/linting.md) |
| mypyArgs | `["--ignore-missing-imports", "--follow-imports=silent"]` | Additional arguments for mypy, where each top-level element that's separated by a space is a separate item in the list. |[Linting](/docs/python/linting.md) |
| mypyPath | `"mypy"` | The path to mypy. | [Linting](/docs/python/linting.md) |
| mypyCategorySeverity.error | `"Error"` | Mapping for mypy error message to VS Code type. | [Linting](/docs/python/linting.md) |
| mypyCategorySeverity.note | `"Information"` | Mapping for mypy note message to VS Code type. | [Linting](/docs/python/linting.md) |

### pydocstyle

| Setting<br/>(python.linting.) | Default | Description | See also |
| --- | --- | --- | --- |
| pydocstyleEnabled | `false` | Specifies whether to enable pydocstyle. | [Linting](/docs/python/linting.md) |
| pydocstyleArgs | `[]` | Additional arguments for pydocstyle, where each top-level element that's separated by a space is a separate item in the list. | [Linting](/docs/python/linting.md) |
| pydocstylePath | `"pydocstyle"` | The path to pydocstyle. | [Linting](/docs/python/linting.md) |

### prospector

| Setting<br/>(python.linting.) | Default | Description | See also |
| --- | --- | --- | --- |
| prospectorEnabled | `false` | Specifies whether to enable prospector. | [Linting](/docs/python/linting.md) |
| prospectorArgs | `[]` | Additional arguments for prospector, where each top-level element that's separated by a space is a separate item in the list. | [Linting](/docs/python/linting.md) |
| prospectorPath | `"prospector"` | The path to prospector. | [Linting](/docs/python/linting.md) |

### pylama

| Setting<br/>(python.linting.) | Default | Description | See also |
| --- | --- | --- | --- |
| pylamaEnabled | `false` | Specifies whether to enable pylama. | [Linting](/docs/python/linting.md) |
| pylamaArgs | `[]` | Additional arguments for pylama, where each top-level element that's separated by a space is a separate item in the list.  | [Linting](/docs/python/linting.md) |
| pylamaPath | `"pylama"` | The path to pylama. | [Linting](/docs/python/linting.md) |

## Unit testing settings

### UnitTest framework

| Setting<br/>(python.testing.) | Default | Description | See also |
| --- | --- | --- | --- |
| unittestEnabled | `false` | Specifies whether UnitTest is enabled for unit testing. | [Unit testing](/docs/python/unit-testing.md)  |
| unittestArgs | `["-v", "-s", ".", "-p", "*test*.py"]` | Arguments to pass to unittest, where each top-level element that's separated by a space is a separate item in the list. | [Unit testing](/docs/python/unit-testing.md) |
| cwd | null | Specifies an optional working directory for unit tests. |
| outputWindow | `"Python Test Log"` | The window to use for unit test output. | [Unit testing](/docs/python/unit-testing.md) | [Unit testing](/docs/python/unit-testing.md)  |
| promptToConfigure | `true` | Specifies whether VS Code prompts to configure a test framework if potential tests are discovered. | [Unit testing](/docs/python/unit-testing.md)  |
| debugPort | `3000` | Port number used for debugging of UnitTest tests. | [Unit testing](/docs/python/unit-testing.md) |
  autoTestDiscoverOnSaveEnabled | `true` | Specifies whether to enable or disable auto run test discovery when saving a unit test file. |

### pytest framework

| Setting<br/>(python.testing.) | Default | Description | See also |
| --- | --- | --- | --- |
| pytestEnabled | `false` | Specifies whether pytest is enabled for unit testing. | [Unit testing](/docs/python/unit-testing.md) |
| pytestPath | `"py.test"` | Path to pytest. Use a full path if pytest is located outside the current environment. | [Unit testing](/docs/python/unit-testing.md) |
| pytestArgs | `[]` | Arguments to pass to pytest, where each top-level element that's separated by a space is a separate item in the list. When debugging unit tests with pytest-cov installed, include `--no-cov` in these arguments. | [Unit testing](/docs/python/unit-testing.md) |

### Nose framework

| Setting<br/>(python.testing.) | Default | Description | See also |
| --- | --- | --- | --- |
| nosetestsEnabled | `false` | Specifies whether Nose  is enabled for unit testing. | [Unit testing](/docs/python/unit-testing.md) |
| nosetestPath | `"nosetests"` | Path to Nose. Use a full path if pytest is located outside the current environment. | [Unit testing](/docs/python/unit-testing.md) |
| nosetestArgs | `[]` | Arguments to pass to Nose, where each top-level element that's separated by a space is a separate item in the list. | [Unit testing](/docs/python/unit-testing.md) |

## Next steps

- [Python environments](/docs/python/environments.md) - Control which Python interpreter is used for editing and debugging.
- [Editing code](/docs/python/editing.md) - Learn about autocomplete, IntelliSense, formatting, and refactoring for Python.
- [Linting](/docs/python/linting.md) - Enable, configure, and apply a variety of Python linters.
- [Debugging](/docs/python/debugging.md) - Learn to debug Python both locally and remotely.
- [Unit testing](/docs/python/unit-testing.md) - Configure unit test environments and discover, run, and debug tests.