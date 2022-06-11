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

We can integrate the Anchore engine Image scanning job before we push the image to the org private docker registry. We currently utilize the anchore-engine circleCI orb to achieve the task.
For now, let the engine use default policy. you can also define a custom policy by dropping them in .circleci/.anchore/<policy.json>

![Anti-Takeover](/screenshots/anchore_3.png)
As we see from the above image, if the image scan job is failed ( does not pass the anchore policies defined by the user ) , workflow fails .

More details on the anchore - circleCI  orb can be found here : https://circleci.com/developer/orbs/orb/anchore/anchore-engine

We are currently utilizing the ## 'analyze_local_image' function to scan the build image. 

Example job definition: 

```bash
anchore_image_scan:
    executor: anchore/anchore_engine
    steps:
      - checkout
      - run: |
          docker build -t "test:anchore" .
      - anchore/analyze_local_image:
          image_name: test:anchore
          timeout: '1000'
          dockerfile_path: ./Dockerfile
          policy_failure: True
      - anchore/parse_reports
      - store_artifacts:
            path: anchore-reports
```

This function will run anchore scans against a locally built image which gets pushed to private repo / gets deployed later in the workflow.


We also has an option to control the job failure status with 'policy_failure' option. If set to True, Workflow halts when the container image violates user defined policy. 

you can refer  the sample workflow with anchore image scan job integrated in this repo :(.circkeci/config.yaml) https://github.com/strikergoutham/Anchore-CircleCI-Integration/blob/main/.circleci/config.yml

We also persist the Anchore generated results as build artifacts for the review by the relevant team.(Build artifacts expire after 30 days.)
## Anchore Engine result artifacts snapshot : 
![Anti-Takeover](/screenshots/anchore_4.png)

## Anchore-Engine Analysis snapshot:
![Anti-Takeover](/screenshots/anchore_2.png)
More on Anchore Engine : 
https://anchore.com/blog/anchore-engine/
