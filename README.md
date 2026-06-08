# AI SRE Lab Guide

## Lab Pre-Read

### Changing Harness Modules
As part of this lab, we will switch between modules several times.

**To switch modules:**
1. Click on the **nine dot** menu icon
2. Select the module relevant to the step
3. The lab begins in the code repository

<img width="362" height="193" alt="image" src="https://github.com/user-attachments/assets/2d953dd9-b370-4308-9504-a9bf13f7fb9a" />

---

# Lab 1 - Incident Types

## Overview
In this lab, you will set up an incident type for failures detected manually. Incident types can vary from questions related to production incidents on critical business services to other types of operational issues.

## Key Outcomes
- Understand the different incident types
- Create a custom incident type for important business services
- Configure incident creation forms

## Walkthrough

### Step 1: Create an Incident Type
1. In the Harness UI, navigate to the **AI SRE** module
2. From the left menu, select **Incidents**
3. From the top right of the screen, select **Incident Types**
4. Then select **+ Create Incident Type**

| Input      | Value     | Notes |
| ---------- | ----------------------  | ----- |
| Name       | <pre>`Incidents for Important Business Services`</pre>||
| Short ID |<pre>`IBS`</pre>||

5. From the top navigation bar, select **Creation Form**
6. Check the **Impacted Services** field

<img width="1364" height="848" alt="image" src="https://github.com/user-attachments/assets/885c81c2-3f0c-4e1f-8a44-12fc175aff07" />

7. Click **Save**

---

# Lab 2 - Runbook Automation

## Overview
In this lab, you will set up a runbook to automate service rollback operations. Runbooks enable automated or semi-automated responses to incidents, helping reduce mean time to resolution (MTTR).

## Key Outcomes
- Create automation runbooks to be executed in an automated or semi-automated way in response to an incident
- Configure runbook inputs and pipeline integration
- Establish automated remediation workflows

## Walkthrough

### Step 1: Create a Rollback Runbook
1. From the left menu, select **Runbooks**
2. Create a new runbook by clicking **+ New Runbook**

| Input      | Value     | Notes |
| ---------- | ----------------------  | ----- |
| Name       | <pre>`Rollback Service`</pre>||

3. For the workflow, set up a new step
4. Observe the list of available actions
5. From the list, select **Other** and then **Execute Harness Pipeline**
6. Setup accordingly
7. For **Run Pipeline YAML**, click on **Data**
8. From the Data Source, select **Runbook Input**
9. Create new Input

| Input      | Value     | Notes |
| ---------- | ----------------------  | ----- |
| Display Name       | <pre>`service_name`</pre>||
| Name       | <pre>`service_name`</pre>||
| Default Value       | <pre>`frontend`</pre>||

<img width="859" height="620" alt="image" src="https://github.com/user-attachments/assets/4b62af43-97e8-4df3-ad16-2eeec7f4af4f" />

10. Replace the **Run pipeline YAML** with the one below:

```yaml
pipeline:
  identifier: rollback_pipeline
  variables:
    - name: service_name
      type: String
      value: {{ScriptedAction.Inputs.service_name}}
```

11. Click **Save**

### Summary
You now have an incident type and an automated runbook configured for service rollback operations.

---

# Lab 3 - Integrations & Change Control

## Overview
In this lab, you will set up change record tracking for Harness deployments. This integration enables automatic correlation of incidents with deployment changes, providing valuable context for root cause analysis.

## Key Outcomes
- Create integrations with external systems
- Track deployment changes automatically
- Connect CI/CD pipelines to AI SRE incident management

## Walkthrough

### Step 1: Create a Deployment Integration
1. In the Harness UI, navigate to the **AI SRE** module
2. From the left menu, select **Integrations**
3. Select **+ New Integration**

| Input      | Value     | Notes |
| ---------- | ----------------------  | ----- |
| Name       | <pre>`deploy`</pre>||
| Type       | <pre>`Deployment`</pre>||
| Select Template       | <pre>`Harness Deployment`</pre>||

