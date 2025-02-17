licenses(["notice"])  # Apache 2

package(default_visibility = ["//visibility:public"])

exports_files([
    "gen_sh_test_runner.sh",
    "sh_test_wrapper.sh",
    "cc_wrapper.py",
])

genrule(
    name = "gnu_build_id",
    outs = ["gnu_build_id.ldscript"],
    cmd = """
      echo --build-id=0x$$(
          grep BUILD_SCM_REVISION bazel-out/volatile-status.txt \\
        | sed 's/^BUILD_SCM_REVISION //') \\
        > $@
    """,
    # Undocumented attr to depend on workspace status files.
    # https://github.com/bazelbuild/bazel/issues/4942
    stamp = 1,
)

# For macOS, which doesn't have GNU ld's `--build-id` flag.
genrule(
    name = "raw_build_id",
    outs = ["raw_build_id.ldscript"],
    cmd = """
      grep BUILD_SCM_REVISION bazel-out/volatile-status.txt \\
    | sed 's/^BUILD_SCM_REVISION //' \\
    | tr -d '\\n' \\
    > $@
    """,
    # Undocumented attr to depend on workspace status files.
    # https://github.com/bazelbuild/bazel/issues/4942
    stamp = 1,
)

config_setting(
    name = "windows_opt_build",
    values = {
        "cpu": "x64_windows",
        "compilation_mode": "opt",
    },
)

config_setting(
    name = "windows_dbg_build",
    values = {
        "cpu": "x64_windows",
        "compilation_mode": "dbg",
    },
)

config_setting(
    name = "windows_fastbuild_build",
    values = {
        "cpu": "x64_windows",
        "compilation_mode": "fastbuild",
    },
)

config_setting(
    name = "opt_build",
    values = {"compilation_mode": "opt"},
)

config_setting(
    name = "fastbuild_build",
    values = {"compilation_mode": "fastbuild"},
)

config_setting(
    name = "dbg_build",
    values = {"compilation_mode": "dbg"},
)

config_setting(
    name = "asan_build",
    values = {"define": "ENVOY_CONFIG_ASAN=1"},
)

config_setting(
    name = "coverage_build",
    values = {"define": "ENVOY_CONFIG_COVERAGE=1"},
)

config_setting(
    name = "clang_build",
    flag_values = {
        "@bazel_tools//tools/cpp:compiler": "clang",
    },
)

config_setting(
    name = "gcc_build",
    flag_values = {
        "@bazel_tools//tools/cpp:compiler": "gcc",
    },
)

config_setting(
    name = "disable_tcmalloc",
    values = {"define": "tcmalloc=disabled"},
)

config_setting(
    name = "debug_tcmalloc",
    values = {"define": "tcmalloc=debug"},
)

config_setting(
    name = "disable_signal_trace",
    values = {"define": "signal_trace=disabled"},
)

config_setting(
    name = "disable_object_dump_on_signal_trace",
    values = {"define": "object_dump_on_signal_trace=disabled"},
)

config_setting(
    name = "disable_hot_restart",
    values = {"define": "hot_restart=disabled"},
)

# Used to avoid conflicting selects https://github.com/bazelbuild/bazel/issues/8323
alias(
    name = "disable_hot_restart_or_apple",
    actual = select({
        ":apple": ":apple",
        "//conditions:default": ":disable_hot_restart",
    }),
)

config_setting(
    name = "disable_google_grpc",
    values = {"define": "google_grpc=disabled"},
)

config_setting(
    name = "enable_path_normalization_by_default",
    values = {"define": "path_normalization_by_default=true"},
)

cc_proto_library(
    name = "grpc_health_proto",
    deps = ["@com_github_grpc_grpc//src/proto/grpc/health/v1:_health_proto_only"],
)

config_setting(
    name = "enable_exported_symbols",
    values = {"define": "exported_symbols=enabled"},
)

config_setting(
    name = "enable_log_debug_assert_in_release",
    values = {"define": "log_debug_assert_in_release=enabled"},
)

config_setting(
    name = "enable_perf_annotation",
    values = {"define": "perf_annotation=enabled"},
)

config_setting(
    name = "force_libcpp",
    values = {"define": "force_libcpp=enabled"},
)

config_setting(
    name = "boringssl_fips",
    constraint_values = [
        "@bazel_tools//platforms:linux",
        "@bazel_tools//platforms:x86_64",
    ],
    values = {"define": "boringssl=fips"},
)

