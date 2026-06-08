## Lab pre-read


### Changing harness modules
- As part of this lab, we will switch between modules several times

In order to switch modules:
1. Click on the **nine dot** menu icon
2. Select the module relevant to the step
3. The lab begins in the code repository

<img width="362" height="193" alt="image" src="https://github.com/user-attachments/assets/2d953dd9-b370-4308-9504-a9bf13f7fb9a" />


# Lab 1 - Incident Types

## Key Outcomes
- Understand the different incident types
 

## Overview
In this lab, the user sets up an incident type for the type of failure detected manullay by the user. Incident types can vary from question related to production incidents on critical business services

---

## Walkthrough

### Step 1: Create an incident type
1. In the Harness UI, navigate to the **AI SRE** module
2. From the left menu, select **Incidents**.  
3. From the top right of the screen select **Incident Types**
4. Then select **+ Create Incident Type**

| Input      | Value     | Notes |
| ---------- | ----------------------  | ----- |
| Name       | <pre>`Incidents for Important Business Services`</pre>||
| Short ID |<pre>`IBS`</pre>||
|`                `|`                            `|`                `|


5. From the top navigation bar select **Creation Form**
6. Check the Impacted Services Field


<img width="1364" height="848" alt="image" src="https://github.com/user-attachments/assets/885c81c2-3f0c-4e1f-8a44-12fc175aff07" />

7. Click **Save**

# Lab 2 - Runbook Automation

## Key Outcomes
- Create automation runbooks to be executed in an automated or semi-auto way in responce to an incident
 

## Overview
In this lab, the user sets up a runbook to create a slack channel whenver a new incident happens

---

1. From the left menu, select **Runbooks**.
2. Create a new runbook by clicking **+ New Runbook**

| Input      | Value     | Notes |
| ---------- | ----------------------  | ----- |
| Name       | <pre>`Rollback Service`</pre>||
|`                `|`     



3. For the workflow setup a new step
4. Observe the list of available actions
5. From the list select **Other** and then **Execute Harness Pipeline**
7. Setup accordingly
8. Run Pipeline YAML click on **Data**
9. From the Data Source select **Runbook Input**
10. Create new Input

| Input      | Value     | Notes |
| ---------- | ----------------------  | ----- |
| Display Name       | <pre>`service_name`</pre>||
| Name       | <pre>`service_name`</pre>||
| Default Value       | <pre>`frontend`</pre>||
|`                `|`     


<img width="859" height="620" alt="image" src="https://github.com/user-attachments/assets/4b62af43-97e8-4df3-ad16-2eeec7f4af4f" />



11. And then replace the **Run pipeline YAML** with the one below

```
pipeline:
  identifier: rollback_pipeline
  variables:
    - name: service_name
      type: String
      value: {{ScriptedAction.Inputs.service_name}}
```

7. Then click on **Save**

### Summary
Now we have an incident type and an automated runbook


# Lab 3 - Integrations Change Control

## Key Outcomes
- Create integrations with external systems
 

## Overview
In this lab, the user sets up a change record for harness deployments


### Step 1: Create an incident type
1. In the Harness UI, navigate to the **AI SRE** module
2. From the left menu, select **Integrations**.  
3. Select **+ New Integration**

| Input      | Value     | Notes |
| ---------- | ----------------------  | ----- |
| Name       | <pre>`deploy`</pre>||
| Type       | <pre>`Deployment`</pre>||
| Select Template       | <pre>`Harness Deployment`</pre>||
|`                `|`     

4. Copy the Endpoint URL
5. From the left handside menu navigate to **Project Settings** and then **variables**
6. Edit the **cd_change_record_url** to the one we copied earlier
7. In the Harness UI, navigate to the **Continous Delivery** module
8. From the left menu, select **Pipelines**.
9. Run the **aisre** pipeline  


# Lab 4 - Put it all together

1.  In the Harness UI, navigate to the **AI SRE** module
2.   From the left menu, select **Changes**.
3.   Observe the new change appearing post pipeline execution

 <img width="1625" height="902" alt="image" src="https://github.com/user-attachments/assets/903758ba-4f17-4e70-b52b-360800dab3cd" />

4. From the left menu, select **Incidents**.
5. Create new incident by clicking **+ New Incident**
6. In the popup window type in the following the text

<pre>Users reported issues with the frontend UI, access to the personal banking is unavailable. </pre>

| Input      | Value     | Notes |
| ---------- | ----------------------  | ----- |
| Environments       | <pre>`prod`</pre>||
| Impacted Services       | <pre>`frontend`</pre>||
|`                `|`     

