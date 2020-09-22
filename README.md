# Pyenv Rules for Bazel

*TODO: Build badges and such*

The pyenv rules integrate [pyenv] with Bazel.

These rules can be used to hermetically download and compile versions of CPython supported by [pyenv]
and [pyenv-win] for the supporting platform.

## Quickstart

Add the following to your `WORKSPACE` file:

``` bzl
load("@bazel_tools//tools/build_defs/repo:http.bzl", "http_archive")

http_archive(
    name = "dpu_rules_pyenv",
    sha256 = "aa7473eb2e410cec894a92285ffe569b5cdcd46caa1e35ec9ef7e3ee4a5b0cbd",
    strip_prefix = "rules_pyenv-0.1.3",
    urls = ["https://github.com/digital-plumbers-union/rules_pyenv/archive/v0.1.3.tar.gz"],
)

load("@dpu_rules_pyenv//pyenv:defs.bzl", "pyenv_install")

pyenv_install(
    py2 = "<The version of python2 you want to use for your project>",
    py3 = "<The version of python3 you want to use for your project>",
)
```

## Examples

`rules_pyenv` allows projects to select between hermetic versions of Python controlled by Bazel or non-hermetic versions
of Python managed by a pre-existing installation of [pyenv] or [pyenv-win] on the host machine.

Hermetic use of `pyenv` is the default, simply declare your project's [py_runtime_pair] in your `WORKSPACE` file.

``` bzl
pyenv_install(
    py2 = "2.7.17",
    py3 = "3.8.2",
)
```

Alternatively, your project can have `rules_pyenv` use the host installed version of [pyenv] or [pyenv-win] and the
versions of Python it already manages. This can be useful if you find yourself needing to clean your workspace often
and are spending a lot of time waiting for `pyenv` to compile the declared Python versions.

``` bzl
pyenv_install(
    hermetic = False,
    py2 = "2.7.17",
    py3 = "3.8.2",
)
```

## Windows

If you want to use ```rules_pyenv``` on Windows you should [enable symlinks](https://docs.bazel.build/versions/master/windows.html#enable-symlink-support)

## Using the interpreter in repository rules

The interpreters are exposed as files, so you can refer to them in a repository rule.

You can refer to the ```python2``` interpreter as ```@pyenv//:py2/python``` and to the ```python3``` interpreter as ```@pyenv//:py3/python```.

## Build problems

```pyenv``` is building Python from sources, so you need all things necessary to build Python. If you have any problems regarding this, you should see [this page](https://github.com/pyenv/pyenv/wiki/common-build-problems) first.

## Caveats

- This thing needs tests
- This thing needs build automation
- You will probably find bugs. No, you're not crazy, it's just not working right; open an issue (PRs are welcome too)
- Since the native Bazel rules only support CPython your use of `pyenv` is restricted to CPython versions 2 and 3. If
    you want additional support for other Python impl

## License

This work is [dual-licensed](LICENSE) as `Apache-2.0 OR MIT` to achieve compatibility with GPLv2 and to be friendly for
both individuals and enterprise.

`SPDX-License-Identifier: Apache-2.0 OR MIT`

[pyenv]: https://github.com/pyenv/pyenv
[pyenv-win]: https://github.com/pyenv-win/pyenv-win
[py_runtime_pair]: https://github.com/bazelbuild/rules_python/blob/4fcc24fd8a850bdab2ef2e078b1de337eea751a6/docs/python.md#py_runtime_pair
