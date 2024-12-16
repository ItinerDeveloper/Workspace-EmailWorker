### Email Worker Integration Service

## Description
The Email Workflow Trigger Service is designed to initiate workflows in the **Itiner Workspace** application based on incoming emails. This service monitors a dedicated email address on the Exchange Server for unread emails. When such emails are detected, the service performs the following actions:

1. Sends a request to the Itiner Workspace application to initiate a new workflow.
2. Attaches the email content and all its attachments to the initiated workflow.


### Prerequisites
- A **dedicated email address** on the Exchange Server is required for the service to operate.
- The service must have the necessary credentials and permissions to access and monitor the specified email address.

### Key Features
- **Periodic Email Check**: The service checks for unread emails at regular intervals.
- **Workflow Automation**: Automatically initiates a new workflow in Itiner Workspace upon detecting an unread email.
- **Email Attachment Handling**: Attaches the email body and all associated attachments to the workflow for seamless information transfer.

## Configuration Parameters
The following parameters will be set in the Itiner Workspace configurations:
- **ConfigName**: A name for the configuration instance (e.g., "Teszt").
- **ItinerTemplateUniqueName**: The unique identifier of the workflow template that is initiated in Itiner Workspace.
- **ItinerStarterUserName**: The workflow initiator user.
- **ItinerManagerUserName**: The workflow manager of the initiated workflow.
- **ItinerDefaultUserName**: The default user of the initiated workflow.
- **Host**: The hostname or IP address of the email server (e.g., "SMAIL03").
- **Port**: The port used for the email server connection (e.g., "143").
- **UserName**: The username used to log in to the email account.
- **Password**: The password for the email account (ensure this is securely managed and not left empty in production environments).
- **SecurityOptions**: The security settings for the email server connection (e.g., "None" for unencrypted connections, or options like "SSL/TLS" if supported).

---

This service is part of the **Itiner Workspace integrations**, offering advanced automation capabilities by connecting your email inbox with workflow initiation in Itiner Workspace.
