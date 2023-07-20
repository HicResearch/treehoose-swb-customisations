# TREEHOOSE Service Workbench Customisations

This add-on will enable customising Service Workbench (SWB) on AWS for TRE specific use-cases.
Currently these changes can not be incorporated in the
open source SWB project as the current version is in lights-on only mode.

These changes can be potentially merged into next major version
release of SWB.

While that happens this add-on enables TRE users to implement limited
customisations to SWB as listed below

- Enable RDP connections for Linux based workspaces
- Enable use of Elastic Fleets in AppStream
- Simplify workspace login experience

> Above customisations are inter-dependent and need to be implemented
> all together. They have not be tested in isolation.

For detailed information on these customisations
refer [DEVELOPMENT_GUIDE](./operations/DEVELOPMENT_GUIDE.md)

## Considerations

The customisations are tested with below versions of Service Workbench and are not
guaranteed to work with all versions. Please perform your own testing
if working with a different than listed below

- 6.0.0

For an existing Service Workbench installation, account re-onboarding is required
to use Elastic Fleets in AppStream. Be mindful of any existing research workspaces while
performing this activity. For new Service Workbench installation, apply this customisation
before the first deployment.

## Deployment Instructions

Follow these [instructions](./deploy/deploy.md) to deploy TRE specific SWB customisations.
