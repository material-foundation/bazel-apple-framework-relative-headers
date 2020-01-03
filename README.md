# Bazel Apple framework relative headers

This repo provides a [Bazel](https://bazel.build/) build rule for adding framework-style import
support to Objective-C libraries. This is most useful if you prefer using `objc_library` to define
your targets, but you also use CocoaPods to import your libraries using framework-style imports.

Note: this repo was forked from the
[material-foundation/material-internationalization-ios](https://github.com/material-foundation/material-internationalization-ios/blob/94a37db3477e6de6b8c3b40e524478056b73031a/apple_framework_relative_headers.bzl)
repository.

## Usage

First, load the repo in your `WORKSPACE`:

    load("@bazel_tools//tools/build_defs/repo:git.bzl", "git_repository")

    git_repository(
        name = "bazel_apple_framework_relative_headers",
        remote = "https://github.com/material-foundation/bazel-apple-framework-relative-headers.git",
        commit = "<# SHA for a commit #>",
    )

You can then add a `apple_framework_relative_headers` dependency to any objc_library in order to add
support for framework-style imports in other Objective-C code:

    load("@bazel_apple_framework_relative_headers//:apple_framework_relative_headers.bzl", "apple_framework_relative_headers")

    objc_library(
        name = "Library",
        srcs = glob(["src/*.m"]),
        hdrs = glob(["src/*.h"]),
        sdk_frameworks = [
            "UIKit",
            "CoreGraphics",
        ],
        enable_modules = 1,
        module_name = "Library",
        visibility = ["//visibility:public"],
        deps = [
            ":LibraryFrameworkHeaders",
        ],
    )

    # Adds support for importing Library headers like so: #import <Library/Library.h>
    apple_framework_relative_headers(
        name = "LibraryFrameworkHeaders",
        hdrs = glob(["src/*.h"]),
        framework_name = "Library",
    )

## License

Licensed under the Apache 2.0 license. See LICENSE for details.
