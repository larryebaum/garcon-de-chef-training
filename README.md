# WARNING: THIS PROJECT ASSUMES PRE-CONFIGURED IMAGES. THIS WILL LIKELY NOT WORK OUTSIDE INTERNAL CHEF
# DOUBLE WARNING: THE ABOVE IS ESPECIALLY TRUE FOR CHEF ESSENTIALS WINDOWS IT WILL ONLY WORK IN THE TRAINING AWS ACCOUNT

## Purpose
Creating machines for students to use in our [onsite Chef training](https://training.chef.io/training/onsite.html) takes manual effort. I don't like manual effort. This project reduces manual effort significantly.

This project does the following:
  - Creates machines for [onsite Chef training](https://training.chef.io/training/onsite.html)
  - Creates a Markdown file that assigns machines to students

## Prerequisites

This project depends on Hashicorp Terraform to create VMs in AWS. Ensure Terraform is installed by installing from [here](https://www.terraform.io/downloads.html) or via `brew install terraform` if you are a Homebrew user.

## How To Use

1. Verify `~/.aws/credentials` is configured. (see [here](http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html))
2. Copy `config.yml.example` to `config.yml`
3. Modify `config.yml`
    - Modify class type
    - Modify company name
    - Modify tag info (X-Dept, X-Contact)
    - Modify student list
3. Run `rake create`
4. Create a GitHub Gist from resulting Markdown in `output/` (I recommend using <https://github.com/defunkt/gist>)
5. Profit

## Cleanup

### Automatic
1. Run `rake destroy:force`

### Manual
2. Run `terraform destroy` in the terraform directory corresponding to your class (Example: `output/2017-04-06-Testing-chef-essentials-windows/terraform/`)

## Using Multiple AWS Accounts

Since this project uses Terraform you have the option to configure it to use an multiple AWS accounts. To do this you need to configure profiles in your `~/.aws/config`. A detailed guide on this process can be found [here](http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html#cli-multiple-profiles).

Be sure your environment profile directs AWS to read its configuration and keys from these files by updating your profile as follows (or to where the files reside): 
```
AWS_CONFIG_FILE=/Users/<YOUR USER ID>/.aws/config
AWS_CREDENTIAL_FILE=/Users/<YOUR USER ID>/.aws/credentials
```

TL;DR:
  1. Add a profile block to your `~/.aws/config`. Example:
  ```
  [default]
  region=us-west-2
  output=json

  [profile other-account]
  region=us-east-1
  output=json
  ```
  2. Add credentials for your profile to `~/.aws/credentials`. Example:
  ```
  [default]
  aws_access_key_id=AKIAIOSFODNN7EXAMPLE
  aws_secret_access_key=wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY

  [other-account]
  aws_access_key_id=AKIAI44QH8DHBEXAMPLE
  aws_secret_access_key=je7MtGbClwBF/2Zp9Utk/h3yCo8nvbEXAMPLEKEY
  ```
  3. Modify profile value in your `config.yml`
  4. Follow other parts of the `How to Use` section above

## Testing

This project both conforms to RuboCop standards and has RSpec tests.

To run both use: `rake test`

To run just RSpec run: `rake test:unit`

To run just RuboCop run: `rake test:lint`
