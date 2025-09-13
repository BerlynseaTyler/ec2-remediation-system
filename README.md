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

![001 App Details]() 

### Step 2: Create EC2 Instance and Log Tables
#### Step 2.1: Create EC2 Instance Table
- Table: EC2 Instance (`x_snc_ec2_monitoring_ec2_instance`)
- Fields:
  - `instance_name` (String)
  - `instance_id` (String)
  - `instance_status` (String)

![002 EC2 Table]()

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
 
 ![003 Remediation Table]()

### Step 3: Connect AWS Integration Server
### Step 4: Set Up One-Click Remediation 
#### Step 4.1 Add "Trigger Remediation" Button to EC2 Table (Custom UI Action)
#### Step 4.2: Add "EC2RemediationHelper" (Script Include)
### Step 5: Create Automated Workflow
#### Trigger
#### Create Incident Record
#### AI Search Custom
#### Set Flow Variable 
#### Post A Slack Message (Instance OFF)
#### Do/Wait Until
#### Update Incident Record
#### Post A Slack Message (Instance ON)
### Step 6: Remediation Knowledge Article 
### Step 7: AI Search Integration 
### Step 8: Testing & Validation 

## Architecture Diagram
![Diagram](https://github.com/BerlynseaTyler/ec2-remediation-system/blob/main/Diagram.png)

## Optimization

## DevOps Usage 