7. Click on runbooks
8. **+ Execute Runbook**
9. Select the Rollback Service
10. service_name: frontend
11.  In the Harness UI, navigate to the **Continuous Delivery** module
12.  From the left handside menu select **Services**
13.  Select **frontend**
14.  Confirm that the service rolled back

<img width="763" height="572" alt="image" src="https://github.com/user-attachments/assets/016c2385-ef25-4945-b930-87764063466e" />

# Lab 5 - AI Change Corelation

## Build Integration
1. In the Harness UI, navigate to the **AI SRE** module
2. From the left menu, select **Integrations**.
3. Select **+ New Integration**

| Input      | Value     | Notes |
| ---------- | ----------------------  | ----- |
| Name       | <pre>`build`</pre>||
| Type       | <pre>`Build`</pre>||
| Select Template       | <pre>`Harness Build`</pre>||
|`                `|`     

4. Copy the Endpoint URL
5. From the left handside menu navigate to **Project Settings** and then **variables**
6. Edit the **build_record_url** to the one we copied earlier


## Alert Integration
1. In the Harness UI, navigate to the **AI SRE** module
2. From the left menu, select **Integrations**.
3. Select **+ New Integration**

| Input      | Value     | Notes |
| ---------- | ----------------------  | ----- |
| Name       | <pre>`alert`</pre>||
| Type       | <pre>`Alert`</pre>||
| Select Template       | <pre>`Dynatrace`</pre>||
|`                `|`     

4. Copy the Endpoint URL
5. From the left handside menu navigate to **Project Settings** and then **variables**
6. Edit the **alert_record_url** to the one we copied earlier


## PR Ingestion
1. In the Harness UI, navigate to the **AI SRE** module
2. From the left menu, select **Integrations**.
3. From the top right nav bar select **PR ingestions**
4. Select **+ New PR Integration**
5. Select **github**
6. From the list of connectors select 

| Input      | Value     | Notes |
| ---------- | ----------------------  | ----- |
| Select a connector       | <pre>`incident_simulator`</pre>||
|`                `|`     

4. Click on Create
5. Observe the ingestion, it will take a couple of minutes 

<img width="658" height="393" alt="image" src="https://github.com/user-attachments/assets/9d00abe0-b533-48e2-a9e1-b018daab6f0a" />



## Generate Changes
1. In the Harness UI, navigate to the **Continous Delivery** module
2. From the left menu, select **Pipelines**.
3. Run the **generate_change_records** pipeline
4. Validate Output

<img width="491" height="420" alt="image" src="https://github.com/user-attachments/assets/ac0acee4-02f0-480f-a42b-a92854f34c69" />

1.  In the Harness UI, navigate to the **AI SRE** module
2.  From the left menu, select **Changes**.
3.  Observe the new change appearing post pipeline execution

## Create a new Incident
1.  In the Harness UI, navigate to the **AI SRE** module
2.  From the left menu, select **Incidents**
3.  Click **+New Incident**
4.  In the AI-Powered Quick Start place one of the options below, create a separate incident for the other option


### Option 1
```
Frequent session timeouts while browsing
Description
I keep getting logged out of my account unexpectedly. It usually happens when I'm navigating through different sections quickly.
```

### Option 2
```
Product images not loading on catalog page
Description
When I navigate to the catalog, none of the product images are showing up, only placeholders are visible. This issue started today and persists across multiple devices and browsers.
```

5. Observe the Incident as Harness analyses all the changes, correlated them with the PR data and build metadata to pinpoint the most likely change causing each incident

<img width="971" height="556" alt="image" src="https://github.com/user-attachments/assets/92751bb0-7a6c-4ba6-bc7b-c1cf35ab6c49" />


# Lab 6 - Full automation use case, certificate expiration

## Validate application

1. From the left hand side menu navigate to project settings
2. Select **project variables**
3. The url exists within the **lab_endpoint** variable 
<img width="1692" height="302" alt="image" src="https://github.com/user-attachments/assets/5a215845-eadc-479d-9c81-5185bf039243" />
4. In a new browser use the url to view the deployed application
5. Everything looks normal?


## Lets deploy an invalid certificate
simulating a certificate expiry use case

1.  In the Harness UI, navigate to the **Continuous Delivery** module
2.  From the left menu, select **Pipelines**.
> [!WARNING]
> Before running the pipeline we will change the input
> setting **ssl_input** to true
4.  Run the **aisre** pipeline with the same input as the image below
<img width="909" height="778" alt="image" src="https://github.com/user-attachments/assets/f3677234-060e-4455-93a0-83360bcc11d8" />

### While the pipeline is running lets do some configuration

1. In the Harness UI, navigate to the **AI SRE** module
2. From the left menu, select **Integrations**.
3. Select **+ New Integration**

| Input      | Value     | Notes |
| ---------- | ----------------------  | ----- |
| Name       | <pre>`cert`</pre>||
| Type       | <pre>`Alert`</pre>||
| Select Template       | <pre>`AlertManager`</pre>||
|`                `|`     

