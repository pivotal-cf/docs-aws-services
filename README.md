# VMware Tanzu Service Broker for AWS

This repo contains the VMware Tanzu Service Broker for AWS documentation.

In this README:

- [VMware Tanzu Service Broker for AWS](#vmware-tanzu-service-broker-for-aws)
  - [ Branches](#-branches)
  - [ Partials](#-partials)
  - [ Contributing to documentation](#-contributing-to-documentation)
  - [ Publishing docs](#-publishing-docs)
    - [ Prepare Markdown files](#-prepare-markdown-files)
    - [ In Docsdash](#-in-docsdash)
    - [ Promoting to pre-prod and prod](#-promoting-to-pre-prod-and-prod)
  - [ Troubleshooting Markdown](#-troubleshooting-markdown)
  - [ Style guide](#-style-guide)

## <a id="branches"></a> Branches

| Branch name | Use for… | Protected? | Currently lives…
|-------------| ------| ------| ------|
| main      | v1.4.x | Yes | [Stage](https://docs-staging.vmware.com/en/VMware-Tanzu-Service-Broker-for-AWS/1.4/documentation/GUID-index.html) and [Prod](https://docs.vmware.com/en/VMware-Tanzu-Service-Broker-for-AWS/1.4/documentation/GUID-index.html) |
| 1.5         | Obsolete. Okay to delete this branch. Service Broker for AWS is no longer supported. There is no plan to release v1.5. | Yes | not published |


## <a id="partials"></a> Partials

Cross-product partials (if any) are single sourced from the [Docs Partials](https://github.com/pivotal-cf/docs-partials) repository.


## <a id="contributing"></a> Contributing to documentation

If there is some documentation to add for an unreleased patch version of these docs, then create a branch off of the **live** branch
you intend to modify and create a pull request against that branch.
After the version that change is targeting is released, the pull request can be merged and will be live
the next time a documentation deployment occurs.

If the documentation is meant to be target several released versions,
then you will need to:
+ create a pull request for each individual minor version
+ or ask the technical writer to cherry-pick to particular branches/versions.

For instructions on how to create a pull request on a branch and instructions on how to create a
pull request using a fork, see
[Creating a PR](https://docs-wiki.sc2-04-pcf1-apps.oc.vmware.com/wiki/external/create-pr.html)
in the documentation team wiki.


## <a id="publishing-docs"></a> Publishing docs

- [docworks](https://docworks.vmware.com/) is the main tool for managing docs used by writers.
- [docsdash](https://docsdash.vmware.com/) is a deployment UI which manages the promotion from
staging to pre-prod to production. The process below describes how to upload our docs to staging,
replacing the publication with the same version.

### <a id="prepare-markdown-files"></a> Prepare Markdown files
- Markdown files live in this repo.
- Images should live in an `images` directory at the same level and linked with a relative link.
- Each page requires an entry in [config/toc.md](config/toc.md) for the table of contents.
- Variables live in [config/template_variables.yml](config/template_variables.yml).

### <a id="in-docsdash"></a> In Docsdash

1. Wait about 1 minute for processing to complete after uploading.
2. Go to https://docsdash.vmware.com/deployment-stage

   There should be an entry with a blue link which says `Documentation` and points to staging.

### <a id="promoting"></a> Promoting to pre-prod and prod

**Prerequisite** Needs additional privileges - reach out to a manager on the docs team [#tanzu-docs](https://vmware.slack.com/archives/C055V2M0H) or ask a writer to do this step for you.

1. Go to Staging publications in docsdash  
  https://docsdash.vmware.com/deployment-stage

2. Select a publication (make sure it's the latest version)

3. Click "Deploy selected to Pre-Prod" and wait for the pop to turn green (refresh if necessary after about 10s)

4. Go to Pre-Prod list  
  https://docsdash.vmware.com/deployment-pre-prod

5. Select a publication

6. Click "Sign off for Release"

7. Wait for your username to show up in the "Signed off by" column

8. Select the publication again

9. Click "Deploy selected to Prod"

## <a id="troubleshooting"></a> Troubleshooting Markdown

| Problem | List displays as a paragraph |
|---------|-----------|
| Symptom:| Bulleted or numbered lists look fine on GitHub but display as a single paragraph in HTML.|
| Solution: | Add a blank line after the stem sentence and before the first item in the list.|

| Problem | List numbering is broken: every item is `1.` |
|---------|-----------|
| Symptom:| Each numbered item in a list is a `1.` instead of `1.`, `2.`, `3.`, etc|
| Solution: | Try removing any blank newlines within each step.|

| Problem | Code boxes not showing |
|---------|-----------|
| Symptom:| VMware publishing system doesn't accept code tags after the three back ticks.|
| Solution: | Make sure you're not using `shell` or `bash` or `console` or `yaml` after back ticks.|

## <a id="style-guide"></a> Style guide

This is a word list for terminology and word usage specific to the VMware Tanzu Service Broker for AWS for docs.
