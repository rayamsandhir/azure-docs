---
ms.author: dacurwin
author: dcurwin
ms.service: defender-for-cloud
ms.topic: include
ms.date: 06/01/2023
---

## Enable the plan

**To enable the plan**:

1. From Defender for Cloud's menu, open the Settings page and select the relevant subscription.

1. In the [Defender plans page](https://portal.azure.com/#blade/Microsoft_Azure_Security/SecurityMenuBlade/pricingTier), select **Defender for Containers** and select **Settings**.

    :::image type="content" source="../media/defender-for-containers-enable-plan-gke/containers-settings.png" alt-text="screenshot of Defender plans page." lightbox="../media/defender-for-containers-enable-plan-gke/containers-settings.png":::

    > [!TIP]
    > If the subscription already has Defender for Kubernetes and/or Defender for container registries enabled, an update notice is shown. Otherwise, the only option will be **Defender for Containers**.
    >
    > :::image type="content" source="../media/release-notes/defender-plans-deprecated-indicator.png" alt-text="Defender for container registries and Defender for Kubernetes plans showing 'Deprecated' and upgrade information.":::

1. Turn the relevant component on to enable it.

    :::image type="content" source="../media/defender-for-containers-enable-plan-gke/container-components-on.png" alt-text="screenshot of turning on components." lightbox="../media/defender-for-containers-enable-plan-gke/container-components-on.png":::

    > [!NOTE]
    >
    > - Defenders for Containers customers who joined before August 2023 and don't have Agentless discovery for Kubernetes enabled as part of Defender CSPM when they enabled the plan, must manually enable the Agentless discovery for Kubernetes extension within the Defender for Containers plan.
    > - When you turn off Defender for Containers, the components are set to off and are not deployed to any more containers but they are not removed from containers that they are already installed on.

<BR>

### Enablement method per capability

By default, when enabling the plan through the Azure portal, [Microsoft Defender for Containers](../defender-for-containers-introduction.md) is configured to automatically enable all capabilities and install all required components to provide the protections offered by the plan, including the assignment of a default workspace.

If you don't want to enable all capabilities of the plans, you can manually select which specific capabilities to enable by selecting **Edit configuration** for the **Containers** plan. Then, in the **Settings & monitoring** page, select the capabilities you want to enable.
In addition, you can modify this configuration from the [Defender plans page](https://portal.azure.com/#blade/Microsoft_Azure_Security/SecurityMenuBlade/pricingTier) after initial configuration of the plan.

The following table provides detailed information on enablement method for each one of the capabilities:

| Domain | Feature | Description | Enablement method | Agents | Azure clouds availability |
|--|--|--|--|--|--|
| Security posture management  | Agentless discovery for Kubernetes | Provides zero footprint, API-based discovery of your Kubernetes clusters, their configurations and deployments.| Enable **Agentless discovery on Kubernetes** toggle | Agentless | Azure commercial clouds |
| Security posture management  | Comprehensive inventory capabilities | Enables you to explore resources, pods, services, repositories, images, and configurations through [security explorer](..\how-to-manage-cloud-security-explorer.md#build-a-query-with-the-cloud-security-explorer) to easily monitor and manage your assets. | Enable **Agentless discovery on Kubernetes** toggle | Agentless | Azure commercial clouds |
| Security posture management  | Enhanced risk-hunting | Enables security admins to actively hunt for posture issues in their containerized assets through queries (built-in and custom) and [security insights](..\attack-path-reference.md#insights) in the [security explorer](..\how-to-manage-cloud-security-explorer.md). | Enable **Agentless discovery on Kubernetes** toggle | Agentless | Azure commercial clouds |
| Security posture management  | [Control plane hardening](..\defender-for-containers-architecture.md) | Continuously assesses the configurations of your clusters and compares them with the initiatives applied to your subscriptions. When it finds misconfigurations, Defender for Cloud generates security recommendations that are available on Defender for Cloud's Recommendations page. The recommendations let you investigate and remediate issues. | Activated with plan | Agentless | Commercial clouds<br><br> National clouds: Azure Government, Azure operated by 21Vianet |
| Security posture management  | [Kubernetes data plane hardening](..\kubernetes-workload-protections.md) | Protect workloads of your Kubernetes containers with best practice recommendations. | Enable **Azure Policy for Kubernetes** toggle | Azure policy agent| Commercial clouds<br><br> National clouds: Azure Government, Azure operated by 21Vianet |
| Vulnerability assessment | Agentless vulnerability scan for registry images (powered by Qualys) | Vulnerability assessment for images in ACR | Activated with plan | Agentless | Commercial clouds<br><br> National clouds: Azure Government, Azure operated by 21Vianet |
| Vulnerability assessment | Agentless vulnerability scan for running images (powered by Qualys) | Vulnerability assessment for running images in AKS | Activated with plan | Defender agent | Commercial clouds<br><br> National clouds: Azure Government, Azure operated by 21Vianet |
| Vulnerability assessment | Agentless registry scan (powered by Microsoft Defender Vulnerability Management) |  Vulnerability assessment for images in ACR | Enable **Agentless container vulnerability assessment** toggle | Agentless | Commercial clouds |
| Vulnerability assessment | Agentless/agent-based runtime (powered by Microsoft Defender Vulnerability Management)| Vulnerability assessment for running images in AKS | Enable **Agentless container vulnerability assessment** toggle | Agentless **OR** install Defender agent for shorter refresh rate | Commercial clouds |
| Runtime threat protection | Control plane threat detection | Detection of suspicious activity for Kubernetes based on Kubernetes audit trail | Enabled with plan | Agentless | Commercial clouds<br><br> National clouds: Azure Government, Azure operated by 21Vianet |
| Runtime threat protection | Workload threat detection | Detection of suspicious activity for Kubernetes for cluster level, node level, and workload level | Enable **Defender Agent in Azure** toggle **OR** deploy Defender agent on individual clusters | Defender agent | Commercial clouds |
| Deployment & monitoring | Discovery of unprotected clusters | Discovering Kubernetes clusters missing Defender agents| Enabled with plan | Agentless | Commercial clouds<br><br> National clouds: Azure Government, Azure operated by 21Vianet |
| Deployment & monitoring | Defender agent auto provisioning | Automatic deployment of Defender agent | Enable **Defender Agent in Azure** toggle | Agentless | Commercial clouds<br><br> National clouds: Azure Government, Azure operated by 21Vianet |
| Deployment & monitoring | Azure Policy for Kubernetes auto provisioning | Automatic deployment of Azure policy agent for Kubernetes | Enable **Azure policy for Kubernetes** toggle | Agentless | Commercial clouds<br><br> National clouds: Azure Government, Azure operated by 21Vianet |

### Roles and permissions

Learn more about the [roles used to provision Defender for Containers extensions](../permissions.md#roles-used-to-automatically-provision-agents-and-extensions).

### Assigning custom workspace for Defender agent

You can [assign a custom workspace](../defender-for-containers-enable.md?pivots=defender-for-container-aks&tabs=aks-deploy-portal%2ck8s-deploy-asc%2ck8s-verify-asc%2ck8s-remove-arc%2caks-removeprofile-api#assign-a-custom-workspace) through Azure Policy.

### Manual deployment of Defender agent or Azure policy agent without auto-provisioning using recommendations

Capabilities that require agent installation can also be deployed on one or more Kubernetes clusters, using the appropriate recommendation:

| Agent | Recommendation |
|--|--|
| Defender Agent for Kubernetes | [Azure Kubernetes Service clusters should have Defender profile enabled](https://portal.azure.com/#blade/Microsoft_Azure_Security/RecommendationsBlade/assessmentKey/56a83a6e-c417-42ec-b567-1e6fcb3d09a9) |
| Defender Agent for Arc-enabled Kubernetes | [Azure Arc-enabled Kubernetes clusters should have the Defender extension installed](https://portal.azure.com/#blade/Microsoft_Azure_Security/RecommendationsBlade/assessmentKey/3ef9848c-c2c8-4ff3-8b9c-4c8eb8ddfce6) |
| Azure policy agent for Kubernetes | [Azure Kubernetes Service clusters should have the Azure Policy Add-on for Kubernetes installed](https://portal.azure.com/#blade/Microsoft_Azure_Security/RecommendationsBlade/assessmentKey/08e628db-e2ed-4793-bc91-d13e684401c3) |
| Azure policy agent for Arc-enabled Kubernetes | [Azure Arc-enabled Kubernetes clusters should have the Azure Policy extension installed](https://portal.azure.com/#blade/Microsoft_Azure_Security/RecommendationsBlade/assessmentKey/0642d770-b189-42ef-a2ce-9dcc3ec6c169) |

Perform the following steps to perform deployment of the Defender agent on specific clusters:

1. From Microsoft Defender for Cloud's recommendations page, open the **Enable enhanced security** security control or search directly for one of the above recommendations (or use the above links to open the recommendation directly)

1. View all clusters without an agent via the unhealthy tab.

1. Select the clusters to deploy the desired agent on and select **Fix**.

1. Select **Fix X resources**.

## Deploying Defender agent - all options

You can enable the Defender for Containers plan and deploy all of the relevant components from the Azure portal, the REST API, or with a Resource Manager template. For detailed steps, select the relevant tab.

Once the Defender agent has been deployed, a default workspace is automatically assigned. You can [assign a custom workspace](../defender-for-containers-enable.md?pivots=defender-for-container-aks&tabs=aks-deploy-portal%2ck8s-deploy-asc%2ck8s-verify-asc%2ck8s-remove-arc%2caks-removeprofile-api#assign-a-custom-workspace) in place of the default workspace through Azure Policy.

> [!NOTE]
> The Defender agent is deployed to each node to provide the runtime protections and collect signals from those nodes using [eBPF technology](https://ebpf.io/).

### [**Azure portal**](#tab/aks-deploy-portal)

### Use the fix button from the Defender for Cloud recommendation

A streamlined, frictionless, process lets you use the Azure portal pages to enable the Defender for Cloud plan and setup auto provisioning of all the necessary components for defending your Kubernetes clusters at scale.

A dedicated Defender for Cloud recommendation provides:

- **Visibility** about which of your clusters has the Defender agent deployed
- **Fix** button to deploy it to those clusters without the agent

1. From Microsoft Defender for Cloud's recommendations page, open the **Enable enhanced security** security control.

1. Use the filter to find the recommendation named **Azure Kubernetes Service clusters should have Defender profile enabled**.

    > [!TIP]
    > Notice the **Fix** icon in the actions column

1. Select the clusters to see the details of the healthy and unhealthy resources - clusters with and without the agent.

1. From the unhealthy resources list, select a cluster and select **Remediate** to open the pane with the remediation confirmation.

1. Select **Fix X resources**.

### [**REST API**](#tab/aks-deploy-rest)

### Use the REST API to deploy the Defender agent

To install the 'SecurityProfile' on an existing cluster with the REST API, run the following PUT command:

```rest
PUT https://management.azure.com/subscriptions/{{Subscription Id}}/resourcegroups/{{Resource Group}}/providers/Microsoft.Kubernetes/connectedClusters/{{Cluster Name}}/providers/Microsoft.KubernetesConfiguration/extensions/microsoft.azuredefender.kubernetes?api-version=2020-07-01-preview
```

Request URI: `https://management.azure.com/subscriptions/{{SubscriptionId}}/resourcegroups/{{ResourceGroup}}/providers/Microsoft.ContainerService/managedClusters/{{ClusterName}}?api-version={{ApiVersion}}`

Request query parameters:

| Name           | Description                        | Mandatory |
|----------------|------------------------------------|-----------|
| SubscriptionId | Cluster's subscription ID          | Yes       |
| ResourceGroup  | Cluster's resource group           | Yes       |
| ClusterName    | Cluster's name                     | Yes       |
| ApiVersion     | API version, must be >= 2022-06-01 | Yes       |

Request Body:

```rest
{
  "location": "{{Location}}",
  "properties": {
    "securityProfile": {
            "defender": {
                "logAnalyticsWorkspaceResourceId": "{{LAWorkspaceResourceId}}",
                "securityMonitoring": {
                    "enabled": true,
                }
            }
        }
    }
}
```

Request body parameters:

| Name | Description | Mandatory |
|--|--|--|
| location | Cluster's location | Yes |
| properties.securityProfile.defender.securityMonitoring.enabled | Determines whether to enable or disable Microsoft Defender for Containers on the cluster | Yes |
| properties.securityProfile.defender.logAnalyticsWorkspaceResourceId | Log Analytics workspace Azure resource ID | Yes |

### [**Azure CLI**](#tab/k8s-deploy-cli)

### Use Azure CLI to deploy the Defender agent

1. Sign in to Azure:

    ```azurecli
    az login
    az account set --subscription <your-subscription-id>
    ```

    > [!IMPORTANT]
    > Ensure that you use the same subscription ID for ``<your-subscription-id>`` as the one associated with your AKS cluster.

1. Enable the Defender agent on your containers:

    - Run the following command to create a new cluster with the Defender agent enabled:

        ```azurecli
        az aks create --enable-defender --resource-group <your-resource-group> --name <your-cluster-name>
        ```

    - Run the following command to enable the Defender agent on an existing cluster:

        ```azurecli
        az aks update --enable-defender --resource-group <your-resource-group> --name <your-cluster-name>
        ```

    A description of all the supported configuration settings on the Defender agent type is given below:

    | Property | Description |
    |----------|-------------|
    | logAnalyticsWorkspaceResourceId | **Optional**. Full resource ID of your own Log Analytics workspace.<br>When not provided, the default workspace of the region will be used.<br><br>To get the full resource ID, run the following command to display the list of workspaces in your subscriptions in the default JSON format:<br>```az resource list --resource-type Microsoft.OperationalInsights/workspaces -o json```<br><br>The Log Analytics workspace resource ID has the following syntax:<br>/subscriptions/{your-subscription-id}/resourceGroups/{your-resource-group}/providers/Microsoft.OperationalInsights/workspaces/{your-workspace-name}. <br>Learn more in [Log Analytics workspaces](../../azure-monitor/logs/log-analytics-workspace-overview.md) |

    You can include these settings in a JSON file and specify the JSON file in the `az aks create` and `az aks update` commands with this parameter: `--defender-config <path-to-JSON-file>`. The format of the JSON file must be:

    ```json
    {"logAnalyticsWorkspaceResourceId": "<workspace-id>"}
    ```

    Learn more about AKS CLI commands in [az aks](/cli/azure/aks).

1. To verify that the agent was successfully added, run the following command on your machine with the `kubeconfig` file pointed to your cluster:

    ```console
    kubectl get pods -n kube-system
    ```

    When the agent is added, you should see a pod called `microsoft-defender-XXXXX` in `Running` state. It might take a few minutes for pods to be added.

### [**Resource Manager**](#tab/aks-deploy-arm)

### Use Azure Resource Manager to deploy the Defender agent

To use Azure Resource Manager to deploy the Defender agent, you'll need a Log Analytics workspace on your subscription. Learn more in [Log Analytics workspaces](../../azure-monitor/logs/log-analytics-workspace-overview.md).

> [!TIP]
> If you're new to Resource Manager templates, start here: [What are Azure Resource Manager templates?](../../azure-resource-manager/templates/overview.md)

To install the 'SecurityProfile' on an existing cluster with Resource Manager:

```json
{ 
    "type": "Microsoft.ContainerService/managedClusters", 
    "apiVersion": "2022-06-01", 
    "name": "string", 
    "location": "string",
    "properties": {
        …
        "securityProfile": { 
            "defender": { 
                "logAnalyticsWorkspaceResourceId": “logAnalyticsWorkspaceResourceId",
                "securityMonitoring": {
                    "enabled": true
                }
            }
        }
    }
}
```
