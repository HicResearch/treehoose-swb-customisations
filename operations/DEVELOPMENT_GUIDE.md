# Development Guide

This document provides information on how base Service Workbench on AWS
is customised and what each customised file accomplishes.

The folder structure under the `src` folder is similar to
SWB's folder structure to enable easy overwrite of files
during deployment process.

## Enable RDP connections for Linux based workspaces

To enable RDP capability from Linux workspaces, a new connection type named
`customrdp` is introduced to handle RDP connection for Linux based workspaces
seperately from the RDP connection for Windows workspaces.

UI is customised for a similar experince for RDP logins irrespective of
the type of workspace. Code updates are [here](../src/addons/addon-base-raas-ui/packages/base-raas-ui/src/parts/environments-sc/parts/)

Backend is updated to handle use of the new connection type `customrdp`.
Updates to backend can be found [here](../src/addons/addon-base-raas/packages/base-raas-services/lib/environment/service-catalog/)

By default `ec2-user` is used as user-name for logging in. It is advised to ensure that this user
is configured and allows login through RDP for the custom workspaces.

The instance-id is used as default password for `ec2-user` and is setup during the
user-data script execution for Linux based desktop workspace.

To use the new connection type, update the workspace's cloudformation template output
section. Refer [this](https://github.com/HicResearch/treehoose-ec2-builder/blob/main/service-catalog-products/al2-mate.cfn.yml)
template for an example.

> For any new custom Linux based workspaces, ensure JQ and FUSE are pre-installed
> in the base AMI.

The [bootstrap](../src/main/solution/post-deployment/config/environment-files/bootstrap.sh#L211) script for
Linux based workspaces is changed to enable auto-mounting of studies on GUI login.

The above steps can be used to enable new connection schemes for workspaces, example VNC or DCV.

## Enable use of Elastic Fleets in AppStream

The default [on-boarding template](../src/addons/addon-base-raas/packages/base-raas-cfn-templates/src/templates/onboard-account.cfn.yml)
for SWB is updated to allow use of Elastic AppStream fleet types.

In addition, changes are made to the
[UI form validations](../src/addons/addon-base-raas-ui/packages/base-raas-ui/src/models/forms/AddUpdateAwsAccountForm.js)
for onboarding an existing AWS account as hosting account.

> Form validations for the SWB feature allowing to create a new AWS account
> to use as hosting account, are not changed. For Trusted Research Environment
> implementations, its expected that an existing account will always
> be used for hosting workspaces.

## Simplify user login experience

The user login simplification is dependent on
[appstream application](https://github.com/HicResearch/treehoose-appstream-appbuilder) setup as outlined in the link.
The applications deployed by the `treehoose-appstream-appbuilder` add-on and the application names
specifically are important for this customisation to work.

Below are the simplifications done for different type of connections:

- RDP - launches Remmina application with a preset connection file. The connection
  file is created using the information provided in the session context for AppStream.
  By default the context information contains, private IP address of workspace, connection scheme
  and username. You can optionally include password by setting `ENABLE_PASSWORD_SHARE` flag
  to `true` in
  [this](../src/addons/addon-base-raas-appstream/packages/base-raas-appstream-services/lib/appstream/appstream-sc-service.js)
  file.
- SSH - automatically launches `terminal` and starts a helper script.
  The user needs to paste their private key.
  User-name and connection IP address are picked from session context
  and an ssh session is automatically established.
- HTTP - automatically launches firefox in `kiosk` mode when AppStream session starts.
  User only needs to copy paste the SageMaker notebook url.
  The SageMaker notebook url is updated to launch the Jupyter lab as a default.
  > Url can not be passed in context info for the AppStream session as it breaches
  > 1000 character limit.
