---
author:
  - Aidan Reilly
  - Author Two
source: Source
publisher: Publisher
permissions: Permissions
audience: Audience
category: Category
keyword:
  - Keyword1
  - Keyword2
resourceid:
  - Resourceid1
  - Resourceid2
workflow: review
---

# Generating a key pair for cluster node SSH access  {#this-is-the-id .this-is-an-output-class}

During an OpenShift Container Platform installation, you can provide an SSH public key to the installation program. The key is passed to the Red Hat Enterprise Linux CoreOS (RHCOS) nodes through their Ignition config files and is used to authenticate SSH access to the nodes. The key is added to the `~/.ssh/authorized_keys` list for the `core` user on each node, which enables password-less authentication.

After the key is passed to the nodes, you can use the key pair to SSH in to the RHCOS nodes as the user `core`. To access the nodes through SSH, the private key identity must be managed by SSH for your local user.

If you want to SSH in to your cluster nodes to perform installation debugging or disaster recovery, you must provide the SSH public key during the installation process. The `./openshift-install gather` command also requires the SSH public key to be in place on the cluster nodes.

Before we start, look at this yaml snippet: 

```yaml
resources:
  '*':
    limits:
      memory: 4Gi
    requests:
      cpu: 100m
      memory: 200Mi
```

Definition list first term
: This is the definition of the first term.

Definition list Second Term
: This is one definition of the second term.
: This is another definition of the second term.

**Procedure**

1. If you do not have an existing SSH key pair on your local machine to use for authentication onto your cluster nodes, create one. For example, on a computer that uses a Linux operating system, run the following command:

   ```terminal
   $ ssh-keygen -t ed25519 -N '' -f <path>/<file_name>
   ```

   !!! important

       Specify the path and file name, such as `~/.ssh/id_ed25519`, of the new SSH key. If you have an existing key pair, ensure your public key is in the your `~/.ssh` directory.

2. View the public SSH key:

   ```terminal
   $ cat <path>/<file_name>.pub
   ```

   For example, run the following to view the `~/.ssh/id_ed25519.pub` public key:

   ```termanal
   $ cat ~/.ssh/id_ed25519.pub
   ```

3. Add the SSH private key identity to the SSH agent for your local user, if it has not already been added. SSH agent management of the key is required for password-less SSH authentication onto your cluster nodes, or if you want to use the `./openshift-install gather` command.

   1. If the `ssh-agent` process is not already running for your local user, start it as a background task:

      ```terminal
      $ eval "$(ssh-agent -s)"
      ```

      **Example output**

      ```terminal
      Agent pid 31874
      ```

4. Add your SSH private key to the `ssh-agent`:

   ```terminal
   $ ssh-add <path>/<file_name>
   ```
   * Specify the path and file name for your SSH private key, such as `~/.ssh/id_ed25519`

      **Example output**

      ```terminal
      Identity added: /home/<you>/<path>/<file_name> (<computer_name>)
      ```

   **Minimum required hosts**

   | Hosts | Description |
   | ---- | ---- |
   | One temporary bootstrap machine | The cluster requires the bootstrap machine to deploy the OpenShift Container Platform cluster on the three control plane machines. You can remove the bootstrap machine after you install the cluster. |
   | Three control plane machines | The control plane machines run the Kubernetes and OpenShift Container Platform services that form the control plane. |
   | At least two compute machines, which are also known as worker machines. | The workloads requested by OpenShift Container Platform users run on the compute machines. |

## Example DNS configuration for user-provisioned clusters

This section provides A and PTR record configuration samples that meet the DNS requirements for deploying OpenShift Container Platform on user-provisioned infrastructure. The samples are not meant to provide advice for choosing one DNS solution over another.

In the examples, the cluster name is `ocp4` and the base domain is `example.com`.

The following example is a BIND zone file that shows sample A records for name resolution in a user-provisioned cluster.

**Sample DNS zone database**

```terminal
$TTL 1W
@  IN SOA   ns1.example.com.  root (
         2019070700  ; serial
         3H    ; refresh (3 hours)
         30M      ; retry (30 minutes)
         2W    ; expiry (2 weeks)
         1W )     ; minimum (1 week)
   IN NS ns1.example.com.
   IN MX 10 smtp.example.com.
;
;
ns1.example.com.     IN A  192.168.1.5
smtp.example.com.    IN A  192.168.1.5
;
helper.example.com.     IN A  192.168.1.5
helper.ocp4.example.com.   IN A  192.168.1.5
;
api.ocp4.example.com.      IN A  192.168.1.5
api-int.ocp4.example.com.  IN A  192.168.1.5
;
*.apps.ocp4.example.com.   IN A  192.168.1.5
;
bootstrap.ocp4.example.com.   IN A  192.168.1.96
;
control-plane0.ocp4.example.com. IN A  192.168.1.97
control-plane1.ocp4.example.com. IN A  192.168.1.98
control-plane2.ocp4.example.com. IN A  192.168.1.99
;
compute0.ocp4.example.com. IN A  192.168.1.11
compute1.ocp4.example.com. IN A  192.168.1.7
;
;EOF
```