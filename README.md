 # EC2 Remediation System Overview 
Centralized application for Netflix that adds a ServiceNow-AWS integration via custom coded UI for DevOps engineers to perform **one-click EC2 remediation**, while automatically handling Incident management when an instance fails and restarts and real-time communication. 

DevOps teams are supported through through high-touch critical failures with an agentic AI-powered workflow that performs smart knowledge search for EC2 remediation guidance, sends an almost-instantaneous Slack notification that includes the KB articles' guidance, and informs the team as isntances are turned ON/OFF.

## System Objectives
- **EC2 Monitoring:** Track instance state (ON/OFF) in ServiceNow.
- **Incident Automation:** Create and resolve Incident records for failures.
- **Manual Remediation:** Allow DevOps engineers to trigger EC2 remediation via custom UI action.
- **AI-Driven Knowledge:** Embed relevant KB search results in Slack notifications.
- **Slack Notifications:** Alert teams in real time as remediation begins and completes.
- **Auditability:** Store logs of all remediation attempts in a dedicated table.

## Implementation Steps 
### Step 1: Create a ServiceNow Application
- Create a scoped app: EC2 Monitoring and Remediation.
- The system will automatically create a scope: `x_snc_ec2_monito_0`.
- All custom tables, flows, and actions must live inside this scope.

![001 App Details](https://github.com/BerlynseaTyler/ec2-remediation-system/blob/main/Images/001%20App%20Details.png) 

### Step 2: Create EC2 Instance and Log Tables
#### Step 2.1: Create EC2 Instance Table
- Table: EC2 Instance (`x_snc_ec2_monitoring_ec2_instance`)
- Fields:
  - `instance_name` (String)
  - `instance_id` (String)
  - `instance_status` (String)

![002 EC2 Table](https://github.com/BerlynseaTyler/ec2-remediation-system/blob/main/Images/002%20EC2%20Table.png)

#### Step 2.2: Create Remediation Log Table
- Table: Remediation Logs `x_snc_ec2_monitoring_remediation_log`
- Fields:
  - `ec2_sys_id` (Reference → EC2 Instance table)
  - `attempted_status` (String)
  - `error_message` (String, 4000 characters)
  - `http_status_code` (Integer)
  - `request_payload` (String, 4000 characters)
  - `response_payload` (String, 4000 characters)
  - `response_time` (Integer)
  - `success` (True/False)
  - `timestamp` (Date/Time)
 
 ![003 Remediation Table](https://github.com/BerlynseaTyler/ec2-remediation-system/blob/main/Images/003%20Remediation%20Table.png)

### Step 3: Connect AWS Integration Server
#### ServiceNow Connection & Credential Alias
- Create an Alias to house the HTTP Connection endpoint and Credential records.
![004 Alias](https://github.com/BerlynseaTyler/ec2-remediation-system/blob/main/Images/004%20Alias.png)

##### ServiceNow HTTP Connection
- Connect the HTTP Connection (URL Builder)
![005 HTTP](https://github.com/BerlynseaTyler/ec2-remediation-system/blob/main/Images/005%20HTTP.png)

##### ServiceNow Credential Records (Type: Basic Auth)
- Connect the Credentials to the HTTP URL (Username/Password)
![006 Credentials](https://github.com/BerlynseaTyler/ec2-remediation-system/blob/main/Images/006%20Credentials.png)

### Step 4: Set Up One-Click Remediation 
#### Step 4.1 Add "Trigger Remediation" Button to EC2 Table (Custom UI Action)
- Add a **“Trigger Remediation”** button to the EC2 Instance form.
- Client Script retrieves the record's `sys_id`
- Calls GlideAjax → `EC2RemediationHelper`.
![007 UI Action](https://github.com/BerlynseaTyler/ec2-remediation-system/blob/main/Images/007%20UI%20Action.png)

#### Step 4.2: Add "EC2RemediationHelper" (Script Include)
- Name: `EC2RemediationHelper`.
- Function: Onclick `trigger_EC2_Remediation()`.
- Calls AWS API to stop/start EC2 instance and logs results.
![008 Script Include](https://github.com/BerlynseaTyler/ec2-remediation-system/blob/main/Images/008%20Script%20Include.png)

### Step 5: Create Automated Workflow
![009 Flow](https://github.com/BerlynseaTyler/ec2-remediation-system/blob/main/Images/009%20Flow.png)

#### Trigger
- Every time an EC2 `instance_status` is OFF
![010](https://github.com/BerlynseaTyler/ec2-remediation-system/blob/main/Images/010%20Trigger.png)

#### Create Incident Record
- `instance_name` is dynamically included in the Incident's `short_description`
- Incident is auto-assigned to `Networking Operations`
- Priority is set to `1-Critical`
![011](https://github.com/BerlynseaTyler/ec2-remediation-system/blob/main/Images/011%20Create%20Incident.png)

#### AI Search Custom
- `Search Terms` = all related keywords to EC2 remediation.
- `Enabled Detail Logging` is required.
- `Search App Name` used the pre-defined index `Knowledge`
![012](https://github.com/BerlynseaTyler/ec2-remediation-system/blob/main/Images/012%20AI%20Search.png)
  
#### Set Flow Variable 
- Flow Variables are used to build record links that will be used in the Post A Slack Message action.
- `Instance URI` is set to script field using the following script: `return gs.getProperty('glide.servlet.uri')`
- `EC2 Link` is set to: `Instance URI``EC2 Instance Table`.do?sys_id=`EC2 Instance Record Sys ID`
- `Incident Record` is set to: `Instance URI``Incident Table`.do?sys_id=`Incident Record Sys ID`

![013](https://github.com/BerlynseaTyler/ec2-remediation-system/blob/main/Images/013%20Set%20Flow%20Variables.png)

#### Post A Slack Message (Instance OFF)
#### Do/Wait Until
#### Update Incident Record
#### Post A Slack Message (Instance ON)
### Step 6: Remediation Knowledge Article 
### Step 7: AI Search Integration 
### Step 8: Testing & Validation 

## Architecture Diagram
![Diagram](https://github.com/BerlynseaTyler/ec2-remediation-system/blob/main/Diagram.png)

![]()
## Optimization

## DevOps Usage 
