workspace(name = "pyo3-bazel-example")

########## Rust
load("@bazel_tools//tools/build_defs/repo:http.bzl", "http_archive")

# 0.43.0 doesn't work on bazel 6: "CARGO_MANIFEST_DIR not set"
http_archive(
    name = "rules_rust",
    integrity = "sha256-JLN47ZcAbx9wEr5Jiib4HduZATGLiDgK7oUi/fvotzU=",
    urls = ["https://github.com/bazelbuild/rules_rust/releases/download/0.42.1/rules_rust-v0.42.1.tar.gz"],
)

load("@rules_rust//rust:repositories.bzl", "rules_rust_dependencies", "rust_register_toolchains")

rules_rust_dependencies()

rust_register_toolchains(
    edition = "2021",
)

load("@rules_rust//crate_universe:repositories.bzl", "crate_universe_dependencies")

crate_universe_dependencies()

load("@rules_rust//crate_universe:defs.bzl", "crate", "crates_repository", "render_config", "splicing_config")

crates_repository(
    name = "crate_index",
    cargo_lockfile = "//:Cargo.lock",
    lockfile = "//:Cargo.Bazel.lock",
    annotations = {
        "pyo3-build-config": [crate.annotation(
            build_script_data = [
                "@python_3_11//:files",
                "@python_3_11//:python3",
            ],
            build_script_env = {
                "PYO3_PYTHON": "$(execpath @python_3_11//:python3)",
            },
        )],
    },
    packages = {
        "pyo3": crate.spec(version = "0.19.2"),
        "rayon": crate.spec(version = "1"),
        "uniffi": crate.spec(version = "0.27.3"),
    },
    splicing_config = splicing_config(resolver_version = "2"),
    render_config = render_config(
        default_package_name = ""
    ),
)

load("@crate_index//:defs.bzl", "crate_repositories")

crate_repositories()

########## Python

# 0.32.0 doesn't work on bazel 6: "undefined symbol CcInfo"
http_archive(
    name = "rules_python",
    sha256 = "c68bdc4fbec25de5b5493b8819cfc877c4ea299c0dcb15c244c5a00208cde311",
    strip_prefix = "rules_python-0.31.0",
    url = "https://github.com/bazelbuild/rules_python/releases/download/0.31.0/rules_python-0.31.0.tar.gz",
)

load("@rules_python//python:repositories.bzl", "py_repositories", "python_register_toolchains")

py_repositories()

python_register_toolchains(
    name = "python_3_11",
    python_version = "3.11",
)

load("@rules_python//python:pip.bzl", "pip_parse")

pip_parse(
    name = "pip_deps",
    requirements_lock = "//:requirements.txt",
)

load("@pip_deps//:requirements.bzl", "install_deps")

install_deps()

########## aspect_bazel_lib (copy_file)

http_archive(
    name = "aspect_bazel_lib",
    sha256 = "e3151d87910f69cf1fc88755392d7c878034a69d6499b287bcfc00b1cf9bb415",
    strip_prefix = "bazel-lib-1.32.1",
    url = "https://github.com/aspect-build/bazel-lib/releases/download/v1.32.1/bazel-lib-v1.32.1.tar.gz",
)

load("@aspect_bazel_lib//lib:repositories.bzl", "aspect_bazel_lib_dependencies")

aspect_bazel_lib_dependencies()
