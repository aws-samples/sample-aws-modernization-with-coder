---
title: "Access Coder"
chapter: false
weight: 04
--- 

## Locate Coder URL and Admin Credentials

1. Navigate to the **Event Outputs** section in the workshop portal
2. Look for the following outputs:
   - **CoderURL**: The CloudFront URL to access your Coder instance
   - **CoderAdminEmail**: The pre-created administrator email
   - **CoderAdminPassword**: The administrator password

![Event Outputs Location](/static/images/coder-outputs.png)

::alert[**Screenshot Update Required**: This screenshot needs to be updated to show the new outputs including CoderAdminEmail and CoderAdminPassword in addition to CoderURL.]{type="warning"}

## Login to Coder

The CloudFormation template has automatically created an administrator account for you. Use the credentials from the Event Outputs to log in:

1. Click on the **CoderURL** link to open your Coder instance
2. On the login page, enter:
   - **Email**: Use the value from **CoderAdminEmail** output (default: `admin@example.com`)
   - **Password**: Use the value from **CoderAdminPassword** output (default: `admin@coder`)

::alert[**Screenshot Update Required**: Replace the signup/login screenshot with a new one showing the login form with email and password fields instead of the signup options.]{type="warning"}

::alert[For security best practices, consider changing your password after first login by navigating to your account settings.]{type="info"}

You are ready to proceed to [Template Engineering module](3_template-engineering/index.html)!