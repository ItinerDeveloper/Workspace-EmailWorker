### Email Worker Integration Service  

## Description  
The Email Worker Integration Service is designed to initiate workflows in the **Itiner Workspace** application based on incoming emails. This service monitors a dedicated email address on the Exchange Server for unread emails. When such emails are detected, the service performs the following actions:  

1. Sends a request to the Itiner Workspace application to initiate a new workflow.  
2. Attaches the email content and all its attachments to the initiated workflow.  
3. Injects email metadata (such as subject, sender name, and body) into predefined workflow variables, if configured.  
4. Optionally adds the full `.msg` email file to the workflow if enabled in the configuration.  

If a workflow cannot be initiated due to an error, the email is marked as **read** to prevent reprocessing and flagged to indicate that the workflow initiation failed.  

### Prerequisites  
- A **dedicated email address** is required for the service to operate.  
- The service must have the necessary permissions to access and monitor the specified mailbox.  
- If Microsoft Exchange Online is used, Basic Authentication with username and password must not be assumed to be available for the long term.  
- For Exchange Online environments, use a Modern Authentication compatible approach wherever possible. If this add-on is configured with username/password only, treat that configuration as suitable for on-premises Exchange or other environments where such authentication is still supported.  

### Key Features  
- **Periodic Email Check**: The service checks for unread emails at regular intervals.  
- **Workflow Automation**: Automatically initiates a new workflow in Itiner Workspace upon detecting an unread email.  
- **Email Attachment Handling**: Attaches all associated attachments and optionally the `.msg` email file to the workflow for seamless information transfer. 
- **Email Tagging**: Adds predefined tags to files attached to the initiated workflow.
- **Metadata Injection**: Allows email metadata (subject, sender name, and body) to be stored in workflow variables for enhanced processing.  
- **Error Handling**: If workflow initiation fails, the email is marked as read and flagged to prevent reprocessing.  

## Configuration Parameters  
> Important:
> The sample configuration in this repository uses a classic host/port/username/password model. This can still be applicable for on-premises Exchange or other compatible mail servers, but it may not be sufficient for Microsoft Exchange Online tenants that require Modern Authentication.
>
> The following parameters will be set in the Itiner Workspace configurations:  

- **ConfigName**: A name for the configuration instance (e.g., "Test").  
- **ItinerTemplateUniqueName**: The unique identifier of the workflow template that is initiated in Itiner Workspace.  
- **ItinerStarterUserName**: The workflow initiator user.  
- **ItinerManagerUserName**: The workflow manager of the initiated workflow.  
- **ItinerDefaultUserName**: The default user of the initiated workflow.  
- **Host**: The hostname or IP address of the email server (e.g., "SMAIL09").  
- **Port**: The port used for the email server connection (e.g., "143").  
- **UserName**: The username used to access the mailbox.  
- **Password**: The password for the mailbox account. For Exchange Online, username/password based legacy authentication should not be considered a future-proof option.  
- **SecurityOptions**: The security settings for the email server connection.  
- **Tags**: A list of tags to be added to files attached to the workflow (e.g., `["EmailWorker", "Test"]`).  
- **SubjectVariable**: The workflow variable that will store the email subject (e.g., `EmailWorkerSubject`).  
- **FromDisplayNameVariable**: The workflow variable that will store the sender’s display name (e.g., `EmailWorkerFromName`).  
- **FromEmailVariable**: The workflow variable that will store the sender’s email address (e.g., `EmailWorkerFromAddress`).  
- **BodyVariable**: The workflow variable that will store the email body content (e.g., `EmailWorkerBody`).  
- **AttachEmailMessage**: A flag (`true/false`) determining whether the full email file (`.msg`) should be attached to the workflow.  

---  

This service is part of the **Itiner Workspace integrations**, offering advanced automation capabilities by connecting your email inbox with workflow initiation in Itiner Workspace.  
