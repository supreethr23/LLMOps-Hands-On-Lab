## Lab 04: Monitoring and Responsible AI 

### Estimated Time: 60 minutes

## Lab Objectives

After completing this lab, you will be able to complete the following tasks:

- **Task 01:** Monitoring your LLM's flow
- **Task 02:** Add Content Safety to Your Solution

## Task 01: Monitoring your LLM's flow

1. Once the **Multi-Round Q&A on Your Data** flow deployment succeeds to an online endpoint, navigate to the **Components > Deployments** section and select your recently created deployment.

1. On the **Details** tab, click on **Enable** within the **Enable generation quality monitoring** box.

   ![](media/enable-monitoring.png)

1. On the **Enable monitoring** window, select all the metrics to monitor, **configure column mapping**, select your **Azure OpenAI Connection** and **Deployment**, and click on **Create**.

   >**Note:** Monitoring sets the default sampling rate at 10%. This means that if 100 requests are sent to your deployment, 10 get sampled and used to compute the generation quality metrics. You can adjust the sampling rate in the settings.

   ![](media/enable-monitoring-create.png)

1. On the **Operational** tab, view the operational metrics for the deployment in near real-time. The supported metrics are:

   - Requests per minute
   - Requests latency
   - Error rate
  
   ![](media/monitoring-operational-tab.png)

1. The results in the **Monitoring (preview)** tab of your deployment provide insights to help you proactively improve the performance of your prompt flow application.

## Task 02: Add Content Safety to Your Solution

1. In your Azure AI Studio, navigate to **Tools > Prompt flow** and click on **+ Create**.

   ![](media/+create-prompt-flow.png)

1. On the **Create a new flow** window, click on **Create** for **Standard flow**.

   ![](media/standard-flow-create.png)

1. In the **Create a new flow** window, name the folder **standard-joke-flow** and click on **Create**.

   ![](media/standard-joke-flow-create.png)

1. A **standard-joke-flow** will be created in the following structure.

   ![](media/standard-joke-flow-structure.png)

1. In the **standard-joke-flow**, delete the **echo** node, as you will be adding two Python nodes to process the output from the **Content Safety** tool. Determine whether to proceed with the standard flow or not, and craft a default response. Click on the **Delete step** once the **Confirm Deletion** pop-up appears.

   ![](media/standard-flow-delete-echo.png)

1. Notice that the outputs node disappears from the graph since the **outputs** node was configured to fetch the output value from the **echo** node.

1. Navigate to the **Outputs** section in the **standard-joke-flow** and change the **output** value to **${joke.output}**.

   ![](media/standard-flow-joke-ouput.png)

1. Now let's add a **Content Safety text** tool to the flow. Click on **+ More tools (1)** and select **Content Safety (Text Analyze) (2)**.

   >**Note:** Start the compute session for your standard-joke-flow to add more tools.

   ![](media/more-tools-content-safety.png)

1. Enter the node name as **content_safety** and click on **Add**.

   ![](media/content-safety-name-add.png)

1. Configure the inputs for the **content_safety** tool by selecting the following values:

   - connection: **Content Safety resource**
   - text: **${inputs.topic}**

   ![](media/config-content-safety-inputs.png)

1. Add a **Python** node to process the output from the **content_safety** tool and determine whether to proceed with the standard flow or not. Click on **+ Python**, name the node **content_safety_check,** and click on **Add**.

   ![](media/content_safety-check-name-add.png)

1. Navigate to the **C:\LabFiles** folder to locate the **content_safety_check.py** file. Copy the content of this file into the **content_safety_check** node, click on **Validate and parse input**, and configure the **input1** value as **${content_safety.output}**.

   ![](media/config-content-safety-check.png)

1. Add a **Python** node to craft a default response. Click on **+ Python**, name the node **generate_result,** and click on **Add**.

   ![](media/generate-result-name-add.png)

1. Navigate to the **C:\LabFiles** folder to locate the **generate_result.py** file. Copy the content of this file into the **generate_result** node, click on **Validate and parse input,** and configure the following inputs.

   - default_result: **${content_safety_check.output}**
   - llm_result: **${joke.output}**

   ![](media/config-generate-result.png)

1. Navigate back to the **Outputs** section in the standard flow and change the **output** value to **${generate_result.output}**.

   ![](media/config-outputs-generate-result.png)

1. Define the **Connection** with the LLM for the **joke** node by selecting the **Default_AzureOpenAI Connection** and **gpt-4** deployment, which connects to the Azure OpenAI resource that was created when the Azure AI project was set up.

   ![](media/config-joke-llm-connection.png)

1. After configuring all the nodes, your standard joke flow will have the following structure:

   ![](media/standard-joke-flow-final.png)

1. Flow Overview:

   - The **joke** node, combined with the **content_safety** node, detects any harmful content from different modalities and languages in the input topic.
   - The **content_safety_check** python node is setting up a function (content_safety_check) decorated with @tool, which may integrate additional functionality provided by the tool decorator. The function itself uses the random module to randomly determine the safety of the content by returning either true or false.
   - The **generate_result** python node returns a specified result (llm_result) if available, or a default result (default_result) if llm_result is not provided or is empty.

1. To run the flow, enter an offensive topic as the input for the joke to be generated, and click on **Run**.

   ![](media/ca.png)

1. Notice how the **content_safety** node rejects offensive or hateful input topics.

   ![](media/content-safety-reject.png)

1. Also, notice in the **Outputs** in the **generate_result** node that it doesn't generate the joke on the offensive/hateful topic and provides an alternate lighthearted and non-offensive joke.

   ![](media/generate-result-joke-output.png)

## Review

In this lab, you have performed  the following tasks:

- Monitored your LLM's flow.
- Added content safety to your solution.

### You have successfully completed the lab.

















