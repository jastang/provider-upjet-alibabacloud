# provider-upjet-alibabacloud

## Build

Run the following to build the provider locally.

```
make submodules
make generate
```

## Test

Add an environment variable to set the credentials for the target Alibaba
account for the tests as follows and then run `make e2e`.

```
export UPTEST_CLOUD_CREDENTIALS='{
    "access_key": "...",
    "secret_key": "...",
    "region": "us-west-1"
}'
```

## Submit PR

- `make reviewable` before submitting a new PR
- git commit -s -m "sign every commit"

## Release New Provider Version

### Determine Version

Identify the version to be released by increasing the minor version by one. For example, if the provider's latest version is v1.1.0, the new version will be v1.2.0.

According to the semantic versioning specification, a version number is represented as MAJOR.MINOR.PATCH. For 1.2.0 : MAJOR=1, MINOR=2, PATCH=0 

### Create Release Branch

From the GitHub UI, create a new branch from the main branch with the name release-<major>-<minor><patch>.

To cut the release v1.2.0, we will name our branch release-1.2.0.

### Build Release Candidate

GitHub should automatically trigger a `CI` workflow run on the newly created branch and produce a package.  You can check it from the GitHub UI by clicking `Actions => CI`.

If it does not, you can manually run the GitHub workflow named CI on the release branch to produce a package.

### Cut The Release

Tag the release branch with the version by running the GitHub workflow named `Tag` on the release branch.

### Publish The Providers

Build and push the family packages using the `Publish Provider Packages` Github Actions workflow. To do this, you need to provide the values of the following parameters:

- subpackages (to be built individually, e.g. config ram): config ack ackone alb alidns cdn cloudmonitorservice ecs fcv3 kms messageservice oss polardb privatelink quotas ram slb tair vpc
- size (Number of smaller provider packages to build and push with each build job): 30
- concurrency (Number of parallel package builds within each build job): 1
- version (Version string to use while publishing the packages,e.g. v1.2.0): v1.2.0
- go-version (Go version to use if building needs to be done): 1.24

Your release build will be published once the `Publish Provider Packages` job if
releasing a family of providers succeeds. Check their availability in the
Upbound marketplace [here](https://marketplace.upbound.io/providers/crossplane-contrib/provider-family-alibabacloud).

### Add Release Notes

Go [here](https://github.com/crossplane-contrib/provider-upjet-alibabacloud) and
click on releases on the left side. 

On the releases page, click on "Draft New Release".
- As target select your release branch that you created above
- Select the corresponding release tag
- Use your version as Release Title, e.g. v1.2.0
- Click "Generate release notes"
- Click "Publish release"
