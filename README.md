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



3. For the workflow setup a new action
4. Observe the list of available actions
5. From the list select **Harness** and then **Execute Harness Pipeline**
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

   <pre>
pipeline:
  identifier: rollback_pipeline
  variables:
    - name: service_name
      type: String
      value: {{ScriptedAction.Inputs.service_name}}
   </pre>



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
6. Edit the **change record url** to the one we copied earlier
7. In the Harness UI, navigate to the **Continous Delivery** module
8. From the left menu, select **Pipelines**.
9. Run the **ai_sre_pipeline**  


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


