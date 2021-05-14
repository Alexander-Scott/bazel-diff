load("@rules_java//java:defs.bzl", "java_binary", "java_library", "java_proto_library")
load("@rules_proto//proto:defs.bzl", "proto_library")

config_setting(
    name = "enable_verbose",
    values = {
        "compilation_mode": "dbg",
    },
)

java_binary(
    name = "bazel-diff",
    jvm_flags = select({
        ":enable_verbose": ["-DVERBOSE=true"],
        "//conditions:default": [],
    }),
    main_class = "com.bazel_diff.BazelDiff",
    visibility = ["//visibility:public"],
    runtime_deps = [":java-bazel-diff-lib"],
)

java_library(
    name = "java-bazel-diff-lib",
    srcs = glob(["*.java"]),
    javacopts = select({
        ":enable_verbose": ["-AVERBOSE=true"],
        "//conditions:default": [],
    }),
    visibility = ["//test/java/com/bazel_diff:__pkg__"],
    deps = [
        ":build_java_proto",
        "@bazel_diff_maven//:com_google_code_gson_gson",
        "@bazel_diff_maven//:com_google_guava_guava",
        "@bazel_diff_maven//:info_picocli_picocli",
        "@com_google_protobuf//:protobuf_java",
    ],
)

java_proto_library(
    name = "build_java_proto",
    deps = [":build_proto"],
)

proto_library(
    name = "build_proto",
    srcs = [":build_proto_gen"],
)

genrule(
    name = "build_proto_gen",
    srcs = ["@bazel_tools//src/main/protobuf:build.proto"],
    outs = ["build.proto"],
    cmd = "cp $< $@",
)
