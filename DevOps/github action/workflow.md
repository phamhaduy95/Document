


``` yaml
jobs:  
deploy:  
if: ${{ github.ref == 'refs/heads/main' }}  
runs-on: ubuntu-latest  
steps:  
- run: echo "Deploying branch ${{ github.ref }}"
```


If one step in a GitHub Actions job fails, the default behavior is that the entire job will fail, and subsequent steps within that job will not be executed. If other jobs in the

#### forwarding artifact between jobs
Yes, built artifacts can be forwarded to other jobs within a GitHub Actions workflow using the `upload-artifact` and `download-artifact` actions. This is the standard and recommended method for sharing data between jobs in the same workflow.

How to forward built artifacts:

- **Upload the artifact in the producing job:** In the job that generates the built artifact, use the `actions/upload-artifact` action to save the desired files or directories. You will need to specify a `name` for the artifact and the `path` to the files or directory to be uploaded.

Code

```yaml
    jobs:
      build:
        runs-on: ubuntu-latest
        steps:
          - name: Build project
            run: |
              # Your build commands here, producing artifacts in a 'dist' folder
              mkdir dist
              echo "Build output" > dist/output.txt
          - name: Upload artifact
            uses: actions/upload-artifact@v4
            with:
              name: my-build-artifact
              path: dist/
```

- **Download the artifact in the consuming job:** In the subsequent job that requires the artifact, use the `actions/download-artifact` action. You will need to specify the `name` of the artifact to download. Optionally, you can also specify a `path` where the artifact should be downloaded within the job's runner.

Code

``` yaml
    jobs:
      deploy:
        runs-on: ubuntu-latest
        needs: build # Ensures 'build' job completes before 'deploy' starts
        steps:
          - name: Download artifact
            uses: actions/download-artifact@v4
            with:
              name: my-build-artifact
              path: downloaded-artifacts/
          - name: Use artifact
            run: |
              cat downloaded-artifacts/output.txt
              # Further deployment steps using the downloaded artifact
```


you can set retention time for artifact
``` yaml
    steps:
      - name: ⬆️ Upload artifact with custom retention
        uses: actions/upload-artifact@v4
        with:
          name: my-build-artifact
          path: build_output/
          retention-days: 7 # This artifact will only be kept for 7 days

```

The `retention-days` parameter must be an integer between the minimum (1 day) and the maximum allowed by your organization/repository settings.