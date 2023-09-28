
# WARNING
Not all scripts and functionality in this repo have been tested to work on the spinnaker-bastion. Only the following scripts and commands have been tested to work. 


# Manage Spinnaker

Use this section to manage your Spinnaker deployment going forward.

### Ensure you are connected to the correct Kubernetes context

```bash
PROJECT_ID=ua-ops-deploy ~/cloudshell_open/spinnaker-for-gcp/scripts/manage/check_cluster_config.sh
```

### Pull Spinnaker config

Paste and run this command to pull the configuration from your Spinnaker
deployment into your Cloud Shell.

```bash
~/cloudshell_open/spinnaker-for-gcp/scripts/manage/pull_config.sh
```
### Configure Spinnaker via Halyard

All [halyard](https://www.spinnaker.io/reference/halyard/commands/) commands are available.

```bash
hal config
```

As with provisioning Spinnaker, don't use `hal deploy connect` when managing
Spinnaker. Also, don't use `hal deploy apply`. Instead, use the `push_and_apply.sh`
command shown below.

### Notes on Halyard commands that reference local files

If you add a Kubernetes account that references a kubeconfig file, that file must live within
the '`~/.hal/default/credentials`' directory on your VM. The
kubeconfig is specified using the `--kubeconfig-file` argument to the
`hal config provider kubernetes account add` and ...`edit` commands.

A similar requirement applies for any other local file referenced from your halyard config,
including Google JSON key files specified via the `--json-path` argument to various commands.
These files must live within '`~/.hal/default/credentials`' or '`~/.hal/default/profiles`'.

### Push and apply updated config to Spinnaker deployment

If you change any of the configuration, paste and run this command to push
and apply those changes to your Spinnaker deployment.

```bash
~/cloudshell_open/spinnaker-for-gcp/scripts/manage/push_and_apply.sh
```

### Upgrade Spinnaker

First, modify `SPINNAKER_VERSION` in your `properties` file to reflect the desired version of Spinnaker:

```bash
cloudshell edit ~/cloudshell_open/spinnaker-for-gcp/scripts/install/properties
```

Next, use Halyard to apply the changes:

```bash
~/cloudshell_open/spinnaker-for-gcp/scripts/manage/update_spinnaker_version.sh
```

### Upgrade Halyard daemon running in cluster

First, modify `HALYARD_VERSION` in your `properties` file to reflect the desired version of Halyard:

```bash
cloudshell edit ~/cloudshell_open/spinnaker-for-gcp/scripts/install/properties
```

Next, apply this change to the Statefulset managing the Halyard daemon:

```bash
~/cloudshell_open/spinnaker-for-gcp/scripts/manage/update_halyard_daemon.sh
```

### Restore a backup to home directory

Restore a backup of the halyard configuration and deployment configuration from Cloud Source Repositories to your home directory. 

```bash
~/cloudshell_open/spinnaker-for-gcp/scripts/manage/restore_backup_to_cloud_shell.sh -p ua-ops-deploy -r spinnaker-1-config -h GIT_HASH
```

All backups can be viewed in this [Cloud Source Repository](https://source.cloud.google.com/ua-ops-deploy/spinnaker-1-config).
