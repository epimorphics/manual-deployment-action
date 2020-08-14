# Manual deployment action

Copy an existing image from one environment-specific ECR repository to another. 

Assumes AWS credentials have been configured and docker has been logged into ECR.

## Inputs

| Input | Description |
|---|---|
| `imagebase` | Name of the image (without any environment suffix) |
| `source` | Name of environment to copy from |
| `destination` | Name of environment to copy to |
| `region` | AWS region of ECR as extracted by deployment mapper |

## Example usage

```yaml
name: Manual release
on:
  workflow_dispatch:
    inputs:
      target:
        description: 'Target environment to deploy to'
        required: true
        default: 'staging'
      source:
        description: 'Environment to copy from'
        required: true
        default: 'dev'

jobs:
  manual_release:
    name: Manual release
    runs-on: ubuntu-18.04
    steps:
    - uses: actions/checkout@v2
    - name: "Load deployment config"
      id: mapper
      uses: epimorphics/deployment-mapper@1.1
      with:
        ref: "dummy"

    - name: Configure AWS Credentials
      uses: aws-actions/configure-aws-credentials@v1
      with:
        aws-access-key-id: ${{ secrets.BUILD_EPI_EXPT_AWS_ACCESS_KEY_ID }}
        aws-secret-access-key: ${{ secrets.BUILD_EPI_EXPT_AWS_SECRET_ACCESS_KEY }}
        aws-region: "${{ steps.mapper.outputs.region }}"
    - name: Login to Amazon ECR
      id: login-ecr
      uses: aws-actions/amazon-ecr-login@v1

    - name: "Perform deployment promotion"
      uses: epimorphics/manual-deployment-action@1.1
      with:
        imagebase:   "${{ steps.mapper.outputs.imagebase }}"
        region:      "${{ steps.mapper.outputs.region }}"
        source:      "${{github.event.inputs.source}}"
        destination: "${{github.event.inputs.target}}"
```