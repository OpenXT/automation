This directory contains a generic .gitlab-ci.yml that can be used for building OpenXT. The configuration is set to build on a Gitlab runner in an AWS environment, including using ECR to host the docker build images. There is also a generic config.toml used to configure a Gitlab runner. This build assumes there are /certs, /cache and /cache/sstate directories. The /cache is used by the gitlab-runner to store the project build cache. The build will make directories in /cache/sstate named after the branch being built so multiple baselines won't clobber the sstate. The /certs directory is used to store the certificates created by bordel in the setup:workspace job.

The following Gitlab secret variables are assumed to be available on the builder project:
 - AWS_S3_ISO_KEY_ID: Key used to upload the finished ISO to an S3 bucket
 - DOCKER_AUTH_CONFIG: Path to the auth config to use for accessing the AWS Elastic Container Registry for docker images
 - EC2_DOCKER_REGISTRY: Hostname of the docker registry to use
 - ECR_ID: ID for the ECR, typically the first part of the EC2_DOCKER_REGISTRY host
 - NUM_DAYS_TO_EXPIRE: Number of days to expire the ISOs stored in S3

Replace the following values in the .gitlab-ci.yml to start a build:
 - GITLAB_HOST: The URL to the Gitlab hosting any customized source repositories
 - AWS_REGION: The AWS region your server and ECR run in

To run the Gitlab Runner on AWS, the docker-credential-ecr-login is required, available at https://github.com/awslabs/amazon-ecr-credential-helper. To configure the gitlab runner, change the following in the etc/gitlab-runner/config.toml:
 - url: The url to your gitlab server
 - token: The project token assigned to your runner when adding it to the build project
 - name: Name the runner appropriately
