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
