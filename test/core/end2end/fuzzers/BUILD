# Copyright 2016 gRPC authors.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

load("//bazel:grpc_build_system.bzl", "grpc_cc_library", "grpc_cc_proto_library", "grpc_internal_proto_library", "grpc_package")
load("//test/core/test_util:grpc_fuzzer.bzl", "grpc_fuzz_test", "grpc_proto_fuzzer")

grpc_package(name = "test/core/end2end/fuzzers")

licenses(["notice"])

grpc_internal_proto_library(
    name = "api_fuzzer_proto",
    srcs = ["api_fuzzer.proto"],
    deps = [
        "//test/core/event_engine/fuzzing_event_engine:fuzzing_event_engine_proto",
        "//test/core/test_util:fuzz_config_vars_proto",
        "//test/core/test_util:fuzzing_channel_args_proto",
    ],
)

grpc_cc_proto_library(
    name = "api_fuzzer_cc_proto",
    deps = ["api_fuzzer_proto"],
)

grpc_cc_library(
    name = "fuzzing_common",
    srcs = ["fuzzing_common.cc"],
    hdrs = ["fuzzing_common.h"],
    external_deps = [
        "absl/log:check",
        "absl/log:log",
        "absl/strings",
    ],
    deps = [
        "api_fuzzer_cc_proto",
        "//:gpr",
        "//:grpc",
        "//test/core/event_engine/fuzzing_event_engine",
    ],
)

grpc_fuzz_test(
    name = "api_fuzzer",
    srcs = ["api_fuzzer.cc"],
    external_deps = [
        "absl/log:check",
        "fuzztest",
        "fuzztest_main",
    ],
    tags = [
        "no_mac",
        "no_windows",
    ],
    deps = [
        "api_fuzzer_cc_proto",
        "fuzzing_common",
        "//:gpr",
        "//:grpc",
        "//src/core:channel_args",
        "//src/core:closure",
        "//src/core:slice",
        "//test/core/end2end:ssl_test_data",
        "//test/core/event_engine/fuzzing_event_engine",
        "//test/core/test_util:fuzz_config_vars",
        "//test/core/test_util:fuzzing_channel_args",
        "//test/core/test_util:grpc_test_util",
        "//test/core/test_util:grpc_test_util_base",
    ],
)

grpc_internal_proto_library(
    name = "fuzzer_input_proto",
    srcs = ["fuzzer_input.proto"],
    deps = [
        "api_fuzzer_proto",
        "//src/core:chaotic_good_frame_proto",
        "//test/core/event_engine/fuzzing_event_engine:fuzzing_event_engine_proto",
        "//test/core/test_util:fuzz_config_vars_proto",
        "//test/core/test_util:fuzzing_channel_args_proto",
    ],
)

grpc_cc_proto_library(
    name = "fuzzer_input_cc_proto",
    deps = ["fuzzer_input_proto"],
)

grpc_cc_library(
    name = "network_input",
    srcs = ["network_input.cc"],
    hdrs = ["network_input.h"],
    external_deps = ["absl/log:log"],
    deps = [
        "fuzzer_input_cc_proto",
        "//:chttp2_frame",
        "//src/core:chaotic_good_frame_header",
        "//test/core/event_engine/fuzzing_event_engine",
        "//test/core/test_util:fuzzing_channel_args",
        "//test/core/test_util:grpc_test_util_base",
    ],
)

grpc_proto_fuzzer(
    name = "client_fuzzer",
    srcs = ["client_fuzzer.cc"],
    corpus = "client_fuzzer_corpus",
    end2end_fuzzer = True,
    external_deps = ["absl/log:check"],
    proto = None,
    tags = ["no_windows"],
    uses_event_engine = False,
    uses_polling = False,
    deps = [
        "fuzzer_input_cc_proto",
        "fuzzing_common",
        "network_input",
        "//:gpr",
        "//:grpc",
        "//src/core:channel_args",
        "//test/core/event_engine/fuzzing_event_engine",
        "//test/core/test_util:fuzz_config_vars",
        "//test/core/test_util:grpc_test_util",
        "//test/core/test_util:grpc_test_util_base",
    ],
)

