load("@implib_so//:implib_gen.bzl", "implib_gen")
load("@rules_license//rules:license.bzl", "license")

package(
    default_applicable_licenses = [":license"],
    default_visibility = ["//visibility:public"],
)

# See https://docs.nvidia.com/deeplearning/nccl/bsd
licenses(["notice"])  # BSD 3-Clause

license(
    name = "license",
    package_name = "nccl",
    license_kinds = ["@rules_license//licenses/spdx:BSD-3-Clause"],
)

# NCCL headers-only target.
# There is no need to depend on this and :nccl.
cc_library(
    name = "nccl_header",
    hdrs = glob(["include/*.h"]),
    strip_include_prefix = "include",
    deps = ["@cuda//:cuda_headers"],
)

# This target dynamically loads the actual libnccl.so.
cc_library(
    name = "nccl",
    srcs = [":implib_gen"],
    hdrs = glob(["include/**"]),
    data = glob(["lib/*.so*"]),
    linkopts = ["-ldl"],
    linkstatic = True,
    strip_include_prefix = "include",
    deps = ["@cuda//:cuda_headers"],
)

implib_gen(
    name = "implib_gen",
    error_value = "ncclSystemError",
    header = "nccl.h",
    package = "nvidia.nccl.lib",
    shared_library = "lib/libnccl.so.2",
    visibility = ["//visibility:private"],
)
