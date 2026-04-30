# AGENTS.md

## Project Structure
- `library/` - Gradle plugin implementation (Groovy), publishes as `com.github.qq549631030:android-junk-code`
- `app/` - Demo application that consumes the plugin
- Plugin ID: `io.github.qq549631030.android-junk-code` (defined in `library/build.gradle:28`)

## Plugin Toggle
Control plugin enable/disable via `gradle.properties`:
```
PLUGIN_ENABLE = true  # or false
```
Plugin applies to `app/` only when enabled (see `app/build.gradle:3-5`)

## Key Commands
```bash
./gradlew :app:testDebugUnitTest    # Run app unit tests
./gradlew :app:connectedDebugAndroidTest  # Run instrumentation tests (requires device)
./gradlew :library:publishPlugin    # Publish plugin to Gradle portal
./gradlew :app:dexcountDebug        # Compare method counts (dexcount plugin applied)
```

## Plugin Configuration
Junk code config lives in `app/build.gradle:39-138` (`androidJunkCode` block):
- Variant-specific configs: `debug` and `release` use different settings
- Custom creators available: `packageCreator`, `activityCreator`, `classNameCreator`, `methodNameCreator`, `drawableCreator`, `stringCreator`, `keepCreator`, `proguardCreator`

## Code Generation
- Plugin generates Java files, resources, and ProGuard rules under `app/build/generated/`
- Generated code uses package base `cn.hx.plugin.ui` by default
- Resources prefixed with `junk_` to avoid conflicts

## Build Notes
- Root `build.gradle` sets Java 11 for all Android subprojects
- `library/` module uses Java 8 (see `library/build.gradle:77-79`)
- Kotlin 1.8.0, Android Gradle Plugin 7.4.2
- `local.properties` requires `sdk.dir` pointing to Android SDK