4. Copy the **Endpoint URL**
5. From the left-hand side menu, navigate to **Project Settings** and then **Variables**
6. Edit the **cd_change_record_url** to the one we copied earlier
7. In the Harness UI, navigate to the **Continuous Delivery** module
8. From the left menu, select **Pipelines**
9. Run the **aisre** pipeline

---

# Lab 4 - Put It All Together

## Overview
In this lab, you will execute an end-to-end incident response workflow. You'll create an incident, observe change correlation, and execute an automated remediation runbook to resolve the issue.

## Key Outcomes
- Create and manage incidents
- Observe change tracking in action
- Execute runbooks to remediate incidents
- Verify automated rollback operations

## Walkthrough

### Step 1: Observe Deployment Changes
1. In the Harness UI, navigate to the **AI SRE** module
2. From the left menu, select **Changes**
3. Observe the new change appearing post pipeline execution

<img width="1625" height="902" alt="image" src="https://github.com/user-attachments/assets/903758ba-4f17-4e70-b52b-360800dab3cd" />

### Step 2: Create an Incident
4. From the left menu, select **Incidents**
5. Create a new incident by clicking **+ New Incident**
6. In the popup window, type in the following text:

<pre>Users reported issues with the frontend UI, access to the personal banking is unavailable.</pre>

| Input      | Value     | Notes |
| ---------- | ----------------------  | ----- |
| Environments       | <pre>`prod`</pre>||
| Impacted Services       | <pre>`frontend`</pre>||

### Step 3: Execute the Rollback Runbook
7. Click on **Runbooks**
8. Click **+ Execute Runbook**
9. Select the **Rollback Service**
10. Set **service_name**: `frontend`

### Step 4: Verify the Rollback
11. In the Harness UI, navigate to the **Continuous Delivery** module
12. From the left-hand side menu, select **Services**
13. Select **frontend**
14. Confirm that the service rolled back

<img width="763" height="572" alt="image" src="https://github.com/user-attachments/assets/016c2385-ef25-4945-b930-87764063466e" />

---

# Lab 5 - AI Change Correlation

## Overview
In this lab, you will configure comprehensive change tracking across builds, deployments, alerts, and pull requests. Harness AI SRE will then use this data to automatically correlate incidents with likely root causes, dramatically reducing investigation time.

## Key Outcomes
- Set up build and alert integrations
- Configure PR ingestion for code change tracking
- Generate historical change data for correlation
- Observe AI-powered root cause analysis in action

## Walkthrough

### Step 1: Build Integration
1. In the Harness UI, navigate to the **AI SRE** module
2. From the left menu, select **Integrations**
3. Select **+ New Integration**

| Input      | Value     | Notes |
| ---------- | ----------------------  | ----- |
| Name       | <pre>`build`</pre>||
| Type       | <pre>`Build`</pre>||
| Select Template       | <pre>`Harness Build`</pre>||

4. Copy the **Endpoint URL**
5. From the left-hand side menu, navigate to **Project Settings** and then **Variables**
6. Edit the **build_record_url** to the one we copied earlier

### Step 2: Alert Integration
1. In the Harness UI, navigate to the **AI SRE** module
2. From the left menu, select **Integrations**
3. Select **+ New Integration**

| Input      | Value     | Notes |
| ---------- | ----------------------  | ----- |
| Name       | <pre>`alert`</pre>||
| Type       | <pre>`Alert`</pre>||
| Select Template       | <pre>`Dynatrace`</pre>||

4. Copy the **Endpoint URL**
5. From the left-hand side menu, navigate to **Project Settings** and then **Variables**
6. Edit the **alert_record_url** to the one we copied earlier

### Step 3: PR Ingestion
1. In the Harness UI, navigate to the **AI SRE** module
2. From the left menu, select **Integrations**
3. From the top right nav bar, select **PR Ingestions**
4. Select **+ New PR Integration**
5. Select **GitHub**
6. From the list of connectors, select:

