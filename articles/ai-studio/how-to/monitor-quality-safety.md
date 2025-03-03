---
title: Monitor quality and safety of deployed applications 
titleSuffix: Azure AI Studio
description: Learn how to monitor quality and safety of deployed applications with Azure AI Studio.
manager: scottpolly
ms.service: azure-ai-studio
ms.custom:
  - ignite-2023
ms.topic: how-to
ms.date: 11/15/2023
ms.reviewer: fasantia
ms.author: mopeakande
author: msakande
---

# Monitor quality and safety of deployed applications 

Monitoring models that are deployed in production is an essential part of the generative AI application lifecycle. Changes in data and consumer behavior can influence your application over time, resulting in outdated systems that negatively affect business outcomes and expose organizations to compliance, economic, and reputational risks. 

Azure AI model monitoring for generative AI applications makes it easier for you to monitor your applications in production for safety and quality on a cadence to ensure it's delivering maximum business value. 

Capabilities and integrations include: 
- Collect production data using Model data collector from a prompt flow deployment.
- Apply Responsible AI evaluation metrics such as groundedness, coherence, fluency, relevance, and similarity, which are interoperable with prompt flow evaluation metrics. 
- Preconfigured alerts and defaults to run monitoring on a recurring basis.
- Consume result and configure advanced behavior in Azure AI Studio.

## Evaluation metrics 

Metrics are generated by the following state-of-the-art GPT language models configured with specific evaluation instructions (prompt templates) which act as evaluator models for sequence-to-sequence tasks. This technique has shown strong empirical results and high correlation with human judgment when compared to standard generative AI evaluation metrics. For more information about prompt flow evaluation, see [Submit bulk test and evaluate a flow](./flow-bulk-test-evaluation.md) and [evaluation and monitoring metrics for generative AI](../concepts/evaluation-metrics-built-in.md).

These GPT models are supported with monitoring and configured as your Azure OpenAI resource: 

- GPT-3.5 Turbo 
- GPT-4 
- GPT-4-32k 

The following metrics are supported for monitoring:

| Metric       | Description |
|--------------|-------------|
| Groundedness | Measures how well the model's generated answers align with information from the source data (user-defined context.) |
| Relevance    | Measures the extent to which the model's generated responses are pertinent and directly related to the given questions. |
| Coherence    | Measures the extent to which the model's generated responses are logically consistent and connected. |
| Fluency      | Measures the grammatical proficiency of a generative AI's predicted answer. |
| Similarity   | Measures the similarity between a source data (ground truth) sentence and the generated response by an AI model. |

## Flow and metric configuration requirements 

When creating your flow, you need to ensure your column names are mapped. The following input data column names are used to measure generation safety and quality: 

| Input column name | Definition | Required |
|------|------------|----------|
| Prompt text | The original prompt given (also known as "inputs" or "question") | Required |
| Completion text | The final completion from the API call that is returned (also known as "outputs" or "answer") | Required |
| Context text | Any context data that is sent to the API call, together with original prompt. For example, if you hope to get search results only from certain certified information sources/website, you can define in the evaluation steps. This is an optional step that can be configured through prompt flow. | Optional |
| Ground truth text | The user-defined text as the "source of truth" | Optional |

What parameters are configured in your data asset dictates what metrics you can produce, according to this table: 

| Metric       | Prompt  | Completion | Context | Ground truth |
|--------------|---------|------------|---------|--------------|
| Coherence    | Required | Required   | -       | -            |
| Fluency      | Required | Required   | -       | -            |
| Groundedness | Required | Required   | Required| -            |
| Relevance    | Required | Required   | Required| -            |
| Similarity   | Required | Required   | -       | Required     |

For more information, see [question answering metric requirements](evaluate-generative-ai-app.md#question-answering-metric-requirements).

## User Experience 

Confirm your flow runs successfully, and that the required inputs and outputs are configured for the metrics you want to assess. The minimum required parameters of collecting only inputs and outputs provide only two metrics: coherence and fluency. You must configure your flow according to the [prior guidance](#flow-and-metric-configuration-requirements).

:::image type="content" source="../media/deploy-monitor/monitor/user-experience.png" alt-text="Screenshot of prompt flow editor with deploy button." lightbox = "../media/deploy-monitor/monitor/user-experience.png":::

Deploy your flow. By default, both inferencing data collection and application insights are enabled automatically. These are required for the creation of your monitor. 

:::image type="content" source="../media/deploy-monitor/monitor/basic-settings.png" alt-text="Screenshot of basic settings in the deployment wizard." lightbox = "../media/deploy-monitor/monitor/basic-settings.png":::

By default, all outputs of your deployment are collected using Azure AI's Model Data Collector. As an optional step, you can enter the advanced settings to confirm that your desired columns (for example, context of ground truth) are included in the endpoint response. 

In summary, your deployed flow needs to be configured in the following way:  

- Flow inputs & outputs: You need to name your flow outputs appropriately and remember these column names when creating your monitor. In this article, we use the following: 
    - Inputs (required): "prompt" 
    - Outputs (required): "completion" 
    - Outputs (optional): "context" and/or "ground truth" 

- Data collection: in the "Deployment" (Step #2 of the prompt flow deployment wizard), the 'inference data collection' toggle must be enabled using Model Data Collector 

- Outputs: In the Outputs (Step #3 of the prompt flow deployment wizard), confirm you have selected the required outputs listed above (for example, completion | context | ground_truth) that meet your metric configuration requirements.

Test your deployment in the deployment **Test** tab. 

:::image type="content" source="../media/deploy-monitor/monitor/test-deploy.png" alt-text="Screenshot of the deployment test page." lightbox = "../media/deploy-monitor/monitor/test-deploy.png":::

 
> [!NOTE]
> Monitoring requires the endpoint to be used at least 10 times to collect enough data to provide insights. If you'd like to test sooner, manually send about 50 rows in the 'test' tab before running the monitor.

Create your monitor by either enabling from the deployment details page, or the **Monitoring** tab.

:::image type="content" source="../media/deploy-monitor/monitor/enable-monitoring.png" alt-text="Screenshot of the button to enable monitoring." lightbox = "../media/deploy-monitor/monitor/enable-monitoring.png":::

Ensure your columns are mapped from your flow as defined in the previous requirements. 

:::image type="content" source="../media/deploy-monitor/monitor/column-map.png" alt-text="Screenshot of columns mapped for monitoring metrics." lightbox = "../media/deploy-monitor/monitor/column-map.png":::


View your monitor in the **Monitor** tab.  

:::image type="content" source="../media/deploy-monitor/monitor/monitor-metrics.png" alt-text="Screenshot of the monitoring result metrics." lightbox = "../media/deploy-monitor/monitor/monitor-metrics.png":::

By default, operational metrics such as requests per minute and request latency show up. The default safety and quality monitoring signal are configured with a 10% sample rate and will run on your default workspace Azure Open AI connection. 

Your monitor is created with default settings:
- 10% sample rate
- 4/5 (thresholds / recurrence)
- Weekly recurrence on Monday mornings
- Alerts are delivered to the inbox of the person that triggered the monitor.

To view more details about your monitoring metrics, you can follow the link to navigate to monitoring in Azure Machine Learning studio, which is a separate studio that allows for more customizations. 


## Next steps

- Learn more about what you can do in [Azure AI Studio](../what-is-ai-studio.md)
- Get answers to frequently asked questions in the [Azure AI FAQ article](../faq.yml)