grpc_cc_library(
    name = "server_fuzzer",
    srcs = ["server_fuzzer.cc"],
    hdrs = ["server_fuzzer.h"],
    external_deps = ["absl/log:check"],
    deps = [
        "fuzzer_input_cc_proto",
        "fuzzing_common",
        "network_input",
        "//:gpr",
        "//:grpc",
        "//src/core:channel_args",
        "//test/core/event_engine/fuzzing_event_engine",
        "//test/core/test_util:fuzz_config_vars",
        "//test/core/test_util:grpc_test_util",
        "//test/core/test_util:grpc_test_util_base",
    ],
)

grpc_proto_fuzzer(
    name = "server_fuzzer_chttp2",
    srcs = ["server_fuzzer_chttp2.cc"],
    corpus = "server_fuzzer_chttp2_corpus",
    end2end_fuzzer = True,
    proto = None,
    tags = [
        "no_mac",
        "no_windows",
    ],
    uses_event_engine = False,
    uses_polling = False,
    deps = [
        ":server_fuzzer",
        "//:grpc",
    ],
)

grpc_proto_fuzzer(
    name = "server_fuzzer_chttp2_fake_creds",
    srcs = ["server_fuzzer_chttp2_fake_creds.cc"],
    corpus = "server_fuzzer_chttp2_fake_creds_corpus",
    end2end_fuzzer = True,
    proto = None,
    tags = [
        "no_mac",
        "no_windows",
    ],
    uses_event_engine = False,
    uses_polling = False,
    deps = [
        ":server_fuzzer",
        "//:grpc",
    ],
)

grpc_proto_fuzzer(
    name = "server_fuzzer_chaotic_good",
    srcs = ["server_fuzzer_chaotic_good.cc"],
    corpus = "server_fuzzer_chaotic_good_corpus",
    end2end_fuzzer = True,
    external_deps = ["absl/log:check"],
    proto = None,
    tags = [
        "no_mac",
        "no_windows",
    ],
    uses_event_engine = False,
    uses_polling = False,
    deps = [
        ":server_fuzzer",
        "//src/core:chaotic_good_server",
    ],
)

grpc_cc_library(
    name = "connector_fuzzer",
    srcs = ["connector_fuzzer.cc"],
    hdrs = ["connector_fuzzer.h"],
    external_deps = ["absl/log:check"],
    deps = [
        "fuzzer_input_cc_proto",
        "fuzzing_common",
        "network_input",
        "//:gpr",
        "//:grpc",
        "//src/core:channel_args",
        "//test/core/event_engine/fuzzing_event_engine",
        "//test/core/test_util:fuzz_config_vars",
        "//test/core/test_util:grpc_test_util",
        "//test/core/test_util:grpc_test_util_base",
    ],
)

grpc_proto_fuzzer(
    name = "connector_fuzzer_chttp2",
    srcs = ["connector_fuzzer_chttp2.cc"],
    corpus = "connector_fuzzer_chttp2_corpus",
    end2end_fuzzer = True,
    proto = None,
    tags = [
        "no_mac",
        "no_windows",
    ],
    uses_event_engine = False,
    uses_polling = False,
    deps = [
        ":connector_fuzzer",
        "//:grpc",
    ],
)

grpc_proto_fuzzer(
    name = "connector_fuzzer_chttp2_fakesec",
    srcs = ["connector_fuzzer_chttp2_fakesec.cc"],
    corpus = "connector_fuzzer_chttp2_fakesec_corpus",
    end2end_fuzzer = True,
    proto = None,
    tags = [
        "no_mac",
        "no_windows",
    ],
    uses_event_engine = False,
    uses_polling = False,
    deps = [
        ":connector_fuzzer",
        "//:grpc",
    ],
)
