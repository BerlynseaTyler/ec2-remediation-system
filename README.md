 # EC2 Remediation System Overview 
Centralized system for Netflix that adds a ServiceNow UI button custom coded for DevOps engineers to perform **one-click EC2 remediation**, while automatically creating and resolving Incidents when an EC2 instance fails and restarts. 

DevOps teams are supported through through high-touch critical failures with an agentic AI-powered workflow that performs smart knowledge search for EC2 remediation guidance, sends an almost-instantaneous Slack notification that includes the KB articles' guidance, and informs the team as isntances are turned ON/OFF.

## Implementation Steps 
### Step 1: Create a ServiceNow Application
### Step 2: Create EC2 Instance and Log Tables
#### Step 2.1: Create EC2 Instance Table
#### Step 2.2: Create Remediation Log Table
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
