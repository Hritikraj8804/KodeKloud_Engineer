# Instruction

The Nautilus DevOps team needs to deploy a Python-based Azure Web App.

### Requirements

| Setting              | Value                 |
| -------------------- | --------------------- |
| Web App Name         | `devops-webapp`       |
| Region               | `South Central US`    |
| Publish              | `Code`                |
| Runtime Stack        | `Python`              |
| Operating System     | `Linux`               |
| App Service Plan     | `devops-learn-python` |
| Pricing Tier         | `Basic B1`            |
| Application Insights | `Disabled`            |
| Tag 1                | `Name=WebAppLearning` |
| Tag 2                | `Environment=Dev`     |
| Final State          | `Running`             |

> **Note:** Use the Azure Portal (UI) to complete this task.



# Solution

## Step 1: Login to Azure Portal

Open:

```text
https://portal.azure.com
```

Login using the provided Azure credentials.



# Step 2: Open App Services

Search:

```text
App Services
```

Click:

```text
+ Create
```

Then select:

```text
Web App
```



# Step 3: Configure Basics

### Project Details

| Setting        | Value                      |
| -------------- | -------------------------- |
| Resource Group | Default lab resource group |



### Instance Details

| Setting          | Value                            |
| ---------------- | -------------------------------- |
| Web App Name     | `devops-webapp`                  |
| Publish          | `Code`                           |
| Runtime Stack    | `Python` (any available version) |
| Operating System | `Linux`                          |
| Region           | `South Central US`               |



# Step 4: Create App Service Plan

Under:

```text
App Service Plan
```

Click:

```text
Create New
```

Configure:

| Setting   | Value                 |
| --------- | --------------------- |
| Plan Name | `devops-learn-python` |
| SKU       | `Basic B1`            |

Save the plan.

Verify the pricing tier displays:

```text
B1 Basic
```



# Step 5: Disable Application Insights

Navigate to:

```text
Monitoring
```

Locate:

```text
Application Insights
```

Select:

```text
No
```

or

```text
Disable
```

(depending on the Azure Portal version).



# Step 6: Add Tags

Navigate to:

```text
Tags
```

Add the following tags:

| Name          | Value            |
| ------------- | ---------------- |
| `Name`        | `WebAppLearning` |
| `Environment` | `Dev`            |



# Step 7: Review and Create

Click:

```text
Review + Create
```

Wait for validation to succeed.

<img width="637" height="807" alt="review" src="https://github.com/user-attachments/assets/1195fe4f-1b78-4402-a8dc-0a9d34e32b4c" />

Click:

```text
Create
```

Deployment will take a few minutes.



# Step 8: Verify the Web App

After deployment:

Open:

```text
App Services
```

Select:

```text
devops-webapp
```

Verify:

| Property         | Expected Value        |
| ---------------- | --------------------- |
| Name             | `devops-webapp`       |
| Runtime          | Python                |
| OS               | Linux                 |
| Region           | South Central US      |
| App Service Plan | `devops-learn-python` |
| Pricing Tier     | B1                    |
| State            | Running               |



# Step 9: Verify Tags

Inside the Web App:

```text
Tags
```

Verify:

| Tag         | Value          |
| ----------- | -------------- |
| Name        | WebAppLearning |
| Environment | Dev            |

<img width="1822" height="827" alt="running" src="https://github.com/user-attachments/assets/977a7d3c-7df0-4d1b-854e-af4923ae7c13" />
