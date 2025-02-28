# Scratch Org Pool Configuration

{% hint style="info" %}
The latest JSON schema for scratch org pool configuration can be found [here](https://github.com/Accenture/sfpowerscripts/blob/develop/packages/sfpowerscripts-cli/resources/schemas/pooldefinition.schema.json).
{% endhint %}

The `orchestrator:prepare` command accepts a JSON configuration file that defines settings and attributes of the scratch org pool. The properties accepted by the configuration file are shown in the table below.

| Property                            | Type    | Description                                                                                                           |
| ----------------------------------- | ------- | --------------------------------------------------------------------------------------------------------------------- |
| tag                                 | string  | Name used to identify the scratch org pool                                                                            |
| waitTime                            | int     | Minutes the command should wait for a scratch org to be created, Default is 6 minutes                                 |
| expiry                              | int     | Number of days for which the scratch orgs are active                                                                  |
| maxAllocation                       | int     | Maximum capacity of the pool                                                                                          |
| batchSize                           | int     | Number of processes for creating scratch orgs in parallel                                                             |
| configFilePath                      | string  | Path to scratch org definition JSON file                                                                              |
| succeedOnDeploymentErrors           | boolean | Whether to persist scratch org to the pool for a deployment error, default:true                                       |
| snapshotPool                        | string  | Name of the earlier prepared scratch org pool that can be utilized by this pool, to prepare pools in multiple stages. |
| installAll                          | boolean | Install all package artifacts, in addition to the managed package dependencies                                        |
| releaseConfigFile                   | string  | Path to a release config file to create pools with selected packages. Use in conjunction with installAll              |
| enableSourceTracking                | boolean | Enable source tracking by deploying packages using source:push and persisting source tracking files                   |
| relaxAllIPRanges                    | boolean | Relax all IP addresses, allowing all global access to scratch orgs                                                    |
| ipRangesToBeRelaxed                 | array   | Range of IP addresses that can access the scratch orgs                                                                |
| retryOnFailure                      | boolean | Retry installation of a package on a failed deployment                                                                |
| maxRetryCount                       | number  | Maximum number of times a package should be retried while deploying to a scratchorg, The default is 2                 |
| preDependencyInstallationScriptPath | string  | Path to a script file that need to be executed before dependent packages are installed in a scratch org               |
| postDeploymentScriptPath            | string  | Path to a script file that need to be exectued after all the packages (dependencies+repository) is installed          |
| enableVlocity                       | boolean | Enable vlocity settings and config deployment. Please note it doesnt install vlocity managed package"                 |
| fetchArtifacts                      | object  | Fetch artifacts, to be deployed to scratch orgs, from an artifact registry                                            |
| fetchArtifacts.artifactFetchScript  | string  | Path to the shell script containing logic for fetching artifacts from a universal registry, if not using npm          |
| fetchArtifacts.npm                  | object  | Fetch artifacts from NPM registry                                                                                     |
| fetchArtifacts.npm.scope            | string  | Scope of the NPM package                                                                                              |

### Sample configuration files

#### Install artifacts from Universal registry, using a shell script

```
{
    "tag": "DEV-POOL",
    "maxAllocation": 20,
    "expiry": 10,
    "batchSize": 10,
    "configFilePath": "config/project-scratch-def.json",
    "relaxAllIPRanges": true,
    "installAll": true,
    "enableSourceTracking": true,
    "retryOnFailure": true,
    "succeedOnDeploymentErrors": true,
    "fetchArtifacts": {
        "artifactFetchScript": "path/to/artifactFetchScript.sh"
    }
}
```

#### Install artifacts from NPM registry

```
{
    "tag": "DEV-POOL",
    "maxAllocation": 20,
    "expiry": 10,
    "batchSize": 10,
    "configFilePath": "config/project-scratch-def.json",
    "relaxAllIPRanges": true,
    "installAll": true,
    "enableSourceTracking": true,
    "retryOnFailure": true,
    "succeedOnDeploymentErrors": true,
    "fetchArtifacts": {
        "npm": {
            "scope": "myproject",
    }
}
```

#### Selective deployment of artifacts using a release config file

````
// Pool Configuration with releaseConfigFile
```json
{
  "$schema": "https://raw.githubusercontent.com/dxatscale/sfpowerscripts/develop/packages/sfpowerscripts-cli/resources/schemas/pooldefinition.schema.json",
  "tag": "cipool-release",
   "maxAllocation": 4,
   "expiry": 2,
   "batchSize": 5,
   "configFilePath": "config/project-scratch-def.json",
   "releaseConfigFile":"release-config/cipoolrelease.yml",
   "enableSourceTracking": false,
   "installAll": true,
    "fetchArtifacts": {
      "npm": {
        "scope": "dxatscale"
      }
    }
 }

```
````
