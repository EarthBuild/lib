# lib/gradle

EarthBuild's official collection of Gradle [functions](https://docs.earthbuild.dev/docs/guides/functions).

First, import the library up in your Earthfile:
```earthfile
VERSION --global-cache 0.7
IMPORT github.com/EarthBuild/lib/gradle:<version/commit> AS gradle
```
> :warning: Due to [this issue](https://github.com/earthly/earthly/issues/3490), make sure to enable `--global-cache` in the calling Earthfile, as shown above.

## +GRADLE_GET_MOUNT_CACHE

This function sets the following entries in the calling environment, so they can be used later to parametrize two global mount caches in `RUN` commands:
- `$EARTH_GRADLE_USER_HOME_CACHE`: Code of the mount cache for the [gradle user home](https://docs.gradle.org/current/userguide/directory_layout.html#dir:gradle_user_home) directory.
- `$EARTH_GRADLE_PROJECT_CACHE`: Code of the mount cache for the [gradle project root](https://docs.gradle.org/current/userguide/directory_layout.html#dir:project_root) directory.

### Arguments
- `cache_prefix`: To be used in both caches. By default: `${EARTH_TARGET_PROJECT_NO_TAG}#${OS_RELEASE}#earth-gradle-cache`

### Example
```earthfile
DO gradle+GRADLE_GET_MOUNT_CACHE
RUN --mount=$EARTH_GRADLE_USER_HOME_CACHE --mount=$EARTH_GRADLE_PROJECT_CACHE gradle --no-daemon build
```

### Scoping

If `cache_prefix` is not set, the two global caches have an Earthfile-scope, that is, they are not shared with other Earthfiles.
Otherwise, all Earthfiles using the same `cache_prefix` will share the cache mounts.

## Example
See [earth-intellij-plugin](https://github.com/EarthBuild/earthly-intellij-plugin/blob/main/Earthfile) for a complete example.
