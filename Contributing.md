We need your help to make this the best terrain plugin for Godot.

Please review the [roadmap](https://github.com/users/outobugi/projects/1) for priority of issues.

# PR Workflow

We use the standard [Godot PR workflow](https://docs.godotengine.org/en/stable/contributing/workflow/pr_workflow.html). Please submit PRs according to the same process Godot uses.

# Code Style

## GDScript

In general, follow the [Godot GDScript style guidelines](https://docs.godotengine.org/en/stable/tutorials/scripting/gdscript/gdscript_styleguide.html). 
In addition:
* All variables and functions are static typed, with a colon then space (eg. `var state: int = 3`)
* Auto static typing can be used *only* when the type is specifically assigned (eg. `var point := Vector2(1, 1)`
* Two blank lines between functions

## C++

In general, follow the [Godot C++ style guidelines](https://docs.godotengine.org/en/latest/contributing/development/code_style_guidelines.html).
In addition:
* One blank line between functions
* Everything braced - no if/for one-liners
* Opening brace on the initial line (eg. `if (condition) {`), and ending brace at the same tab stop as the initial line.
* Private variables/functions prefaced with `_`
* Private/public/protected explicit and grouped together
* Functions in h and cpp files in same order
* All code passed through clang-format. See below.

### clang-format

clang-format will adjust the style of your code to a consistent standard. Once you install it you can manually run it on all of your code to see or apply changes, and you can set it up to run automatically upon each commit.

#### Installing clang-format binary onto your system.
* Download version 13 or later
* Make sure the LLVM binary directory where `clang-format` is stored gets added to the `PATH` during installation
* Linux/OSX: Install the `clang-format` package, or all of `LLVM` or `clang` if your distribution doesn't provide the standalone tool
* Windows: Download LLVM for Windows from <https://releases.llvm.org/download.html>

#### Using clang-format automatically

We use Godot's clang-format hooks that will format your code upon making a commit. Install the hooks into your repo after cloning.

* Copy `tools/hooks/*` into `.git/hooks` or run `python tools/install-hooks.py`

#### Using clang-format manually

* View a formatted file, no changes on disk: `clang-format <filenames>`
* See what changes would be made: `git-clang-format --diff <filenames>`
* Change the files in place: `clang-format -i <filenames>`

