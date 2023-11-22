## Introduction

This is a sample integration that demonstrates the ease of use of the Mobb CLI (Bugsy) in a CI/CD environment. By adding a single CLI command, you can introduce Mobb's autofix capability to any SAST pipelines to expedite the remediation of your security findings. In this  example, we will showcase how to add a Mobb Pipeline Stage to an existing Gitlab pipeline where a Checkmarx scan is configured to during during merge requests

## Pre-Requisites

To replicate this sample integration in your own Gitlab Pipeline:
- Sign up a free account at https://mobb.ai
- Generate an API token and store it in a safe place
- An existing Checkmarx (CxOne) subscription

![image](/source/images/MobbGenerateAPI.gif "Generate Mobb API Key"){width=70%}



