load("@rules_python//python:pip.bzl", "compile_pip_requirements")
load("@rules_rust//rust:defs.bzl", "rust_shared_library")

compile_pip_requirements(
    name = "requirements",
)

rust_shared_library(
    name = "word_count_rs",
    srcs = glob(["src/**/*.rs"]),
    visibility = ["//word_count:__pkg__"],
    deps = [
        "@crate_index//:pyo3",
        "@crate_index//:rayon",
        "@rules_python//python/cc:current_py_cc_libs",
    ],
)
