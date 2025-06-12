# Firewall Automation Workflow for n8n

This repository contains an n8n workflow for automating firewall provisioning using Cisco APIs and Google Sheets integration. The workflow leverages Zero-Touch Provisioning (ZTP), device model lookup, status monitoring, and error handling, while keeping records up to date in Google Sheets.

## Table of Contents

- [Prerequisites](#prerequisites)
- [Installation and Setup](#installation-and-setup)
- [Importing the Workflow](#importing-the-workflow)
- [Configuring Credentials](#configuring-credentials)
  - [Cisco API Credentials](#cisco-api-credentials)
  - [Google Sheets Credentials](#google-sheets-credentials)
- [Workflow Overview](#workflow-overview)
- [Usage](#usage)
- [Contributing](#contributing)
- [License](#license)

## Prerequisites

- **n8n:** Install n8n either locally or via Docker.
- **Cisco Developer Account:** For API credentials and access to Cisco Firepower/SDC endpoints.
- **Google Account:** With Google Sheets access; OAuth2 credentials must be configured.
- Basic comfort with REST APIs and editing JSON-based workflows.

## Installation and Setup

### Running n8n Locally

1. **Using npm:**
   - Install globally:
     ```bash
     npm install -g n8n
     ```
   - Start n8n:
     ```bash
     n8n start
     ```

2. **Using Docker:**
   - Run n8n with Docker:
     ```bash
     docker run -it --rm -p 5678:5678 n8nio/n8n
     ```
   - Open your browser at [http://localhost:5678](http://localhost:5678).

## Importing the Workflow

1. Open your n8n dashboard.
2. Click the **Import** button in the workflows menu.
3. Select and upload the `Firewall_Automation_n8n.json` file.
4. Review the workflow to confirm that all nodes and credentials have imported properly.
5. **Activate** the workflow when you’re ready to run it.

> **Note:** The Google Sheets integration is preconfigured to force a copy of your spreadsheet. Ensure you have a valid spreadsheet ID and proper permissions before activating the workflow.

## Configuring Credentials

### Cisco API Credentials

The workflow relies on Cisco APIs for provisioning tasks, using two main credentials:

- **SCC Header:** For HTTP header authentication.
- **SCC Bearer:** For accessing Bearer tokens via Cisco’s OAuth2 endpoint.

**Setup Steps:**

1. Sign up at [Cisco Developer](https://developer.cisco.com/) and obtain your `client_id` and `client_secret`.
2. In n8n, navigate to **Credentials** and create a new credential using **HTTP Header Auth** for SCC Header.
3. Also create a credential for **HTTP Bearer Auth** for SCC Bearer.
4. Replace placeholders (e.g., `!!!YOUR_CLIENT_ID` and `!!!YOUR_CLIENT_SECRET`) found in the workflow nodes with your actual values.

### Google Sheets Credentials

1. Go to the [Google Cloud Console](https://console.cloud.google.com/).
2. Create (or select) a project and enable the Google Sheets API.
3. Create OAuth2 credentials.
4. In n8n, add a new credential under **Google Sheets OAuth2 API** and follow the on-screen instructions.
5. Make sure the spreadsheet the workflow will use is shared with the service account (if applicable).

## Workflow Overview

Key components of the workflow include:

- **ZTP Node:** Initiates a POST request to Cisco’s provisioning API with parameters set from the JSON data.
- **Provision Status Management:** Utilizes IF and Switch nodes to check if the provisioning is "IN_PROGRESS," "DONE," or has encountered an "ERROR" and updates Google Sheets accordingly.
- **SN to Model Lookup:** Retrieves the device model based on the provided serial number.
- **Google Sheets Inputs & Updates:**
  - **Triggers:** Monitor for new rows/updates in your Google Sheet.
  - **Updates:** Write provisioning status, device model details, and error notifications back to the sheet.
- **Error Handling:** Stops the workflow and logs errors when critical inputs (like Serial Number or Template ID) are missing.

## Usage

1. **Configure the Credentials:** Follow the [Configuring Credentials](#configuring-credentials) section above to set up Cisco and Google Sheets authentication.
2. **Import the Workflow:** Use the steps under [Importing the Workflow](#importing-the-workflow) to add it to your n8n instance.
3. **Customize as Needed:**
   - Adjust API endpoints, spreadsheet IDs, or node parameters if your environment differs.
   - Make sure to update any placeholder values in the workflow.
4. **Activate and Test:**
   - Activate the workflow.
   - Trigger a test via the Google Sheets trigger or manually using the provided nodes.
   - Monitor the workflow logs on n8n to ensure smooth execution.

## Contributing

Contributions are welcome! If you want to help improve this workflow:

1. **Fork the Repository.**
2. **Clone Your Fork Locally:**
   ```bash
   git clone https://github.com/yourusername/firewall-automation-workflow.git
   ```
3. **Create a Feature Branch:**
   ```bash
   git checkout -b feature/your-feature-name
   ```
4. ***Make Your Changes:*** Ensure your changes adhere to the documentation and coding standards.
5. ***Submit a Pull Request:*** Clearly explain your changes, the improvements, and how they benefit the workflow.

For more details, please review CONTRIBUTING.md.