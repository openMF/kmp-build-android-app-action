# KMP Android Build Action

## Overview

A comprehensive GitHub Action for building Android applications with support for multiple flavors, build types, and secure release configurations.

## Features

- 🤖 Cross-flavor Android app building
- 🔒 Secure release configuration support
- 📦 Automatic artifact generation
- 🚀 Version code generation
- 🔑 Keystore and Google Services integration

## Usage Examples

### Debug Build
```yaml
- name: Checkout Repository
  uses: actions/checkout@v4
  with:
    fetch-depth: 0

- name: Build Android Debug
  uses: openMF/kmp-build-android-app-action@v1.0.0
  with:
    android_package_name: 'myapp'
```

### Release Build
```yaml
- name: Build Android Release
  uses: openMF/kmp-build-android-app-action@v1.0.0
  id: build-android
  with:
    android_package_name: 'myapp'
    build_type: 'Release'
    key_store: ${{ secrets.KEYSTORE_BASE64 }}
    google_services: ${{ secrets.GOOGLE_SERVICES_BASE64 }}
    key_store_password: ${{ secrets.KEYSTORE_PASSWORD }}
    key_store_alias: ${{ secrets.KEY_ALIAS }}
    key_store_alias_password: ${{ secrets.KEY_ALIAS_PASSWORD }}
    
- name: Display APK Paths
  run: |
    echo "Demo APK: ${{ steps.build-android.outputs.demo_apk }}"
    echo "Prod APK: ${{ steps.build-android.outputs.prod_apk }}"
```

## Workflow Diagram
```mermaid
flowchart TD
    A[Start Build Process] --> B[Set up Java 17]
    B --> C[Setup Gradle]
    C --> D[Cache Gradle Dependencies]
    D --> E[Generate Version Number]
    E --> F{Build Type?}
    F -->|Debug| G[Build Debug APK]
    F -->|Release| H[Inflate Secrets]
    H --> I[Build Release APK]
    G --> J[Collect APK Paths]
    I --> J
    J --> K[Upload Artifacts]
    K --> L[End Build Process]
```

## Inputs

### `android_package_name`
- **Description**: Name of the Android project module
- **Required**: `true`
- **Type**: `string`
- **Example**: `'app'`

### `build_type`
- **Description**: Type of build to perform
- **Required**: `true`
- **Default**: `'Debug'`
- **Options**:
    - `'Debug'`
    - `'Release'`

### `key_store` (Optional for Release)
- **Description**: Base64 encoded keystore file
- **Required**: `false` (Required for Release builds)
- **Type**: `string`

### `google_services` (Optional)
- **Description**: Base64 encoded google-services.json file
- **Required**: `false`
- **Type**: `string`

### Additional Security Inputs
- `key_store_password`
- `key_store_alias`
- `key_store_alias_password`

## Outputs

### `demo_apk`
- **Description**: Path to Demo flavor APK
- **Type**: `string`

### `prod_apk`
- **Description**: Path to Prod flavor APK
- **Type**: `string`

### `artifact-name`
- **Description**: Generated artifact name
- **Type**: `string`
- **Default**: `'android-app'`


## Release Configuration

For release builds, you must provide:
- Base64 encoded keystore
- Keystore password
- Keystore alias
- Keystore alias password

## Version Generation

The action automatically:
- Generates a version code based on:
    - Total git commits
    - Number of tags
- Creates a `version.txt` file
- Sets `VERSION_CODE` and `VERSION` environment variables

## Artifact Handling

- Uploads APKs for both Demo and Prod flavors
- Artifact name defaults to `android-app`
- APK paths are dynamically discovered

## Best Practices

- Use secrets for sensitive information
- Separate debug and release configurations
- Implement secure keystore management
- Use mock files for development

## Troubleshooting

- Verify keystore and Google Services configurations
- Check Gradle build scripts
- Review GitHub Actions logs
- Ensure all required secrets are configured

## Performance Optimization

- Gradle dependency caching
- Efficient version code generation
- Minimal build step overhead

## Security Considerations

- Never commit sensitive files directly
- Use GitHub secrets for confidential data
- Rotate keys and credentials regularly

## Requirements

- Java 17
- Gradle
- Android Gradle Plugin
- Configured flavors (Demo/Prod)