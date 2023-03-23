# Deployment

**Time to deploy**: Approximately 30 minutes

The steps will be done in TRE project account where the Amazon Machine Images need to be created.
You can choose to do this in centralised account if using the centralised AMI management
capability of SWB.

## Log in to the EC2 instance

- [ ] Logon to AWS the management console for TRE project account.
- [ ] Follow these [instructions](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/session-manager.html)
      to learn how to
      connect via SSM to the TRE deployment EC2 instance.
- [ ] Run the following command to initialise your environment:

  ```shell
  sudo -iu ec2-user
  ```

## Download repo

- [ ] Download the source code repo using below command. You would need to provide your
      git credentials if required.

  ```console
  git clone https://github.com/HicResearch/treehoose-swb-customisations.git /home/ec2-user/tmp/tre/src
  ```

## Copy customised files

- [ ] Copy over customised files to SWB code and overwrite existing files

  ```console
  cp -R /home/ec2-user/tmp/tre/src/treehoose-swb-customisations/src/* /home/ec2-user/tmp/service-workbench-on-aws-5.2.7/
  ```

## Redeploy service workbench

Run below command to re-deploy service workbench

```bash
cd /home/ec2-user/tmp/service-workbench-on-aws-5.2.7
./scripts/environment-deploy.sh treprod
```
