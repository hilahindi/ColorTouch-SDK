# Publishing colortouch-sdk to Jitpack

Jitpack builds directly from GitHub — there's no separate publish step to run
locally. A release is just a pushed git tag.

## Releasing a new version

1. Bump `version` in `colortouch-sdk/build.gradle.kts` (follow
   [semver](https://semver.org/): MAJOR.MINOR.PATCH) and add an entry to
   `docs/CHANGELOG.md`.
2. Commit that change.
3. Tag the commit with the same version and push the tag:
   ```bash
   git tag v0.1.0
   git push origin v0.1.0
   ```
4. Trigger the build once by opening
   `https://jitpack.io/#hilahindi/ColorTouch-SDK/v0.1.0` in a browser —
   Jitpack builds lazily, on first request for a given tag.

## Consuming it from another project

```kotlin
// settings.gradle.kts
dependencyResolutionManagement {
    repositories {
        maven("https://jitpack.io")
    }
}
```

```kotlin
// app/build.gradle.kts
dependencies {
    implementation("com.github.hilahindi:ColorTouch-SDK:v0.1.0")
}
```

Note the coordinate is `com.github.hilahindi:ColorTouch-SDK` (repo name as
the artifactId), not `colortouch-sdk` — since `colortouch-sdk` is currently
the only module with a `maven-publish` publication, Jitpack exposes it
directly under the repo-level GAV rather than a per-module one. If a second
module is ever published too, Jitpack would switch to per-module
coordinates instead.

`jitpack.yml` at the repo root pins the build JDK to 17 (required by AGP
8.13.x).
