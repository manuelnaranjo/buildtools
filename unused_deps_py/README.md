# Unused Deps Py

unused_deps_py is a command line tool to determine any unused dependencies
in [java_library](https://docs.bazel.build/versions/master/be/java.html#java_library)
targets. It outputs `buildozer` commands to apply the suggested
prunings. It's based on [unused_deps](https://github.com/bazelbuild/buildtools/blob/master/unused_deps/README.md)
but it also adds support for [rules_jvm_external](https://github.com/bazelbuild/rules_jvm_external)
maven repositories.

## Installation

### Preferred way
```shell
$ pip install unused-deps-py
$ unused_deps_py --help
```

### Building from sources
```shell
$ bazel run //unused_deps_py -- --help
```

## Usage

```shell
unused_deps_py --workspace-path WORKSPACE_PATH TARGET...
```

Here, `TARGET` is a space-separated list of Bazel labels, with support for `:all` and `...`

## Releasing to PyPI

In order to release a new version you first need update the version number in:

1. `BUILD.bazel`
2. `main.py`

Then you need to run:

```shell
$ bazel build //unused_deps_py:unused_deps_py_wheel
$ twine upload bazel-bin/unused_deps_py/unused_deps_py-<version>-py3-none-any.whl
```

## Local manual testing

```shell
# build
bazel build //unused_deps_py/...
# store the filename of the Python Wheel in a variable
WHL="$(ls $(bazel info bazel-bin)/unused_deps_py/*.whl)"
# move to a temporary directory
pushd /tmp
# setup a virtual environment to avoid polluting your Python installation
pipenv install
# install the wheel
pipenv install $WHL
# enter the pipenv environment to do manual testing
pipenv shell
# do some manual tests
unused_deps_py -h
unused_deps_py --version
unused_deps_py --workspace-path /whatever //libs/super-lib
# exit the virtual environment
exit
# delete the virtual environment
pipenv --rm
rm Pipfile && rm Pipfile.lock
# go back to your previous directory
popd
```
