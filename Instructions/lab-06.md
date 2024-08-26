# Lab 06: LLM Performance Testing

### Estimated Duration: 60 minutes

## Overview

In this lab, you will set up and execute a load test on an Azure OpenAI model deployment using the Azure OpenAI Benchmarking Tool. You will learn to configure the test environment, run tests, and analyze results to understand the model's performance under various scenarios.

## Lab Objectives

By the end of this lab, you will be able to:

- Set up the Azure OpenAI Benchmarking Tool.
- Execute load tests on an Azure OpenAI deployment.
- Analyze and interpret the results of the load tests.

## Task 01: Azure OpenAI Benchmarking Tool

1. To open Git Bash, click on Search from the VM desktop, type **Git Bash**, and select it from the results. 

   ![Git](media/26-08-2024(1).png)

2. In Git Bash, run the following command to clone the workshop repository:

   ```bash
   git clone https://github.com/microsoft/llmops-workshop.git
   ```

   ![clone1](media/26-08-2024(3).png)

3. Navigate to the Performance Lab directory where you cloned the repository:

   ```bash
   cd llmops-workshop/labs/performance
   ```

   ![clone1](media/26-08-2024(4).png)

4. Run the following command to install the necessary Python libraries:

   ```bash
   pip install -r requirements.txt
   ```

   ![clone1](media/26-08-2024(5).png)

5. Run the following command to clone the Azure OpenAI Benchmarking Tool repository:

   ```bash
   git clone https://github.com/Azure/azure-openai-benchmark
   ```

   ![clone1](media/26-08-2024(2).png)

1. Run the following command to install the necessary Python libraries for azure-openai-benchmark:

   ```bash
   pip install -r azure-openai-benchmark/requirements.txt
   ```

7. Run the following command to rename the file name `benchmark.parameters.template` to `benchmark.parameters`

   ```bash
   mv benchmark.parameters.template benchmark.parameters
   ```

   ![clone1](media/26-08-2024(6).png)

1. Navigate to [Azure portal](portal.azure.com). In the search box, type **Resource groups** and select it from the results.

   ![clone1](media/26-08-2024(7).png)

1. Open the **llm-ops-<inject key="Deployment-ID" enableCopy="false"/>** resource group and click on **odl_user_<inject key="Deployment-ID" enableCopy="false"/>_ai** Azure AI hub.

   ![clone1](media/26-08-2024(9).png)

1. Click on **Launch Azure AI Studio**.

   ![clone1](media/26-08-2024(8).png)

1. Once the studio is opened, go to **Deployments (1)** under Shared resources and click on **gpt-35-turbo-16k (2)** model deployment.

   ![clone1](media/26-08-2024(10).png)

1. Copy the following details and store them in a notepad.

   - **Azure OpenAI deployment name (1)**
   - **Azure OpenAI resource name (2)**
   - **Azure OpenAI API key (3)**

      ![clone1](media/26-08-2024(11).png)

9. Run the following command to open the repo in the code editor: 

   ```bash
   code .
   ```

10. In the `benchmark.parameters` file, replace the first four lines with your values:

    ```bash
    export OPENAI_API_KEY=[Your Azure OpenAI API key]
    AOAI_ENDPOINT=https://[Your Azure OpenAI resource name].openai.azure.com/
    AOAI_DEPLOYMENT=[Your Azure OpenAI deployment name]
    TEST_NAME=paygo-gpt35-eastus-4RPM
    ```

    ![clone1](media/26-08-2024(12).png)

## Task 02: Running the Performance Test

1. Run the following command to start the test:

   ```bash
   bash ./runtest.sh
   ```

   > **Note:** The test may take 2-3 minutes to complete.

   ![clone1](media/26-08-2024(13).png)

2. Go to the `gpt-35-turbo-16k` deployment, click **Edit**, increase the quota to **200K Tokens per Minute Rate Limit**, and then select **Save and close**.

   ![clone1](media/26-08-2024(14).png)

1. Navigate to **Visual Studio Code** and change TEST_NAME to **paygo-gpt35-eastus-50RPM** in the benchmark.parameters file.

   ![clone1](media/26-08-2024(18).png)

1. Run the test again, and you will now see heavy load traffic going to the model.

   ```bash
   bash ./runtest.sh
   ```

   ![clone1](media/26-08-2024(13).png)

## Task 03: Analyzing the Results

1. Open the `benchmark_analysis.ipynb` file in Visual Studio Code. Execute each code snippet step by step and observe the output to analyze the performance results.

   ![clone1](media/26-08-2024(15).png)

   ![clone1](media/26-08-2024(16).png)

   ![clone1](media/26-08-2024(17).png)

## Summary

In this lab, you have performed the following tasks:

- Set up the Azure OpenAI Benchmarking Tool.
- Execute load tests on an Azure OpenAI deployment.
- Analyze and interpret the results of the load tests.

### You have successfully completed the lab.
