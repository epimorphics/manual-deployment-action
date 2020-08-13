# Manual deployment action

Copy an existing image from one environment-specific ECR repository to another. 

Assumes AWS credentials have been configured and docker has been logged into ECR.

## Inputs

| Input | Description |
|---|---|
| `access_key_id` | AWS credentials key id |
| `secret_access_key` | AWS credentials secret access key |
| `imagebase` | Name of the image (without any environment suffix) |
| `source` | Name of environment to copy from |
| `destination` | Name of environment to copy to |
| `region` | AWS region of ECR as extracted by deployment mapper |
| `accountid` | AWS account id ECR as extracted by deployment mapper |

## Example usage

TODO