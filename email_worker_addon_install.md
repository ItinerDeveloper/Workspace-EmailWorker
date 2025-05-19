# Itiner Workspace Email Worker Add-on Installation Guide

## 1. Preparation

### 1.1 System Requirements
- Windows operating system
- Internet Information Services (IIS)
- HTTPS certificate 
- Installed Itiner Workspace application
- A **working Exchange Server** configured and accessible (e.g., Exchange Web Services), with a dedicated email address monitored by the addon.

### 1.2 File System
The Email Worker Add-on **must be located in the same folder as the Itiner Workspace application**.  
Recommended structure:  
`D:\ItinerWorkspace\EmailWorker\`

---

## 2. Installation Steps

### 2.1 Copy the Email Worker Add-on Folder
1. Unzip the `ItinerWorkspaceService_EmailWorker-x.x.x.ZIP`.
2. Copy the `EmailWorker` folder to `D:\ItinerWorkspace\`.

---

## 3. Email Worker Add-on Setup

### 3.1 Required Files
Ensure the following files exist in the `EmailWorker` folder:
- `runtimes` folder
- `.dll` files
- `appsettings.json`
- `ProtocolEmailWorkerService` file
- `.deps` files
- `Web.config`

### 3.2 IIS Application Pool
1. Open IIS Manager.
2. Create a new application pool:
   - **Name**: `ItinerWorkspace_EmailWorker` (or custom)
   - **.NET CLR Version**: No Managed Code
   - **Managed Pipeline Mode**: Integrated

### 3.3 Website Configuration
Install the Email Worker Add-on on the same site as Itiner Workspace (e.g., `Default Website`).

---

### 3.4 Configuration
Edit the `appsettings.json` file with the following parameters:
> 💡 For reference, see the `email_worker_appsettings_sample.json` file located in the GitHub repository.

#### Host Configuration
```json
"Host": {
  "WebHostUrl": "https://localhost:5001",
  "WSUrl": "http://workspacehost/workspace/api",
  "WsApiKey": "test", // API key generated for integration user
  "HealthCheckBaseUrl": "https://localhost:5001", // optional
  "CustomApiKey": "test",
  "PathBase": "/workspace/emailworker",
  "DebugMode": false,
  "DisableRequestLog": false,
  "DisableMetadata": true,
  "PollIntervalInSeconds": "60"
}
```

#### Email Server Settings
```json
"EmailServers": [{
  "ConfigName": "Teszt", // Custom name of the worker
  "ItinerStarterUserName": "integration", // Determines the Workflow Starter assigned to the workflow initiated by the worker
  "ItinerManagerUserName": "integration", // Determines the Workflow Manager assigned to the workflow initiated by the worker
  "ItinerDefaultUserName": "integration", // Determines the Default User assigned to the workflow initiated by the worker
  "Tags": ["Email"], // All attachments sent to the workflow will be tagged by the worker with the tags given in this list (optional)
  "SubjectVariable": "SubjectVariable", // If given, the email subject will be extracted into the variable given here (optional)
  "FromDisplayNameVariable": "FromDisplayNameVariable", // If given, the full name of the email sender will be extracted into the variable given here (optional)
  "FromEmailVariable": "FromEmailVariable", // If given, the email address of the sender will be extracted into the variable given here (optional)
  "BodyVariable": "BodyVariable", // If given, the email body will be extracted into the variable given here (optional)
  "AttachEmailMessage": true, // If true, the incoming email itself will be attached to the workflow initiated (as a .msg file)
  "Host": "SMAIL01", // Host of exchange server
  "Port": "143", // Port of exchange server
  "UserName": "user",
  "Password": "password",
  "SecurityOptions": "None"
}],
"Storage": {
        "Password": "test" // This password has to match with the storage.password value in the Workspace appsettings.json
    },
```

#### HMAC Configuration
```json
"Hmac": {
  "Secret": "demo"
}
```

#### Reference Filter Configuration
- **Purpose**: Controls which events the integration service processes based on the `Reference` value in the event.
- **Behavior**:
  - If the `Filter` list is **not empty**, the integration service will **only process events** that contain a reference included in the `Filter` list.
  - If the `Filter` list is **empty**, the service will process **all events**, regardless of their `Reference` value.
- **Usage**: Populate the `Filter` list with user-defined keys to restrict the scope of processed events.

```json
"Reference": {
  "Filter": ["Email"]
},
```

#### Logging (Serilog) Configuration
```json
"Serilog": {
  "Using": ["Serilog.Sinks.Seq"],
  "LevelSwitches": {
    "$controlSwitch": "Information"
  },
  "MinimumLevel": {
    "ControlledBy": "$controlSwitch",
    "Override": {
      "System": "Warning",
      "Microsoft": "Error"
    }
  },
  "WriteTo": [{
    "Name": "Seq",
    "Args": {
      "serverUrl": "http://global_seq:5341",
      "apiKey": ""
    }
  }],
  "Enrich": ["FromLogContext", "WithMachineName", "WithThreadId"],
  "Properties": {
    "Application": "EmailWorker"
  }
}
```

---

### 3.5 Add IIS Web Application
1. Open IIS Manager.
2. Add a new web application with the following settings:
   - **Alias**: `EmailWorker`
   - **Application Pool**: Select the previously created pool (e.g., `ItinerWorkspace_EmailWorker`)
   - **Physical Path**: `D:\ItinerWorkspace\EmailWorker`

---

## 4. Logging
By default, logs are sent to a SEQ server. Optionally, configure file or database logging as needed.

---

## 5. Restart
After completing the setup, restart the application pool in IIS Manager to apply the changes.

---

## 6. Updating the Email Worker Add-on
1. **Stop** the application pool in IIS Manager.
2. Replace the existing `EmailWorker` folder with the updated version.
3. If required, modify the `appsettings.json` file and add any new keys provided.
4. **Restart** the application pool to activate the updated configuration.

---

This guide details the installation and update process for the Email Worker Add-on within the Itiner Workspace application. For additional support, consult your development team or system administrator.
