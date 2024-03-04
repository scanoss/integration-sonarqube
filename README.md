# SCANOSS Sonarqube Integration Example

This repository serves as an example to demonstrate how to use the [SCANOSS Sonarqube Example Plugin](https://github.com/scanoss/scanoss-sonar-example-plugin/) for license compliance in your projects. 

The SCANOSS Sonar Example Plugin provides a few predefined  measures that can be enabled as checks in Sonar Quality Gates: Copyleft, Copyright and Vulnerabilities.

## Overview

The repository is structured into a single `main` branch to showcase the plugin execution:

- [`main`](https://github.com/scanoss/integration-sonarqube/tree/main): Demonstrates a scenario where the codebase comply with the policies:
    - No copyleft licenses are found within the codebase.

Introduction of copyleft components can be tested by adding code to the repository.
  

## Policies in Detail

- **Copyleft**: This policy scans your code for copyleft licenses. If no copyleft licenses are identified, the check passes. Otherwise, it fails, indicating non-compliance.


## How to Use Sonarqube Integration in Your Project

To use the SCANOSS Sonarqube Example Plugin in your project

### Pre-requisites:

- Sonarqube instance

- Install SCANOSS Sonarqube Plugin (See [Plugin's repository](https://github.com/scanoss/scanoss-sonar-example-plugin/) for further information)

### Project Setup Instructions:

1. Add the required project variables and secrets:

- SONAR HOST URL (Variable): `SONAR_HOST_URL` pointing to your sonar instance. Example: https://sonar.yourcompany.com
- SONAR TOKEN (Secret): `SONAR_TOKEN` secret corresponding to your Sonar's project Analysis Method (Other CI).

2. Add a `sonar-project.properties` file at the root folder of your project containing the project Key from your Sonar instance:
```
sonar.projectKey=integration-sonarqube
```

3. Add a workflow file under `.github/workflows` with the following basic setup:
```yaml
name: SCANOSS Sonarqube Copyleft detection

on:
  push:
    branches:
      - 'main'

jobs:
  scanoss:
    name: SCANOSS Scan
    runs-on: ubuntu-latest
    permissions: read-all
    steps:
      - uses: actions/checkout@v3
        with:
          fetch-depth: 0 
      - uses: sonarsource/sonarqube-scan-action@master
        env:
          SONAR_TOKEN: ${{ secrets.SONAR_TOKEN }}
          SONAR_HOST_URL: ${{ vars.SONAR_HOST_URL }}
      - uses: sonarsource/sonarqube-quality-gate-action@master
        timeout-minutes: 5
        env:
          SONAR_TOKEN: ${{ secrets.SONAR_TOKEN }}
```