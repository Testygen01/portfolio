# AWS Innovation Sandbox
`AWS` `Terraform` `AWS Control Tower` `GitLab`

## Overview

The DoIT Public Cloud Team needed a sandbox where researchers could experiment with AWS services without risking production accounts or breaking institutional guardrails. The project started as a direct ask from a Research facilitator consultant and was not on the team's roadmap for the year. I joined it during my rotation and worked with Kelly Rivera, the Cloud Engineer Lead.

## The central question

AWS ships the Innovation Sandbox as a set of CloudFormation deployment templates. The Public Cloud Team runs a 100 percent Terraform environment. The question was how to take templates designed for CloudFormation and deploy the whole sandbox through Terraform without losing what the templates were built to do.

## What I did

I sat in the requirements meetings where we worked out what the sandbox had to do, alongside the consultant and the team. Then I wrote the Terraform in live coding sessions over Teams video calls, with Kelly directing each session and telling me what to do as we went. I wrote the Terraform that deployed the AccountPool stacks, the IDC stack, the Data stack, and the Compute stack. Because the AWS templates were written for CloudFormation, I modified the .template and .yaml files so they could be deployed through Terraform.

## Blocker 1: no API for third-party SAML apps

The sandbox needs single sign-on, and AWS does not expose API endpoints to fully automate provisioning a third-party SAML application through the Terraform AWS provider. The provider's ssoadmin_application resource documents what is possible, and full automation of that step is not among it. That meant the SAML application step could not be folded into the Terraform deployment like everything else. It had to be handled outside the automated path.

## Blocker 2: template size limit

The InnovationSandbox-Compute.template file exceeded the maximum template size that Terraform accepts for a deployment. The file was simply too large to deploy directly. To get past it, I created a centralized S3 bucket with Terraform to store the ISB CloudFormation templates, and the deployment referenced the templates from the bucket instead of pushing the full file contents through Terraform.

## Result and scope

I rotated off the team before the project finished, so I did not see the final deployment myself. After my rotation I checked in with the Public Cloud team, and they told me the project is now complete and fully deployed. The groundwork I contributed, the Terraform stacks and the S3 workaround, was part of what got it there.

## What this shows

I can take vendor templates built for one tool and make them work in another, write Terraform for real multi-stack deployments under direction, and diagnose exactly where a provider's automation stops. The SAML gap was not a bug in our code. It was a missing API, and naming that precisely is what kept the team from burning time trying to automate the unautomatable.
