# GitHub Action for Android Lint

[![StepSecurity Maintained Action](https://raw.githubusercontent.com/step-security/maintained-actions-assets/main/assets/maintained-action-banner.png)](https://docs.stepsecurity.io/actions/stepsecurity-maintained-actions)

![CI](https://github.com/step-security/action-android-lint/actions/workflows/ci.yml/badge.svg)

This Action generates annotations from
[Android Lint](https://developer.android.com/studio/write/lint) Report XML.

## Usage

An example workflow(.github/workflows/android-lint.yml) to executing Android
Lint follows:

```yml
name: AndroidLint

on:
  pull_request:
    paths:
      - .github/workflows/android-lint.yml
      - '*/src/**'
      - gradle/**
      - '**.gradle'
      - gradle.properties
      - gradlew*

jobs:
  android-lint:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v7
        with:
          fetch-depth: 1
      - name: set up JDK
        uses: actions/setup-java@v5
        with:
          distribution: jetbrains
          java-version: 21
          cache: gradle
      - run: ./gradlew lint
      - uses: step-security/action-android-lint@v5
        with:
          report-path: build/reports/*.xml # Support glob patterns by https://www.npmjs.com/package/@actions/glob
          ignore-warnings: true # Ignore Lint Warnings
        continue-on-error: false # If annotations contain error of severity, action-android-lint exit 1.
```

## License

action-android-lint is available under the MIT license. See
[the LICENSE file](./LICENSE) for more info.
