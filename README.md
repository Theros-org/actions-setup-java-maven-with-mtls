# actions-setup-java-maven-with-mtls

## Description

This GitHub Action composite action sets up Java, Maven, and configures Maven to use client mTLS authentication. It combines the functionality of the following actions:
- [actions/setup-java](https://github.com/actions/setup-java)
- [tradeshift/actions-setup-maven](https://github.com/Tradeshift/actions-setup-maven)
- [tradeshift/actions-setup-java-mtls](https://github.com/tradeshift/actions-setup-java-mtls)

By encapsulating these steps, this action streamlines the setup process for Java projects, making GitHub Actions workflows more concise and easier to manage.

## Features

- **Setup Java**: Installs the specified Java version and distribution.
- **Setup Maven**: Installs the specified Maven version.
- **Configure Maven with mTLS**: Configures Maven to use client mTLS authentication using provided certificates and settings.

## Inputs

You can check the available inputs in the [actions.yml](actions.yml) file on the root of this repo.

## Usage

Here’s a complete example workflow that uses this action:

```yaml
name: Java Build

on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main

jobs:
  build:
    runs-on: [self-hosted, ts-m-x-small-x64-docker-small]

    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Setup Java, Maven, and Configure Maven
        uses: tradeshift/setup-java-maven-with-mtls@v1
        with:
          java-version: 17
          distribution: 'zulu'
          maven-version: 3.8.6
          maven-settings: ${{ secrets.MAVEN_SETTINGS_GH_PG }}
          maven-security: ${{ secrets.MAVEN_SECURITY }}
          maven-p12: ${{ secrets.MAVEN_P12 }}
          maven-p12-password: ${{ secrets.MAVEN_P12_PASSWORD }}
          mtls-cacert: ${{ secrets.MTLS_CACERT }}

      - name: Build with Maven
        run: mvn clean install
```