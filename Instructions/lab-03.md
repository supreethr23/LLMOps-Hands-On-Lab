# Lab 03: Evaluating and Deploying LLMs
### Estimated Time: 60 mins

## Task 01: Evaluate Chat Flow Performance and Robustness

1. Navigate to your **Multi-Round Q&A on Your Data** prompt flow in your Azure AI Studio.

1. Create a new output named **documents** in the Outputs node. This output will represent the documents that were retrieved in the **lookup** node and subsequently formatted in the **generate_prompt_context** node. Assign the output of the generate_prompt_context node to the documents output

1. Click **Save** before moving to the next steps.

1. In the Azure AI Studio, navigate to **Tools > Prompt flow** and click on **+ Create**.

1. Select the Evaluation Flow filter and click on **Clone** on the **QnA Groundedness Evaluation** card.

1. In the Clone flow window, accept the default folder name value and click on **Clone**.

1. An Evaludation flow will be created with the following structure.

1. Update the Connection field to point to a **gpt-4** deployment in **groundedness_score** node. After updating the connection information, click on **Save** in the evaluation flow.

1. Now, repeat the same steps to create two additional evaluation flows, one **QnA Relevance Evaluation** and another **QnA GPT Similarity Evaluation**.

   >**Note:** Note that the LLM nodes, where you will set the Azure OpenAI connection for each flow, have slightly different names: **relevance_score** and **similarity_score**, respectively.

1. In the Flows section of **Prompt Flow**, open the **Multi-Round Q&A on Your Data** flow that you created. This will be the flow we use for evaluation.

1. Start the compute session if not started already.

1. On your **Multi-Round Q&A on Your Data** flow, select **Custom Evaluation** from the **Evaluation** dropdown.

1. On the **Batch run and Evaluate** window, in the **Basic settings**, for the **Promt_variants** option, select at least two variants to avoid reaching your GPT-4 model quota limit and click on **Next**.

1. On the **Batch run settings**, click on **+ Add new data**.

1. On the **Add new data** pane, enter a name for your data, click on **Browse** to choose a file and upload the **data.csv** file from yout labvm and click on **Add**.

1. After uploading your file, configure the **chat_input** as **$(data.question)** and click on **Next**.

1. On the **Evaluation settings > Select evaluation**, select the three evaluation flows you created earlier amd click on **Next**.

1. On the **Evaluation settings > Configure evaluation**, to set up the **question**, **context**, **ground_truth** and **answer** fields for each evaluation flow.

   >**Note:** Please take a moment to ensure you've selected the correct value. It's crucial for accurate metric calculation. Notice that the default values initially presented in the wizard are not the same as those indicated in the following images. 

   - QnA GPT Similarity Evaluation
   - QnA Groundedness Evaluation
   - QnA Relevance Evaluation

1. Once you have configured all the three evalution settings, click on **Review + submit** and then **Submit**.

1. The evaluation process has started. To view all evaluations (one per variant), navigate to the **Tools > Evaluation**.

1. Upon selecting specific evaluation results, you will have the ability to view their detailed information.

1. You can also select **Switch to dashboard view** to access a dashboard that provides a tabular and visual comparison between the rounds of different variations.

   - Table comparison:
   - Chart comparison:





