| Input      | Value     | Notes |
| ---------- | ----------------------  | ----- |
| Select a connector       | <pre>`incident_simulator`</pre>||

7. Click **Create**
8. Observe the ingestion; it will take a couple of minutes

<img width="658" height="393" alt="image" src="https://github.com/user-attachments/assets/9d00abe0-b533-48e2-a9e1-b018daab6f0a" />

### Step 4: Generate Changes
1. In the Harness UI, navigate to the **Continuous Delivery** module
2. From the left menu, select **Pipelines**
3. Run the **generate_change_records** pipeline
4. Validate output

<img width="491" height="420" alt="image" src="https://github.com/user-attachments/assets/ac0acee4-02f0-480f-a42b-a92854f34c69" />

5. In the Harness UI, navigate to the **AI SRE** module
6. From the left menu, select **Changes**
7. Observe the new changes appearing post pipeline execution

### Step 5: Create a New Incident with AI Correlation
1. In the Harness UI, navigate to the **AI SRE** module
2. From the left menu, select **Incidents**
3. Click **+ New Incident**
4. In the **AI-Powered Quick Start**, place one of the options below (create a separate incident for the other option):

#### Option 1
```
Frequent session timeouts while browsing
Description
I keep getting logged out of my account unexpectedly. It usually happens when I'm navigating through different sections quickly.
```

#### Option 2
```
Product images not loading on catalog page
Description
When I navigate to the catalog, none of the product images are showing up, only placeholders are visible. This issue started today and persists across multiple devices and browsers.
```

5. Observe the incident as Harness analyzes all the changes, correlates them with PR data and build metadata, and pinpoints the most likely change causing each incident

<img width="971" height="556" alt="image" src="https://github.com/user-attachments/assets/92751bb0-7a6c-4ba6-bc7b-c1cf35ab6c49" />

---

# Lab 6 - Full Automation Use Case: Certificate Expiration

## Overview
In this lab, you will simulate a complete incident lifecycle for a certificate expiration scenario. This demonstrates end-to-end automation: from detection through alert generation, incident creation, and automatic remediation via runbooks.

## Key Outcomes
- Simulate a certificate expiration incident
- Configure alert-driven incident creation
- Set up automatic remediation triggers
- Observe the complete incident response lifecycle

## Walkthrough

### Step 1: Validate Application
1. From the left-hand side menu, navigate to **Project Settings**
2. Select **Project Variables**
3. The URL exists within the **lab_endpoint** variable

<img width="1692" height="302" alt="image" src="https://github.com/user-attachments/assets/5a215845-eadc-479d-9c81-5185bf039243" />

4. In a new browser, use the URL to view the deployed application
5. Everything looks normal?

### Step 2: Deploy an Invalid Certificate
Simulating a certificate expiry use case:

1. In the Harness UI, navigate to the **Continuous Delivery** module
2. From the left menu, select **Pipelines**

> [!WARNING]
> Before running the pipeline, we will change the input
> setting **ssl_input** to **true**

3. Run the **aisre** pipeline with the same input as the image below:

<img width="909" height="778" alt="image" src="https://github.com/user-attachments/assets/f3677234-060e-4455-93a0-83360bcc11d8" />

### Step 3: Configure Alert Integration (While Pipeline is Running)
1. In the Harness UI, navigate to the **AI SRE** module
2. From the left menu, select **Integrations**
3. Select **+ New Integration**

| Input      | Value     | Notes |
| ---------- | ----------------------  | ----- |
| Name       | <pre>`cert`</pre>||
| Type       | <pre>`Alert`</pre>||
| Select Template       | <pre>`AlertManager`</pre>||

4. Copy the **Endpoint URL**
5. From the left-hand side menu, navigate to **Project Settings** and then **Variables**
6. Edit the **cert_alert_url** to the one we copied earlier

