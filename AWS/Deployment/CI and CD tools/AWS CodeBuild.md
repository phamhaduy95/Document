After CodeBuild successfully connects to the source repository or location, you select a _build environment_
Next, you will configure the _build specification_. This can be done by inserting build commands in the console or by specifying a `buildspec.yml` file in your source code.

```YAML
version: 0.2

env:
  variables:
    NODE_ENV: production
  # Define variables to be sourced from AWS Secrets Manager
  secrets-manager:
    # Syntax: <LOCAL_ENV_VAR_NAME>: <SECRET_ID>[:<JSON_KEY>[:<VERSION_STAGE>|<VERSION_ID>]]
    DB_PASSWORD: "my-database-secret:password" 
    DOCKER_TOKEN: "docker-registry-secret"
    # Example using a full ARN (useful for cross-account access)
    # API_SECRET: "arn:aws:secretsmanager:us-east-1:123456789012:secret:api-secret-key"

phases:
  install:
    runtime-versions:
      nodejs: 18
    commands:
      - echo "Installing dependencies..."
      - npm install
  
  pre_build:
    commands:
      - echo "Running tests..."
      # You can now access the secrets using the local environment variable names
      - echo "Database user is connecting with password: $DB_PASSWORD"
      # Securely log into a Docker registry using the secret
      - echo "$DOCKER_TOKEN" | docker login -u YOUR_DOCKER_USER --password-stdin
      
  build:
    commands:
      - echo "Building the application..."
      - npm run build
      
  post_build:
    commands:
      - echo "Build completed on `date`"
      # Further actions like pushing Docker images can use the secrets

artifacts:
  files:
    - 'build/**/*'
    - 'appspec.yml'

```

If your build creates artifacts you would like to use in later steps of your pipeline/process, you specify _output artifacts_ to save to S3.



CodeBuild supports caching, which you can configure in the next step. Caching saves some components of the build environment to reduce the time to create environments when you submit build jobs.

When you run builds manually in the CodeBuild console, AWS CLI, or AWS SDK, you have the option to change several properties before you run a build job:

- Source version (Amazon S3)
- Source branch, version, and Git clone depth (AWS CodeCommit, GitHub, and Bitbucket)
- Output artifact type, name, or location
- Build timeout
- Environment variables