# Anchore-CircleCI-Integration
PoC Script to integrate Anchore Engine to existing CI/CD pipeline on circle CI.

## Why Docker Image Scan? 

> build level integrations with existing circleCI pipeline.

> We want to find an open source alternative which does the same job as any enterprise tool such as prisma cloud at more granular level. (ex:  when a new feature is added to a specific service which introduces a new dependency at code level , vulnerable base image etc. ) This also helps the dev team to look at the issues specific to their team eliminating the need to contact security team for trivial issues.

> We want to fail the CD part(not pushing to docker registry) if built image / docker file do not adhere to certain security policies . This prevents pushing/deploying vulnerable docker containers onto the cloud.

## How do we plan to do? 

We can Address the above issues with the help of OSS Anchore Engine. 

The Anchore Engine is an open-source tool for scanning and analyzing container images for security vulnerabilities and policy issues. 

It accepts the bare minimum built container image and does a variety of Security / Best practices / Compliance checks by analyzing contents of the image itself.

## Why Anchore Engine?
It is Open source software which is widely supported and used across industries.

It is container native , distributed system. 

It supports  flexible and customizable user defined policies .

Out of the box Integrations with circle CI and github actions . 

## Anchore Capabilities:
checks vulnerable OS packages , 3rd party software packages , build metadata.

checks for hardcoded Secrets , metadata (DockerFile misconfigurations.)

## How do we Integrate it to our existing CircleCI CD ?

