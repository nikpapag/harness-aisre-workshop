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


| Input      | Value     | Notes |
| ---------- | ----------------------  | ----- |
| Channel Name       | <pre>`SLACK-{{Activity.activity_number}}`</pre>||
|`                `|`     


7. That way the channel will be named according to the issue
8. Then navigate to **Triggers** from the navigation bar
9. Select **+ New Trigger**
10. Leave things as they are and click **save**





# Lab 3 - Integrations Change Control
# Lab 4 - Auto Remediation


Runbooks
Automated Bridge
Workflow


Incident Types
<img width="524" height="450" alt="image" src="https://github.com/user-attachments/assets/9dd5d1e1-6d9d-4bf8-a7f3-1dc43ab36e5d" />


Required Fields
Impacted Services
Summary



Integration

<img width="495" height="443" alt="image" src="https://github.com/user-attachments/assets/d09ef2d5-bc58-41f1-9089-972ba8d36c86" />




Rollback
<img width="571" height="520" alt="image" src="https://github.com/user-attachments/assets/782708be-0f90-444c-83f5-fc818bbe0a86" />