4. Copy the Endpoint URL
5. From the left handside menu navigate to **Project Settings** and then **variables**
6. Edit the **cert_alert_url** to the one we copied earlier


### Step 1: Create an certificate incident type
1. In the Harness UI, navigate to the **AI SRE** module
2. From the left menu, select **Incidents**.  
3. From the top right of the screen select **Incident Types**
4. Then select **+ Create Incident Type**

| Input      | Value     | Notes |
| ---------- | ----------------------  | ----- |
| Name       | <pre>`Certificate Incident`</pre>||
| Short ID |<pre>`CERT`</pre>||
|`                `|`                            `|`                `|
<img width="1277" height="754" alt="image" src="https://github.com/user-attachments/assets/ace6d9a6-9eb3-4a53-be28-bb4ae3a4c0ae" />


### Step 2: Setup a runbook in response to certificate issues

1. From the left menu, select **Runbooks**.
2. Create a new runbook by clicking **+ New Runbook**

| Input      | Value     | Notes |
| ---------- | ----------------------  | ----- |
| Name       | <pre>`Certificate Runbook`</pre>||
|`                `|`     

3. For the workflow setup a new step
4. Observe the list of available actions
5. From the list select **Other** and then **Execute Harness Pipeline**
7. Setup accordingly
8. Run Pipeline YAML click on **Data**
9. From the Data Source select **Runbook Input**
10. Create new Input

| Input      | Value     | Notes |
| ---------- | ----------------------  | ----- |
| Display Name       | <pre>`service_name`</pre>||
| Name       | <pre>`service_name`</pre>||
| Default Value       | <pre>`frontend`</pre>||
|`                `|`     

11. And then replace the **Run pipeline YAML** with the one below

```
pipeline:
  identifier: certificate_fix
  variables:
    - name: service_name
      type: String
      value: {{ScriptedAction.Inputs.service_name}}
```
12. On the **triggers** tab select **+ New Trigger**

| Input      | Value     | Notes |
| ---------- | ----------------------  | ----- |
| Template       | <pre>`Certificate Incident`</pre>||
| When       | <pre>`Activity Created`</pre>||
| service_name      | <pre>`frontend`</pre>||
|`                `|`     

13. From the left menu, select **Incidents**.
14. From the top right of the screen select **Incident Types**
15. Select the **Certificate Incident**
16. Under **Runbooks** tab
17. **Pin** the Certificate Runbook



### Step 4: Setup the Alert integration
1. In the Harness UI, navigate to the **AI SRE** module
2. From the left menu, select **Alerts**.  
3. From the top right of the screen select **Alert Rules**
4. Then select **+ New Alert Rule**

| Input      | Value     | Notes |
| ---------- | ----------------------  | ----- |
| Display Name       | <pre>`Certificate Rule`</pre>||
| Source Integration |<pre>`ALERT - cert`</pre>||
|`                `|`                            `|`                `|

5. Under the Create Incident Tab

| Input      | Value     | Notes |
| ---------- | ----------------------  | ----- |
| Incident Type      | <pre>`Certificate Incident`</pre>||
| Title      | <pre>`{{Activity.title}}`</pre>||
| Severity   | <pre>SEV1:Major</pre>||
| Severity   | <pre>New</pre>||
|`                `|`                            `|`                

<img width="794" height="424" alt="image" src="https://github.com/user-attachments/assets/4b365ded-4873-4d20-a284-8ebc40a91ad7" />


