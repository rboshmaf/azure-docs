---
title: Create a High Availability Pacemaker cluster provider for Azure Monitor for SAP solutions (preview)
description: Learn how to configure High Availability (HA) Pacemaker cluster providers for Azure Monitor for SAP solutions (AMS).
author: MightySuz
ms.service: virtual-machines-sap
ms.subservice: baremetal-sap
ms.topic: how-to
ms.date: 07/28/2022
ms.author: sujaj
#Customer intent: As a developer, I want to create a High Availability Pacemaker cluster so I can use the resource with Azure Monitor for SAP solutions.
---

# Create High Availability cluster provider for Azure Monitor for SAP solutions (preview)

[!INCLUDE [Azure Monitor for SAP solutions public preview notice](./includes/preview-azure-monitor.md)]

In this how-to guide, you'll learn to create a High Availability (HA) Pacemaker cluster provider for Azure Monitor for SAP solutions (AMS). You'll install the HA agent, then create the provider for AMS. 

This content applies to both AMS and AMS (classic) versions.

## Prerequisites

- An Azure subscription. 
- An existing AMS resource. To create an AMS resource, see the [quickstart for the Azure portal](azure-monitor-sap-quickstart.md) or the [quickstart for PowerShell](azure-monitor-sap-quickstart-powershell.md).

## Install HA agent

Before adding providers for HA (Pacemaker) clusters, install the appropriate agent for your environment in each cluster node.

For SUSE-based clusters, install **ha_cluster_provider** in each node. For more information, see [the HA cluster exporter installation guide](https://github.com/ClusterLabs/ha_cluster_exporter#installation). Supported SUSE versions include SLES for SAP 12 SP3 and above.

For RHEL-based clusters, install **performance co-pilot (PCP)** and the **pcp-pmda-hacluster** sub package in each node.For more information, see the [PCP HACLUSTER agent installation guide](https://access.redhat.com/articles/6139852). Supported RHEL versions include 8.2, 8.4 and above.

For RHEL-based pacemaker clusters, also install [PMProxy](https://access.redhat.com/articles/6139852) in each node.


## Create provider for AMS

1. Sign in to the [Azure portal](https://portal.azure.com).
1. Go to the AMS service. 
1. Open your AMS resource.
1. In the resource's menu, under **Settings**, select **Providers**.
1. Select **Add** to add a new provider.

    ![Diagram of Azure Monitor for SAP solutions resource in the Azure portal, showing button to add a new provider.](./media/azure-monitor-sap/azure-monitor-providers-ha-cluster-start.png)  

1. For **Type**, select **High-availability cluster (Pacemaker)**.
1. Configure providers for each node of the cluster by entering the endpoint URL for **HA Cluster Exporter Endpoint**. 

    1. For SUSE-based clusters, enter `http://<'IP address'> :9664/metrics`. 

    ![Diagram of the setup for an Azure Monitor for SAP solutions resource, showing the fields for SUSE-based clusters.](./media/azure-monitor-sap/azure-monitor-providers-ha-cluster-suse.png)  

    
    1. For RHEL-based clusters, enter `http://<'IP address'>:44322/metrics?names=ha_cluster`.

    ![Diagram of the setup for an Azure Monitor for SAP solutions resource, showing the fields for RHEL-based clusters.](./media/azure-monitor-sap/azure-monitor-providers-ha-cluster-rhel.png)  


1. Enter the system identifiers, host names, and cluster names. For the system identifier, enter a unique SAP system identifier for each cluster. For the hostname, the value refers to an actual hostname in the VM. Use `hostname -s` for SUSE- and RHEL-based clusters.

1. Select **Add provider** to save.

1. Continue to add more providers as needed.

1. Select **Review + create** to review the settings.

1. Select **Create** to finish creating the resource.

## Next steps

> [!div class="nextstepaction"]
> [Learn about AMS provider types](azure-monitor-providers.md)
