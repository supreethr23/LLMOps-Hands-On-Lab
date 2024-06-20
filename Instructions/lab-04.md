## Lab 04: Monitoring and Responsible AI 
### Estimated Time: 60 mins

## Task 01: Monitoring your LLMs flow

1. Once the **Multi-Round Q&A on Your Data** flow deployment succeeds to an online endpoint, navigate to the **Components > Deployments** section and select your recently created deployment.

1. On the **Details** tab, click on **Enable** within the *Enable generation quality monitoring* box.

1. On the **Enable monitoring** window, select all the metrics to monitor, configure column mapping, select your Azure OpenAI Connection and Deployment and click on **Create**.

   >**Note:** Monitoring sets the default sampling rate at 10%. This means that if 100 requests are sent to your deployment, 10 get sampled and used to compute the generation quality metrics. You can adjust the sampling rate in the settings.

1. On the **Operational** tab, view the operational metrics for the deployment in near real-time. The supported metrics are:

   - Request count
   - Latency
   - Error rate

1. The results in the Monitoring (preview) tab of your deployment provide insights to help you proactively improve the performance of your prompt flow application.

## Task 02: Add Content Safety to your Solution

1. 






















