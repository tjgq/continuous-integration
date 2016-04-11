# Copyright 2015 The Bazel Authors. All rights reserved.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#    http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

# Docker base images
load("//base:docker_pull.bzl", "docker_pull")
docker_pull(
    name = "ubuntu-wily",
    tag = "ubuntu:wily",
)

# Jenkins
load("//jenkins/base:plugins.bzl", "JENKINS_PLUGINS")
load("//jenkins/base:jenkins_base.bzl", "jenkins_base")

jenkins_base(
    name = "jenkins",
    version = "1.642.4",
    plugins = JENKINS_PLUGINS,
    volumes = ["/opt/secrets"],
)

# Docker debian deps
load("//base:debs.bzl", "docker_debs_repositories")
docker_debs_repositories()

# Releases stuff
http_file(
    name = "hoedown",
    sha256 = "779b75397043f6f6cf2ca8c8a716da58bb03ac42b1a21b83ff66b69bc60c016c",
    url = "https://github.com/hoedown/hoedown/archive/3.0.4.tar.gz",
)

http_file(
    name = "github_release",
    sha256 = "d6994f8a43aaa7c5a7c8c867fe69cfe302cd8eda0df3d371d0e69413999c83d8",
    url = "https://github.com/c4milo/github-release/archive/v1.0.7.tar.gz",
)

# Use Jinja for templating our files
new_http_archive(
    name = "markupsafe_archive",
    url = "https://pypi.python.org/packages/source/M/MarkupSafe/MarkupSafe-0.23.tar.gz#md5=f5ab3deee4c37cd6a922fb81e730da6e",
    sha256 = "a4ec1aff59b95a14b45eb2e23761a0179e98319da5a7eb76b56ea8cdc7b871c3",
    build_file_content = """
py_library(
    name = "markupsafe",
    srcs = glob(["markupsafe/*.py"]),
    srcs_version = "PY2AND3",
    visibility = ["//visibility:public"],
)
""",
    strip_prefix = "MarkupSafe-0.23",
)

new_http_archive(
    name = "jinja2",
    url = "https://pypi.python.org/packages/source/J/Jinja2/Jinja2-2.8.tar.gz#md5=edb51693fe22c53cee5403775c71a99e",
    sha256 = "bc1ff2ff88dbfacefde4ddde471d1417d3b304e8df103a7a9437d47269201bf4",
    build_file_content = """
py_library(
    name = "jinja2",
    srcs = glob(["jinja2/*.py"]),
    srcs_version = "PY2AND3",
    deps = [
        "@markupsafe_archive//:markupsafe",
    ],
    visibility = ["//visibility:public"],
)
""",
    strip_prefix = "Jinja2-2.8",
)

# Our template engine use gflags
new_git_repository(
    name = "gflags",
    remote = "https://github.com/google/python-gflags",
    tag = "python-gflags-2.0",
    build_file_content = """
py_library(
    name = "gflags",
    srcs = [
        "gflags.py",
        "gflags_validators.py",
    ],
    visibility = ["//visibility:public"],
)
""",
)
