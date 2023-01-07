# IaC Environment Pipeline - Automatic Deployments

_**Astronaut Buzz Lloyd:** "Blow the hatch."  
**NASA Administrator Charles Keith:** "DO NOT! DO NOT blow the hatch!"  
**Astronaut Buzz Lloyd:** "Hatch blown!"  
**--Marooned**_

This repository contains Terraform definitions for environments which are suitable for unattended, automated deployments from [application build pipelines](https://github.com/mikeroach/aphorismophilia). The idea is that with comprehensive, robust testing and rich observability metrics at each stage of developing the code powering our infrastructure and application services, we have high confidence that our work is ready to be delivered to customers when our CI/CD pipelines indicate success.

This [Terraform HCL](./main.tf) is effectively a wrapper to deploy a specific version of the [IaC Environment Template](https://github.com/mikeroach/iac-template-pipeline) with environment-appropriate variables. It and its [unsecret variables](./auto.tfvars) are updated by humans (e.g. to specify a new environment template module version), or by application build pipelines through use of the so-called ["GitOps Helper" script](./gitops-helper.sh).

#### Changes Workflow

1. Human or application pipeline updates Terraform HCL/variables in feature branch and submits pull request.
1. Jenkins examines pull request, runs Terraform validation tests, then merges into `main` upon success.
1. Jenkins examines `main` branch, repeats validation tests, applies Terraform plan in the live environment, and runs integration tests.
1. (Optional) If tests succeed and the Git commit message contains the magic word ```[PROMOTE]```, submit an upgrade request to the [gated environment](https://github.com/mikeroach/iac-pipeline-gated) via that repository's [GitOps Helper script](https://github.com/mikeroach/iac-pipeline-gated/gitops-helper.sh).