config_setting(
    name = "boringssl_disabled",
    values = {"define": "boringssl=disabled"},
)

config_setting(
    name = "enable_quiche",
    values = {"define": "quiche=enabled"},
)

config_setting(
    name = "wasm_all",
    values = {"define": "wasm=enabled"},
)

config_setting(
    name = "wasm_v8",
    values = {"define": "wasm=v8"},
)

config_setting(
    name = "wasm_wavm",
    values = {"define": "wasm=wavm"},
)

# Alias pointing to the selected version of BoringSSL:
# - BoringSSL FIPS from @boringssl_fips//:ssl,
# - non-FIPS BoringSSL from @boringssl//:ssl.
alias(
    name = "boringssl",
    actual = select({
        "//bazel:boringssl_fips": "@boringssl_fips//:ssl",
        "//conditions:default": "@boringssl//:ssl",
    }),
)

# Alias pointing to a platform-specific version of wee8.
alias(
    name = "wee8",
    actual = select({
        ":darwin": "@wee8_macos//:wee8",
        ":darwin_x86_64": "@wee8_macos//:wee8",
        "//conditions:default": "@wee8_linux//:wee8",
    }),
)

config_setting(
    name = "linux_x86_64",
    values = {"cpu": "k8"},
)

config_setting(
    name = "linux_aarch64",
    values = {"cpu": "aarch64"},
)

config_setting(
    name = "linux_ppc",
    values = {"cpu": "ppc"},
)

config_setting(
    name = "linux_mips64",
    values = {"cpu": "mips64"},
)

config_setting(
    name = "windows_x86_64",
    values = {"cpu": "x64_windows"},
)

# Configuration settings to make doing selects for Apple vs non-Apple platforms
# easier. More details: https://docs.bazel.build/versions/master/configurable-attributes.html#config_settingaliasing
config_setting(
    name = "darwin",
    values = {"cpu": "darwin"},
)

config_setting(
    name = "darwin_x86_64",
    values = {"cpu": "darwin_x86_64"},
)

config_setting(
    name = "ios_i386",
    values = {"cpu": "ios_i386"},
)

config_setting(
    name = "ios_x86_64",
    values = {"cpu": "ios_x86_64"},
)

config_setting(
    name = "ios_armv7",
    values = {"cpu": "ios_armv7"},
)

config_setting(
    name = "ios_armv7s",
    values = {"cpu": "ios_armv7s"},
)

config_setting(
    name = "ios_arm64",
    values = {"cpu": "ios_arm64"},
)

config_setting(
    name = "ios_arm64e",
    values = {"cpu": "ios_arm64e"},
)

config_setting(
    name = "manual_stamp",
    values = {"define": "manual_stamp=manual_stamp"},
)

alias(
    name = "apple",
    actual = select(
        {
            ":darwin": ":darwin",
            ":darwin_x86_64": ":darwin_x86_64",
            ":ios_arm64": ":ios_arm64",
            ":ios_arm64e": ":ios_arm64e",
            ":ios_armv7": ":ios_armv7",
            ":ios_armv7s": ":ios_armv7s",
            ":ios_i386": ":ios_i386",
            ":ios_x86_64": ":ios_x86_64",
            # If we're not on an apple platform return a value that will never match in the select() statement calling this
            # since it would have already been matched above.
            "//conditions:default": ":darwin",
        },
    ),
)

alias(
    name = "linux",
    actual = select(
        {
            ":linux_x86_64": ":linux_x86_64",
            ":linux_aarch64": ":linux_aarch64",
            ":linux_ppc": ":linux_ppc",
            ":linux_mips64": ":linux_mips64",
            # If we're not on an linux platform return a value that will never match in the select() statement calling this
            # since it would have already been matched above.
            "//conditions:default": ":linux_x86_64",
        },
    ),
)

alias(
    name = "x86",
    actual = select(
        {
            ":darwin_x86_64": ":darwin_x86_64",
            ":ios_x86_64": "ios_x86_64",
            "linux_x86_64": "linux_x86_64",
            "windows_x86_64": "windows_x86_64",
            # If we're not on an x86 platform return a value that will never match in the select() statement calling this since it would have already been matched above.
            "//conditions:default": ":darwin_x86_64",
        },
    ),
)
