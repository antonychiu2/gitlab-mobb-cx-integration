# Sample Integration of Mobb with Checkmarx SAST scan in Gitlab Pipelines

This is a sample integration that demonstrates the ease of use of the Mobb CLI (Bugsy) in a CI/CD environment. By adding a single CLI command, you can introduce Mobb's autofix capability to any SAST pipelines to expedite the remediation of your security findings. In this  example, we will showcase how to add a Mobb Pipeline Stage to an existing Gitlab pipeline where a Checkmarx scan is configured to during during merge requests

# Usage

## Register

You will need the following to perform this integration:

- Sign up for a free account at https://mobb.ai
- An existing Checkmarx (CxOne) subscription
- An existing Gitlab account

## Setup 

To setup this integration in your own Gitlab environment, you will need to first gen the Mobb API Token. 

After logging into the Mobb portal, click on the "settings" icon on the bottom left, then select "Access tokens". 
From here, you can generate an API key by selecting the "Add API Key" button. 

![image](/source/images/MobbGenerateAPI.gif "Generate Mobb API Key"){width=60%}

Next, go to your Gitlab repository and select "Settings -> CI/CD -> Variables". From here, we can select "Add Variable". For the variable key, we will call it `MOBB_API_KEY`. For the Value, we will past the value of the token from the previous step. 

![image](/source/images/GitlabAddVariable.gif "Gitlab Add Variable"){width=60%}

## Configure Gitlab Pipeline

Let's now configure the Gitlab Pipeline. You can use the following sample YAML script, or customize it to your liking. 

If you decide to this this exact YAML script, please ensure your Checkmarx related variables are well defined. Namely, 
`CX_BASE_AUTH_URI`, `CX_API_KEY`, `CX_BASE_URI`, `CX_TENANT`. You can find the value to these variables by following this [guide](https://checkmarx.com/resource/documents/en/34965-118315-authentication-for-checkmarx-one-cli.html) on Checkmarx documentation page. 

```yaml
# This example utilizes Mobb with Checkmarx via GitLab CI/CD pipelines

image:
  name: "node:latest"

stages:
  - checkmarx-sast-scan
  - mobb-autofixer

workflow: # Run on every merge request
  rules:
    - if: $CI_PIPELINE_SOURCE == 'merge_request_event'

checkmarx-sast-scan-job:
  stage: checkmarx-sast-scan
  tags:
    - saas-linux-medium-amd64
  script:
    - wget https://github.com/Checkmarx/ast-cli/releases/download/2.0.61/ast-cli_2.0.61_linux_x64.tar.gz -O checkmarx.tar.gz
    - tar -xf checkmarx.tar.gz
    - ./cx configure set --prop-name cx_apikey --prop-value $CX_API_KEY
    - ./cx configure set --prop-name cx_base_auth_uri --prop-value $CX_BASE_AUTH_URI
    - ./cx configure set --prop-name cx_base_uri --prop-value $CX_BASE_URI
    - ./cx configure set --prop-name cx_tenant --prop-value $CX_TENANT
    - ./cx scan create --project-name $CX_PROJECT_NAME -s ./ --report-format json --scan-types sast --branch nobranch  --threshold "sast-high=1"
  artifacts:
    paths:
    - "*.json"
    when: always

mobb-autofixer-job:
  stage: mobb-autofixer
  tags:
    - saas-linux-medium-amd64
  script:
    - npx mobbdev@latest analyze -f cx_result.json -r $CI_PROJECT_URL --ref $CI_COMMIT_REF_NAME --api-key $MOBB_API_KEY
  when: on_failure # Run Mobb only if there's a finding to fix


```
