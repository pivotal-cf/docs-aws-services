# Service Broker for AWS

## How the branches here work

Use **master** for next the unreleased version, and numbered branches for the corresponding live releases. For example:

| Branch name | Use for… | Currently lives…
|-------------| ------| ------|
| master      | Currently the published 1.4.5 branch | http://docs.pivotal.io/aws-services/ |
| 1.5         | Currently edge | https://docs-pcf-staging.cfapps.io/aws-services/1-5/ |

`master` currently has all changes for v1.4.<br>
`1.5` should have all changes made to v1.4, as well as additional work for EMR.<br>
When making changes to v1.4, cherry-pick those changes to 1.5 as well!

Currently v1.5 is in "sustain mode". There is no plan for its release.
