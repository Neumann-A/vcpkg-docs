---
title: vcpkg_from_gitlab
description: Learn how to use vcpkg_from_gitlab.
ms.date: 01/10/2024
---
# vcpkg_from_gitlab

Download and extract a project from Gitlab instances. Enables support for `install --head`.

## Usage

```cmake
vcpkg_from_gitlab(
    GITLAB_URL <https://gitlab.com>
    OUT_SOURCE_PATH <SOURCE_PATH>
    REPO <gitlab-org/gitlab-ce>
    [REF <v10.7.3>]
    [SHA512 <45d0d7f8cc350...>]
    [HEAD_REF <master>]
    [PATCHES <patch1.patch> <patch2.patch>...]
    [AUTHORIZATION_TOKEN <${SECRET_FROM_FILE}>]
    [FILE_DISAMBIGUATOR <N>]
)
```

## Parameters

### GITLAB_URL

The URL of the Gitlab instance to use.

### OUT_SOURCE_PATH

Specifies the out-variable that will contain the extracted location.

This should be set to `SOURCE_PATH` by convention.

### REPO

The organization or user plus the repository name on the Gitlab instance.

### REF

A stable git commit-ish (ideally a tag) that will not change contents. **This should not be a branch.**

For repositories without official releases, this can be set to the full commit id of the current latest master.

If `REF` is specified, `SHA512` must also be specified.

### SHA512

The SHA512 hash that should match the archive (${GITLAB_URL}/${REPO}/-/archive/${REF}/${REPO_NAME}-${REF}.tar.gz).
The REPO_NAME variable is parsed from the value of REPO.

This is most easily determined by first setting it to `0`, then trying to build the port. The error message will contain the full hash, which can be copied back into the portfile.

### HEAD_REF

The unstable git commit-ish (ideally a branch) to pull for `--head` builds.

For most projects, this should be `master`. The chosen branch should be one that is expected to be always buildable on all supported platforms.

### PATCHES

A list of patches to be applied to the extracted sources.

Relative paths are based on the port directory.

### AUTHORIZATION_TOKEN

A token to be passed via the Authorization HTTP header "PRIVATE-TOKEN".

### FILE_DISAMBIGUATOR

A token to uniquely identify the resulting filename if the SHA512 changes even though a git ref does not, to avoid stepping on the same file name.

## Notes

At least one of `REF` and `HEAD_REF` must be specified, however it is preferable for both to be present.

This exports the `VCPKG_HEAD_VERSION` variable during head builds.

## Source

[scripts/cmake/vcpkg\_from\_gitlab.cmake](https://github.com/Microsoft/vcpkg/blob/master/scripts/cmake/vcpkg_from_gitlab.cmake)
