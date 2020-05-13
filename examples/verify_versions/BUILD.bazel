package(default_visibility = ["//visibility:public"])

py_binary(
    name = "main",
    srcs = ["main.py"],
    python_version = "PY3",
)

py_binary(
    name = "main_py2",
    srcs = ["main.py"],
    main = "main.py",
    python_version = "PY2",
)

filegroup(
    name = "srcs",
    srcs = [
        "BUILD",
        "WORKSPACE",
        "main.py",
    ],
)
