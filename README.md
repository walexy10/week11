# This workflow uses actions that are not certified by GitHub.
# They are provided by a third-party and are governed by
# separate terms of service, privacy policy, and support
# documentation.

name: tfsec

on:
  push:
    branches: [ prod, dev, staging ]
  pull_request:
    branches: [ "prod" ]
  schedule:
    - cron: '25 23 * * 0'

jobs:
  tfsec:
    name: Run tfsec sarif report
    runs-on: ubuntu-latest
    permissions:
      actions: read
      contents: read
      security-events: write

    steps:
      - name: Clone repo
        uses: actions/checkout@v3

      - name: Run tfsec
        uses: aquasecurity/tfsec-sarif-action@9a83b5c3524f825c020e356335855741fd02745f
        with:
          sarif_file: tfsec.sarif

      - name: Upload SARIF file
        uses: github/codeql-action/upload-sarif@v2
        with:
          # Path to SARIF file relative to the root of the repository
          sarif_file: tfsec.sarif
