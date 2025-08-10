# WhatsApp-Driven Google Drive Assistant (n8n Workflow)

A robust n8n workflow designed to manage Google Drive files and folders directly from WhatsApp, created as a submission for the internship task.

##  Demo

*A short video demonstrating the end-to-end functionality of the workflow.*

**[https://youtu.be/FDs1PNfw3Ew?si=4Qtw0yeTinauT1vB]**

---

## 🌟 Workflow Overview

This workflow listens for commands sent via WhatsApp and performs the corresponding actions in a user's Google Drive, including listing, deleting, moving, and summarizing files.

![Complete Workflow Overview] <img width="1919" height="856" alt="Main-Editor-Workflow" src="https://github.com/user-attachments/assets/cc697fa4-dfaa-426e-83cc-ee87f3c417a2" />


---

## 🛠️ Core Technologies

* **n8n:** The core low-code platform for building the workflow logic.
* **Twilio:** Used for the WhatsApp Sandbox integration.
* **Google Drive API:** For all file and folder management operations.
* **Google OAuth2:** For secure authentication with a user's Google Account.

---

## 🚀 Setup & Installation Guide

Follow these steps to get the workflow up and running.

### Prerequisites

* An active n8n instance (cloud or self-hosted).
* A Twilio account with an active Sandbox for WhatsApp.
* A Google Cloud Platform account.

### Step 1: Import the n8n Workflow

1.  Download the `workflow.json` file from this repository.
2.  In your n8n canvas, select **Import from File** and upload the `workflow.json` file.

### Step 2: Google Cloud Platform Setup

To allow n8n to access Google Drive, you need to create API credentials.

1.  Go to the [Google Cloud Console]<img width="1897" height="875" alt="Google-cloud-console" src="https://github.com/user-attachments/assets/25f28d25-6f43-4bd5-b7bd-e76794ee68e1" />
.
2.  Create a new project or select an existing one.
3.  In the search bar, find and **Enable** the **"Google Drive API"**.
4.  Navigate to **APIs & Services > Credentials**.
5.  Click **+ CREATE CREDENTIALS** and select **OAuth client ID**.
6.  Set the **Application type** to `Web application`.
7.  Under **Authorized redirect URIs**, click **+ ADD URI** and paste the OAuth Redirect URL from your n8n Google credential configuration.
    ![Google Cloud Redirect URI Setup](images/google-redirect-uri.png)
8.  Click **CREATE**. A pop-up will appear with your **Client ID** and **Client Secret**. Copy these two values.

### Step 3: Configure n8n Credentials

This workflow requires one credential to be configured within n8n.

1.  In the n8n sidebar, go to **Credentials** and click **Add Credential**.
2.  Search for and select **`Google OAuth2 API`**.
    * **Important Note:** The generic `Google OAuth2 API` type is used to manually define the required permissions, as this was not available in the `Google Drive OAuth2 API` credential type in the n8n version used.
3.  Fill in the following details:
    * **Credential Name:** A name of your choice (e.g., `My Google Drive Creds`).
    * **Client ID:** Paste the Client ID from the Google Cloud Console.
    * **Client Secret:** Paste the Client Secret from the Google Cloud Console.
    * **Scopes:** `https://www.googleapis.com/auth/drive` (This provides full access for all operations).
    ![n8n Credential Scopes Setup]<img width="1911" height="866" alt="Credentials" src="https://github.com/user-attachments/assets/33e62f5b-66ec-455e-9ea8-3b230804d0f5" />

4.  Click **Save** and complete the Google authentication pop-up flow. You will need to bypass the "unverified app" screen as you are the developer.

### Step 4: Configure Twilio Sandbox

1.  Log in to your Twilio account and navigate to the **WhatsApp Sandbox Settings**.
2.  In the **"When a message comes in"** field, paste the **Production URL** of the Webhook node from your n8n workflow.
3.  Ensure the method is set to `HTTP POST` and save the changes.
    ![Twilio Webhook Setup] <img width="1903" height="855" alt="Twilo-setup" src="https://github.com/user-attachments/assets/f508411a-7c9b-410c-9633-b8916d22044c" />

4.  Follow the instructions on the Twilio page to connect your phone to the sandbox by sending the specified join code.

---

## ⚙️ Workflow Logic

The workflow operates based on a central **Switch Node** that parses the first word of an incoming WhatsApp message and routes the execution to the appropriate branch (`LIST`, `DELETE`, `MOVE`, `SUMMARY`). If no command is matched, a **Default** path logs the message to a Google Sheet.

---

## 📝 Command Syntax

Send the following commands to the Twilio WhatsApp number:

| Command | Description | Example |
| :--- | :--- | :--- |
| `LIST` | Lists files in the pre-configured root folder. | `LIST foldername` |
| `DELETE` | Deletes a specified file. | `DELETE filename.txt` |
| `MOVE` | Moves a file to a new destination. | `MOVE file.txt destination_folder` |
| `SUMMARY` | Summarizes a file's content. | `SUMMARY filename.txt` |

---
## Whatsapp Twilio Message
<img width="1256" height="745" alt="Screenshot 2025-08-10 222151" src="https://github.com/user-attachments/assets/c22e1531-467c-48d5-b898-4a1392059340" />


## Google Sheet Data Save 
<img width="1919" height="865" alt="Data-in-Google-sheet" src="https://github.com/user-attachments/assets/287e2dd7-5e9e-4f80-8d1a-b5810190616e" />


## ❗ Known Issues & Blockers

As of the submission deadline (`10 August 2025, 23:59 IST`), the workflow's core logic for all commands is fully built and executes successfully within n8n.

* **Primary Blocker:** There is a persistent issue where the final `Respond to Webhook` node fails to deliver the reply back to the user on WhatsApp.
* **Proof of Functionality:** Despite the reply issue, the backend logic is fully functional. The successful execution logs, showing all nodes turning green after a command is sent, have been included in the demo video as proof.
    ![Successful n8n Execution Log]<img width="1912" height="763" alt="Excution-workflow" src="https://github.com/user-attachments/assets/a07180af-ec01-494b-bc8e-edde706e16a3" />


## 📄 License

This project is licensed under the MIT License.
