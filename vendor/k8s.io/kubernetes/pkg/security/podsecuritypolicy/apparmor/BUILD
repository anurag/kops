package(default_visibility = ["//visibility:public"])

licenses(["notice"])

load(
    "@io_bazel_rules_go//go:def.bzl",
    "go_library",
    "go_test",
)

go_library(
    name = "go_default_library",
    srcs = ["strategy.go"],
    tags = ["automanaged"],
    deps = [
        "//pkg/api:go_default_library",
        "//pkg/security/apparmor:go_default_library",
        "//pkg/util/maps:go_default_library",
        "//pkg/util/validation/field:go_default_library",
    ],
)

go_test(
    name = "go_default_test",
    srcs = ["strategy_test.go"],
    library = "go_default_library",
    tags = ["automanaged"],
    deps = [
        "//pkg/api:go_default_library",
        "//pkg/security/apparmor:go_default_library",
        "//pkg/util/maps:go_default_library",
        "//vendor:github.com/davecgh/go-spew/spew",
        "//vendor:github.com/stretchr/testify/assert",
    ],
)
