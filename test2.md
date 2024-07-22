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
  - Resourceid3
workflow: review
---

# Creating the Kubernetes manifest and Ignition config files

Before you start, review the previous information about DNS configuration: [Example DNS configuration for user-provisioned clusters](test.md#example-dns-configuration-for-user-provisioned-clusters)

Because you must modify some cluster definition files and manually start the cluster machines, you must generate the Kubernetes manifest and Ignition config files that the cluster needs to configure the machines.

The installation configuration file transforms into the Kubernetes manifests. The manifests wrap into the Ignition configuration files, which are later used to configure the cluster machines.

!!! important

    * The Ignition config files that the OpenShift Container Platform installation program generates contain certificates that expire after 24 hours, which are then renewed at that time. If the cluster is shut down before renewing the certificates and the cluster is later restarted after the 24 hours have elapsed, the cluster automatically recovers the expired certificates. The exception is that you must manually approve the pending `node-bootstrapper` certificate signing requests (CSRs) to recover kubelet certificates. See the documentation for _Recovering from expired control plane certificates_ for more information.
    * It is recommended that you use Ignition config files within 12 hours after they are generated because the 24-hour certificate rotates from 16 to 22 hours after the cluster is installed. By using the Ignition config files within 12 hours, you can avoid installation failure if the certificate update runs during installation.

**Prerequisites**

* You obtained the OpenShift Container Platform installation program.
* You created the `install-config.yaml` installation configuration file.

**Procedure**

1. Change to the directory that contains the OpenShift Container Platform installation program and generate the Kubernetes manifests for the cluster:

   ```terminal
   $ ./openshift-install create manifests --dir <installation_directory>
   ```

   For `<installation_directory>`, specify the installation directory that
   contains the `install-config.yaml` file you created.

   !!! warning

       If you are installing a three-node cluster, skip the following step to allow the control plane nodes to be schedulable.

   !!! important

       When you configure control plane nodes from the default unschedulable to schedulable, additional subscriptions are required. This is because control plane nodes then become compute nodes.

2. Check that the `mastersSchedulable` parameter in the `<installation_directory>/manifests/cluster-scheduler-02-config.yml` Kubernetes manifest file is set to `false`. This setting prevents pods from being scheduled on the control plane machines:

   1. Open the `<installation_directory>/manifests/cluster-scheduler-02-config.yml` file.
   2. Locate the `mastersSchedulable` parameter and ensure that it is set to `false`.
   3. Save and exit the file.

3. To create the Ignition configuration files, run the following command from the directory that contains the installation program:

   ```terminal
   $ ./openshift-install create ignition-configs --dir <installation_directory>
   ```
   For `<installation_directory>`, specify the same installation directory.

   Ignition config files are created for the bootstrap, control plane, and compute nodes in the installation directory. The `kubeadmin-password` and `kubeconfig` files are created in the `./<installation_directory>/auth` directory:

      ```terminal
      .
      ├── auth
      │   ├── kubeadmin-password
      │   └── kubeconfig
      ├── bootstrap.ign
      ├── master.ign
      ├── metadata.json
      └── worker.ign
      ```
