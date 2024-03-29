# ReversingLabs rl-scanner-cloud Azure DevOps (ADO) Examples

This repository contains a working example of Azure DevOps (ADO) pipeline scripts to illustrate scanning with the
[ReversingLabs Spectra Assure Portal](https://docs.secure.software/portal/integrations/).

ReversingLabs Spectra Assure Portal is capable of scanning
[nearly any type](https://docs.secure.software/concepts/language-coverage)
of software artifact or package that results from a build.

In this example, we use the source code and Maven build instructions for the Struts2 showcase web app,
which came with [Apache Struts v2.5.28](https://archive.apache.org/dist/struts/2.5.28/).

The following example is provided in this repository:

- **azure-pipelines.yml** uses the latest rl-scanner-cloud Docker image.

This example requires that you define the `RLPORTAL_ACCESS_TOKEN` secret environment variable to store your [Portal access token](https://docs.secure.software/api/generate-api-token).


## Pipeline example


### azure-pipelines.yml
This pipeline script builds the WAR file and scans it using the
[ReversingLabs rl-scanner-cloud Docker image](https://hub.docker.com/r/reversinglabs/rl-scanner-cloud).

After the file is scanned, analysis reports in JSON, CycloneDX, and SPDX formats are saved as pipeline artifacts.

**Note:**
There is no support for the rl-html report format at this time.


**Supported parameters**

| Name | Required | Type | Description |
| ---- | -------- | ---- | ----------- |
| `RLPORTAL_ACCESS_TOKEN` | *Yes* | string | A Personal Access Token for authenticating requests to the Spectra Assure Portal. Before you can use this example, you must [create the token](https://docs.secure.software/api/generate-api-token) in your Portal settings. Tokens can expire and be revoked, in which case you'll have to update this value. Define it as a secret in a group `rl-scanner-cloud` |
| `RLPORTAL_SERVER`      | *Yes* | string | Name of the Spectra Assure Portal instance to use for the scan. The Portal instance name usually matches the subdirectory of `my.secure.software` in your Portal URL. For example, if your portal URL is `my.secure.software/demo`, the instance name to use with this parameter is `demo`. |
| `RLPORTAL_ORG`         | *Yes* | string | Name of the Spectra Assure Portal organization to use for the scan. The organization must exist on the Portal instance specified with `RLPORTAL_SERVER`. The user account authenticated with the token must be a member of the specified organization and have the appropriate permissions to upload and scan a file. Organization names are case-sensitive. |
| `RLPORTAL_GROUP`       | *Yes* | string | Name of the Spectra Assure Portal group to use for the scan. The group must exist in the Portal organization specified with `RLPORTAL_ORG`. Group names are case-sensitive. |
| `RL_PACKAGE_URL`        | *Yes* | string | The package URL (purl) used to associate the file with a project and package on the Portal. Package URLs are unique identifiers in the format `[pkg:type/]<project></package><@version>`. When scanning a file, you must assign a package URL to it, so that it can be placed into the specified project and package as a version. If the project and package you specified don't exist in the Portal, they will be automatically created.  |
| `ARTIFACT2SCAN`         | No | string | The artifact (file) you want to scan. The file must be in any of the [formats supported by Spectra Assure](https://docs.secure.software/concepts/reference). |
| `REPORT_PATH`           | No | string | Path to the location where you want to store analysis reports. Must be relative to the `System.DefaultWorkingDirectory` |


## Pipeline result

The scan in this example will produce the `FAIL` CI status.
The pipeline will fail, as the scanner detects 4 critical issues: `Found 4 vulnerabilities matching selected criteria.`

## Useful resources

  1. [The official Spectra Assure Portal documentation](https://docs.secure.software/portal/)
  2. The official `reversinglabs/rl-scanner-cloud` Docker image [on Docker Hub](https://hub.docker.com/r/reversinglabs/rl-scanner-cloud)
