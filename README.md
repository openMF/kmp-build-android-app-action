# Build Android App GitHub Action

## Overview

This GitHub Action is designed to build an Android application in debug mode using Gradle. It provides a streamlined process for compiling the Android project, with built-in caching to improve build performance.

## Features

- Sets up Java 17 development environment using Zulu OpenJDK
- Configures Gradle build system
- Implements caching to speed up subsequent builds
- Builds specified Android module in debug configuration
- Uploads generated APK as a workflow artifact

## Inputs

### `android_package_name`
- **Description**: Name of the Android project module to build
- **Required**: Yes
- **Type**: String

## Usage Example

```yaml
jobs:
  build-android-app:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: openMF/kmp-build-android-app-action@v1.0.0
        with:
          android_package_name: ':app'
```

## Build Process

The action performs the following steps:

1. Set up Java 17 development environment using Zulu OpenJDK
2. Configure Gradle build system
3. Cache Gradle dependencies and build outputs
4. Build the specified Android module in debug mode using `./gradlew :module_name:assembleDebug`
5. Upload generated APK as a workflow artifact

## Prerequisites

- Android project using Gradle as the build system
- Gradle wrapper configured in the project
- Clearly defined Android module name

## Recommendations

- Use the fully qualified module name (e.g., `:app` for the root app module)
- Ensure your Gradle build scripts are correctly configured
- Verify the module name matches exactly with your project structure

## Troubleshooting

- Confirm Java 17 compatibility with your Android project
- Check Gradle configuration and module naming
- Review GitHub Actions logs for specific build errors
- Verify that the specified module exists in your project

