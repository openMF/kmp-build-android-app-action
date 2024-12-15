# KMP Build Android App Action

## Overview

This GitHub Action simplifies the process of building Android applications with support for multiple flavors (Demo and Prod) across different build types (Debug and Release).

## Inputs

### `android_package_name`
- **Description**: Name of the Android project module
- **Required**: `true`
- **Type**: `string`
- **Example**: `'app'`

### `build_type`
- **Description**: Type of build to perform
- **Required**: `false`
- **Default**: `'Debug'`
- **Accepted Values**: `'Debug'`, `'Release'`

## Outputs

### `demo_apk`
- **Description**: Full path to the generated Demo APK
- **Type**: `string`
- **Example**: `./app/build/outputs/apk/demo/debug/app-demo-debug.apk`

### `prod_apk`
- **Description**: Full path to the generated Prod APK
- **Type**: `string`
- **Example**: `./app/build/outputs/apk/prod/debug/app-prod-debug.apk`

### Artifact Name
- **Description**: Name of the generated artifact
- **Type**: `string`
- **Name**: `android-app`

## Features

- Automatically sets up Java development environment
- Caches Gradle dependencies and build outputs
- Builds APKs for Demo and Prod flavors
- Uploads all generated APKs as artifacts
- Provides easy access to specific APK paths

## Usage Examples

### Basic Usage (Debug Build)
```yaml
- name: Build Android App
  uses: openMF/kmp-build-android-app-action@v1.0.0
  with:
    android_package_name: 'myapp'
```

### Specific Build Type
```yaml
- name: Build Android Release
  uses: openMF/kmp-build-android-app-action@v1.0.0
  with:
    android_package_name: 'myapp'
    build_type: 'Release'
```

### Accessing APK Paths
```yaml
- name: Build Android App
  id: build-android
  uses: openMF/kmp-build-android-app-action@v1.0.0
  with:
    android_package_name: 'myapp'

- name: Display APK Paths
  run: |
    echo "Demo APK: ${{ steps.build-android.outputs.demo_apk }}"
    echo "Prod APK: ${{ steps.build-android.outputs.prod_apk }}"
```

## Prerequisites

- Gradle project with demo and prod flavors configured
- GitHub Actions workflow environment

## Requirements

- Java 17
- Gradle 7.x or higher
- Android Gradle Plugin compatible with the project

## Notes

- The action assumes standard Android project structure
- APK paths are dynamically discovered based on build configuration
- All generated APKs are uploaded as artifacts for easy access

## Troubleshooting

- Ensure your Gradle build script correctly defines demo and prod flavors
- Verify that APK generation paths match the action's search pattern
- Check Gradle build logs for any build-related issues