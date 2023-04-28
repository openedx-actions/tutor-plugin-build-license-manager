<img src="https://avatars.githubusercontent.com/u/40179672" width="75">

[![Tests](https://github.com/openedx-actions/tutor-plugin-build-license-manager/actions/workflows/testRelease.yml/badge.svg)](https://github.com/openedx-actions/tutor-plugin-build-license-manager/actions)
[![Open edX Discussion](https://img.shields.io/static/v1?logo=discourse&label=Forums&style=flat-square&color=000000&message=discuss.openedx.org)](https://discuss.openedx.org/)
[![docs.tutor.overhang.io](https://img.shields.io/static/v1?logo=readthedocs&label=Documentation&style=flat-square&color=blue&message=docs.tutor.overhang.io)](https://docs.tutor.overhang.io)
[![hack.d Lawrence McDaniel](https://img.shields.io/badge/hack.d-Lawrence%20McDaniel-orange.svg)](https://lawrencemcdaniel.com)<br/>
[![AWS](https://img.shields.io/badge/AWS-%23FF9900.svg?style=for-the-badge&logo=amazon-aws&logoColor=white)](https://aws.amazon.com/)
[![Docker](https://img.shields.io/badge/docker-%230db7ed.svg?style=for-the-badge&logo=docker&logoColor=white)](https://www.docker.com/)
[![Kubernetes](https://img.shields.io/badge/kubernetes-%23326ce5.svg?style=for-the-badge&logo=kubernetes&logoColor=white)](https://kubernetes.io/)

# tutor-plugin-build-license-manager

Github Action that uses Tutor to build a Docker image for Open edX [License Manager](https://github.com/openedx/license-manager) service, and uploads to an AWS Elastic Container Registry repository. Automatically creates the AWS ECR repository if it does not exist.

## About the license-manager image

The edX License Manager Service, a Django backend for managing licenses and subscriptions. This is a production-ready image consisting of the repository [https://github.com/openedx/license-manager](https://github.com/openedx/license-manager).

## Usage

```yaml
name: Example workflow

on: workflow_dispatch

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      # required antecedent
      - uses: actions/checkout

      # required antecedent
      - name: Configure AWS credentials
        uses: aws-actions/configure-aws-credentials
        with:
          aws-access-key-id: ${{ secrets.THE_NAME_OF_YOUR_AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.THE_NAME_OF_YOUR_AWS_SECRET_ACCESS_KEY }}
          aws-region: us-east-2

      # install and configure tutor and kubectl
      - name: Configure Github workflow environment
        uses: openedx-actions/tutor-k8s-init

      # This action.
      # Note:
      # - aws-ecr-repo is optional. Default: 'license-manager'
      # - plugin-repository is optional. Default: https://github.com/openedx/license-manager.git
      # - plugin-version is optional. Default: main
      - name: Build the image and upload to AWS ECR
        uses: openedx-actions/tutor-plugin-build-license-manager
        with:
          aws-ecr-repo: license_manager
          plugin-repository: https://github.com/lpm0073/tutor-contrib-license-manager.git
          plugin-version: main # in this case the main branch is specified. You may also specify a tag
```
