# Lab 05: Automating

## Exercise 01: Bootstrapping a New Project

In this section, you will learn how to start a new project using a project template. The bootstrapping process will create a new project repository on GitHub and populate it with content from the project template. Additionally, it will set up the development environment for your project, ensuring that you have everything you need to get started quickly and efficiently.

## Prerequisites

* [Azure CLI (az)](https://aka.ms/install-az) - to manage Azure resources.
* [Azure Developer CLI (azd)](https://aka.ms/install-azd) - to manage Azure deployments.
* [GitHub CLI (gh)](https://cli.github.com/) - to create GitHub repo.
* [Git](https://git-scm.com/downloads) - to update repository contents.

You will also need:
* [Azure Subscription](https://azure.microsoft.com/free/) - sign up for a free account.
* [GitHub Account](https://github.com/signup) - sign up for a free account.
* [Access to Azure OpenAI](https://learn.microsoft.com/legal/cognitive-services/openai/limited-access) - submit a form to request access.
* Permissions to create a Service Principal (SP) in your Azure AD Tenant.
* Permissions to assign the Owner role to the SP within the subscription.

### Task 01: Steps to Bootstrap a Project

1. **Clone the LLMOps Repo (this repository) into a temporary directory**

   Clone the repository from GitHub into a temporary directory:

   ```sh
    mkdir temp
    cd temp
    git clone https://github.com/azure/llmops
   ```

2. **Define Properties for Bootstrapping**

    Go to the `llmops` directory.

   ```sh
    cd llmops
   ```

   Create a copy of the `bootstrap.properties.template` file with this filename `bootstrap.properties`.

    ```sh
    cp bootstrap.properties.template bootstrap.properties
    ```

    Open the `bootstrap.properties` with a text editor and update it with the following information:

   - **GitHub Repo Creation** (related to the new repository to be created)
     - `github_username`: Your GitHub **username**.
     - `github_use_ssh`: Set to **true** to interact with GitHub repos using [SSH](https://docs.github.com/en/get-started/getting-started-with-git/about-remote-repositories#cloning-with-ssh-urls), **false** to use [HTTPS](https://docs.github.com/en/get-started/getting-started-with-git/about-remote-repositories#cloning-with-https-urls).
     - `github_template_repo`: The project template repository. Ex: *azure/llmops-project-template*.
     - `github_new_repo`: The bootstrapped project repo to be created. Ex *placerda/my-rag-project*.
     - `github_new_repo_visibility`: Visibility of the new repository, choose **public**, **private** or **internal**.

        > For private or internal repositories, you must use GitHub Pro, GitHub Team, or GitHub Enterprise.

   - **Dev Environment Provision Properties**
     - `azd_dev_env_provision`: Set to **true** to provision a development environment.
     
          > If you set it to **false**, you will need to manually create the environment for the project.

     - `azd_dev_env_name`: The name of the development environment. Ex: *rag-project-dev*.
     - `azd_dev_env_subscription`: Your Azure subscription ID.
     - `azd_dev_env_location`: The Azure region for your dev environment. Ex: *eastus2*.

    > The dev environment resources will be created in the selected subscription and region. This decision should consider the quota available for the resources to be created in the region, as well as the fact that some resources have specific features enabled only in certain regions. Therefore, ensure that the resources to be created by the IaC of your template project have quota and availability in the chosen subscription and region. More information about the resources to be created can be found on the template page, as shown in this project template example: [LLMOps Project Template Resources](https://github.com/Azure/llmops-project-template/blob/main/README.md#project-resources).

   Here is an example of the `bootstrap.properties` file:

   ```properties
   github_username="placerda"
   github_use_ssh="true"
   github_template_repo="azure/llmops-project-template"
   github_new_repo="placerda/my-rag-project"
   github_new_repo_visibility="public"
   azd_dev_env_provision="true"
   azd_dev_env_name="rag-project-dev"
   azd_dev_env_subscription="12345678-1234-1234-1234-123456789098"
   azd_dev_env_location="eastus2"
   ```

3. **Authenticate with Azure and GitHub**

   Log in to Azure CLI:

   ```sh
   az login
   ```

   Log in to Azure Developer CLI:

   ```sh
   azd auth login
   ```

   Log in to GitHub CLI:

   ```sh
   gh auth login
   ```

4. **Run the Bootstrap Script**

   The bootstrap script is available in two versions: Bash (`bootstrap.sh`) and PowerShell (`bootstrap.ps1`). 

   Run the appropriate script for your environment.

   **For Bash:**

   ```sh
   ./bootstrap.sh
   ```

   **For PowerShell:**

   ```powershell
   .\bootstrap.ps1
   ```

    At the end of its execution, the script will have created and initialized the new repository and provisioned the development environment resources, provided you set `azd_dev_env_provision` to true. During its execution, the script checks if the new repository exists and creates it if it does not. It then clones the template repository and mirrors it to the new repository. Additionally, it sets the default branch for the new repository.

5. **Create a Service Principal**

   Create a service principal using the following command:

   ```sh
   az ad sp create-for-rbac --name "<your-service-principal-name>" --role Owner --scopes /subscriptions/<your-subscription-id> --sdk-auth
   ```

   > Ensure that the output information created here is properly saved for future use.

6. **Set GitHub Environment Variables**

   Go to the newly created project repository and set the following GitHub environment variables and secret for three environments: `dev`, `qa`, and `prod`.

   - **Environment Variables:**
     - `AZURE_ENV_NAME`
     - `AZURE_LOCATION`
     - `AZURE_SUBSCRIPTION_ID`
   
   - **Secret:**
     - `AZURE_CREDENTIALS`

   After creating the variables and secret, your Environments page should resemble the following example:
   
   ![Environments Page](../media/bootstrapping_environments.png)
   
   Below is an example of environment variable values for a development environment:
   
   ![Environment Variables](../media/bootstrapping_env_vars.png)
   
   The `AZURE_CREDENTIALS` secret should be formatted as follows:
    
   ```json
   {
       "clientId": "your-client-id",
       "clientSecret": "your-client-secret",
       "subscriptionId": "your-subscription-id",
       "tenantId": "your-tenant-id"
   }
   ```

   > **Note:** If you are only interested in experimenting with this accelerator, you can use the same subscription, varying only `AZURE_ENV_NAME` for each enviornment.

7. **Enable GitHub Actions**

   Ensure that GitHub Actions are enabled in your repository, as in some cases, organizational policies may not have this feature enabled by default. To do this, simply click the button indicated in the figure below:

   ![Enable Actions](../media/enable_github_actions.png)

That's all! Your new project is now bootstrapped and ready to go.

## Exercise 02: # Delivering a New Feature

Once the project bootstrapping is complete, the team can begin developing new features. This section provides a detailed guide on delivering a new feature, covering every step from initial development to production deployment. To illustrate the procedure, we will develop a new feature called "Feature X," which will be included in the project's release 1.0.0. The process can be summarized in six steps, represented in the following diagram, making it easier to understand and follow along.

![Git Workflow](../media/git_workflow_branching.png)

Follow the steps below to deliver this feature from the beginning of development to deployment in production. You will need access to your project repository that has been bootstrapped, a terminal (bash or PowerShell) with Git, and the GitHub page of your repository.

### Start Cloning Your Project

Use a command like the one below to clone your bootstrapped project repository. Make sure to replace the example repository name with the actual repository you created during the bootstrapping process:

```bash
git clone git@github.com:your-username/your-repository.git
cd your-repository
```

If you prefer to clone the repository using HTTPS instead of SSH, you can use the following command:

```bash
git clone https://github.com/your-username/your-repository.git
cd your-repository
```

### 1. Creating a Feature Branch

The workflow starts by creating a feature branch named `feature/feature_x` from the `develop` branch. This is where the development team will work on the new feature X.

**Switch to the `develop` branch and pull the latest changes:**

```bash
git checkout develop
git pull
```

**Create the feature branch:**

```bash
git checkout -b feature/feature_x
```

**Make non-disruptive changes to the repository. For instance, create a file `FEATUREX.md` in the project root:**

*Using Bash:*

```bash
touch FEATUREX.md
```

*Using PowerShell:*

```powershell
New-Item -ItemType File -Name "FEATUREX.md"
```

This ensures the new feature is developed in isolation, maintaining the integrity of the project's `develop` branch and promptflow.

### 2. Pull Request (PR) to `develop`

Upon completing the feature, create a Pull Request (PR) to merge changes from the feature branch `feature/feature_x` to the `develop` branch, which is the default branch where the team integrates changes.

**Add changes, commit, and push to the feature branch:**

```bash
git add .
git commit -m "Feature X complete"
git push origin feature/feature_x
```

**Create the PR:**

```bash
gh pr create --base develop --head feature/feature_x --title "Feature X" --body "Description of the changes and the impact."
```

You can also use the GitHub website to create the pull request. Remember to select `develop` as the base branch and `feature/feature_x` as the compare branch.

The creation of the PR triggers a PR Evaluation Pipeline to ensure that the code adheres to standards, passes unit tests, and the orchestration flow is evaluated by AI to ensure it meets quality metrics.

### 3. Merge to `develop`

Approve the Pull Request, merging it into the `develop` branch. This merge triggers the Continuous Integration (CI) Pipeline, which builds the orchestration flow and conducts AI-assisted evaluations using a comprehensive test dataset based on the [Golden Dataset](https://aka.ms/copilot-golden-dataset-guide). Upon successful completion, the Continuous Deployment (CD) Pipeline is executed to deploy the flow to the **dev** environment.

**Merge the PR using GitHub:**

Go to the Pull Requests tab in your repository, select the recently created PR, and click on **Merge pull request**.

### 4. Release Branch (`release/1.0.0`)

After confirming the stability of the `develop` branch through testing in **dev**, create a release branch `release/1.0.0` from `develop`. This triggers a *Continuous Deployment (CD) pipeline* to deploy the application to the **qa** environment. Before deployment, an AI-based evaluation assesses [quality](https://learn.microsoft.com/en-us/azure/ai-studio/how-to/develop/flow-evaluate-sdk), risk and [safety](https://learn.microsoft.com/en-us/azure/ai-studio/how-to/develop/simulator-interaction-data) evaluation. The application in **qa** is then used for User Acceptance Testing (UAT) and [red-teaming](https://learn.microsoft.com/en-us/azure/ai-services/openai/concepts/red-teaming) ou LLM App.

**Create the release branch:**

```bash
git checkout develop
git pull origin develop
git checkout -b release/1.0.0
git push origin release/1.0.0
```

### 5. Pull Request to `main`

After UAT tests in the **qa** environment confirm that the application is ready for production, create a Pull Request (PR) to merge the changes into the `main` branch from the `release/1.0.0` branch.

**Create the PR:**

Below is an example utilizing the GitHub CLI:

```bash
gh pr create --base main --head release/1.0.0 --title "Release 1.0.0" --body "Merging release/1.0.0 into main after successful UAT in QA environment"
```

You can also use the GitHub website to create the pull request. Remember to select `main` as the base branch and `release/1.0.0` as the compare branch.

### 6. Merge to `main`

Once the Pull Request (PR) to the `main` branch is approved on GitHub, go to the Pull Requests tab of your project repository on GitHub, select the PR created for merging into production, and click on **Merge pull request** to manually approve the merge of `release/1.0.0` into the `main` branch. This action triggers the Continuous Deployment (CD) pipeline, which deploys the code to the **prod** environment.

