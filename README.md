# Terraform Backend Action

This action generates simple Terraform backend.tf File   

## Inputs

### `s3-bucket-name`

**Required** - S3 Bucket name for Backend.

### `s3-bucket-region`

**Required** - S3 Bucket region for Backend.

## Outputs

### [None]


## Example usage
```yaml
name: 'Terraform'

on:
  push:
    branches: [ "master" ]
  pull_request:

jobs:
  terraform:
    name: 'Terraform'
    runs-on: ubuntu-latest

    defaults:
      run:
        shell: bash

    steps:
    - name: Checkout
      uses: actions/checkout@v3

    - uses: hashicorp/setup-terraform@v2
      with:
        terraform_version: 1.1.9

    - uses: ikambarov/terraform-backend-action@master
      with:
        s3-bucket-name: ${{ secrets.BUCKET_NAME }}
        s3-bucket-region: "us-east-1"

    - name: Terraform Init
      run: terraform init

    - name: Terraform Apply
      if: github.ref == 'refs/heads/"master"' && github.event_name == 'push'
      run: terraform apply -auto-approve -input=false
```