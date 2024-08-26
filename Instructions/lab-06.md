### Lab 06: LLM Performance Testing

---

## Overview

In this exercise, you will set up and execute a load test on an Azure OpenAI model deployment using the Azure OpenAI Benchmarking Tool. You will learn to configure the test environment, run tests, and analyze results to understand the model's performance under various scenarios.

### Lab Objectives

By the end of this lab, you will be able to:

- Bootstrap a new project for LLM performance testing.
- Set up the Azure OpenAI Benchmarking Tool.
- Execute load tests on an Azure OpenAI deployment.
- Analyze and interpret the results of the load tests.

---

## Task 01: Bootstrapping a New Project

1. **Open Git Bash**: From the VM desktop, click on **Search**, type **Git Bash**, and select it from the search results. 

   ![Git](media/Git.png)

2. **Clone the Workshop Repository**: Run the following command in Git Bash to clone the workshop repository:

   ```bash
   git clone https://github.com/microsoft/llmops-workshop.git
   ```

   ![clone1](media/gitclone1.png)

3. **Navigate to the Performance Lab Directory**: Change to the directory where you cloned the repository:

   ```bash
   cd llmops-workshop/labs/performance
   ```

4. **Install Required Libraries**: Install the necessary Python libraries by running:

   ```bash
   pip install -r requirements.txt
   ```

5. **Clone the Benchmarking Tool Repository**: Clone the Azure OpenAI Benchmarking Tool repository with:

   ```bash
   git clone https://github.com/Azure/azure-openai-benchmark
   ```

6. **Install Libraries for the Benchmarking Tool**: Navigate to the `azure-openai-benchmark` directory and install the required libraries:

   ```bash
   pip install -r azure-openai-benchmark/requirements.txt
   ```

   At this point, you have cloned the necessary repositories and installed the required libraries.

7. **Rename and Update Benchmark Parameters**: Rename the `benchmark.parameters.template` file to `benchmark.parameters`:

   ```bash
   mv benchmark.parameters.template benchmark.parameters
   ```

8. **Retrieve Azure OpenAI Details**: Navigate to [ai.azure.com](https://ai.azure.com) and sign in. Open **Projects**, then **Deployments**. Find your `gpt-35-turbo-16k` deployment and copy the following details:
   - **Azure OpenAI API key**
   - **Azure OpenAI resource name**
   - **Azure OpenAI deployment name**

   Store these details in a notepad.

9. **Open the Code Editor**: Open Visual Studio Code by running:

   ```bash
   code .
   ```

10. **Update Benchmark Parameters**: In the `benchmark.parameters` file, replace the first four lines with your values:

    ```bash
    export OPENAI_API_KEY=[Your Azure OpenAI API key]
    AOAI_ENDPOINT=https://[Your Azure OpenAI resource name].openai.azure.com/
    AOAI_DEPLOYMENT=[Your Azure OpenAI deployment name]
    TEST_NAME=paygo-gpt35-eastus-4RPM
    ```

    ![connectionstring](media/coconntstrings2.png)

---

## Task 02: Running the Performance Test

1. **Run the Benchmark Test**: Start the test by executing:

   ```bash
   bash ./runtest.sh
   ```

   > **Note:** The test may take 1-2 minutes to complete.

   ![runtest](media/test1.2.png)

2. **Adjust Deployment Quota**: In Azure AI Studio, go to the `gpt-35-turbo-16k` deployment, click **Edit**, and increase the quota to 200K Tokens per Minute Rate Limit.

   ![deploymentupdate](media/connection2.png)

3. **Rerun the Test with Updated Parameters**: Update the `benchmark.parameters` file:
   - Change the `TEST_NAME` to `paygo-gpt35-eastus-50RPM`.

   Execute the test again:

   ```bash
   bash ./runtest.sh
   ```

   You have now updated the benchmark parameters and executed the performance tests.

---

## Task 03: Analyzing the Results

1. **Run the Analysis Notebook**: Open the `benchmark_analysis.ipynb` file in Visual Studio Code. Execute each code snippet step by step and observe the output to analyze the performance results.

   ![deploymentupdate](media/output1.png)

---
