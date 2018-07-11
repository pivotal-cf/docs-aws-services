# Service Broker for AWS

## How the branches here work

Use **master** for next the unreleased version, and numbered branches for the corresponding live releases. For example:

| Branch name | Use for… | Protected? | Currently lives…
|-------------| ------| ------| ------|
| master      | Currently the published 1.4.8 docs. Contains all changes for v1.4. | Yes | http://docs.pivotal.io/aws-services/ |
| 1.5         | Currently Edge. Contains all changes made to v1.4, as well as additional work for EMR. v1.5 is in "sustain mode". There is no plan for its release. However, when making changes to v1.4, cherry-pick those changes to 1.5 as well! | Yes | https://docs-pcf-staging.cfapps.io/aws-services/1-5/ |