### Step 4: Create a Certificate Incident Type
1. In the Harness UI, navigate to the **AI SRE** module
2. From the left menu, select **Incidents**
3. From the top right of the screen, select **Incident Types**
4. Then select **+ Create Incident Type**

| Input      | Value     | Notes |
| ---------- | ----------------------  | ----- |
| Name       | <pre>`Certificate Incident`</pre>||
| Short ID |<pre>`CERT`</pre>||

<img width="1277" height="754" alt="image" src="https://github.com/user-attachments/assets/ace6d9a6-9eb3-4a53-be28-bb4ae3a4c0ae" />

### Step 5: Setup a Runbook for Certificate Issues
1. From the left menu, select **Runbooks**
2. Create a new runbook by clicking **+ New Runbook**

| Input      | Value     | Notes |
| ---------- | ----------------------  | ----- |
| Name       | <pre>`Certificate Runbook`</pre>||

3. For the workflow, set up a new step
4. Observe the list of available actions
5. From the list, select **Other** and then **Execute Harness Pipeline**
6. Setup accordingly
7. For **Run Pipeline YAML**, click on **Data**
8. From the Data Source, select **Runbook Input**
9. Create new Input

| Input      | Value     | Notes |
| ---------- | ----------------------  | ----- |
| Display Name       | <pre>`service_name`</pre>||
| Name       | <pre>`service_name`</pre>||
| Default Value       | <pre>`frontend`</pre>||

10. Replace the **Run pipeline YAML** with the one below:

```yaml
pipeline:
  identifier: certificate_fix
  variables:
    - name: service_name
      type: String
      value: {{ScriptedAction.Inputs.service_name}}
```

11. On the **Triggers** tab, select **+ New Trigger**

| Input      | Value     | Notes |
| ---------- | ----------------------  | ----- |
| Template       | <pre>`Certificate Incident`</pre>||
| When       | <pre>`Activity Created`</pre>||
| service_name      | <pre>`frontend`</pre>||

12. From the left menu, select **Incidents**
13. From the top right of the screen, select **Incident Types**
14. Select the **Certificate Incident**
15. Under the **Runbooks** tab
16. **Pin** the Certificate Runbook

### Step 6: Setup the Alert Integration Rule
1. In the Harness UI, navigate to the **AI SRE** module
2. From the left menu, select **Alerts**
3. From the top right of the screen, select **Alert Rules**
4. Then select **+ New Alert Rule**

| Input      | Value     | Notes |
| ---------- | ----------------------  | ----- |
| Display Name       | <pre>`Certificate Rule`</pre>||
| Source Integration |<pre>`ALERT - cert`</pre>||

5. Under the **Create Incident** tab:

| Input      | Value     | Notes |
| ---------- | ----------------------  | ----- |
| Incident Type      | <pre>`Certificate Incident`</pre>||
| Title      | <pre>`{{Activity.title}}`</pre>||
| Severity   | <pre>SEV1:Major</pre>||
| Status   | <pre>New</pre>||

### Step 7: Simulation
1. Navigate to the deployed application (for retrieving the URL, see Step 1)
2. You should see a certificate error

<img width="631" height="646" alt="image" src="https://github.com/user-attachments/assets/a8522fd6-987b-4c51-8656-78b257e8c7cc" />

3. In the Harness UI, navigate to the **Continuous Delivery** module
4. From the left menu, select **Pipelines**
5. Select the **ssl_error_pipeline** and run it
6. Observe how Harness handles the full incident lifecycle

## Incident Response Flow

```
Detection
    ↓
Trigger Alert (Simulate Observability Tools)
    ↓
Raise Incident
    ↓
Auto-Remediate Issue
```

---

## Lab Complete! 🎉

You have successfully completed the AI SRE lab. You've learned how to:
- Configure incident types and runbooks
- Set up integrations for change tracking
- Use AI-powered root cause analysis
- Implement fully automated incident response workflows
