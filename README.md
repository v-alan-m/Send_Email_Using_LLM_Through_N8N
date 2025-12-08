# 📧 Email agent in N8N
| ![n8n_screenshot](send_email_workflow.PNG) |
|:------------------------------------------:|

## 🧠 Create and program AI agent
- Click `+ Add first step`
   - Click `AI agent`
<br><br>
- Send to web ChatGPT: Upload image of ./send_email_workflow.PNG + 'Create a system prompt for an n8n 'AI agent', taking into considerations the tools attached based the image attached, that used the "send_email" tools to send emails based on the user's query. it is a helpful assistant that is friendly. As a system prompt for n8n, also add a tool called "contacts_database" that the agent will use to retrieve contact data such as names, email addresses and phone numbers. Please provide it as an easy to copy text snippet as your response.'
<br><br>
- Click on `AI Agent` > `Add Option` > `System Message`
  - Click on `Expression` and click the `enlarge button` on the bottom right on the box.
  - Enter the system prompt [system_prompt.txt](./system_prompt.txt) into the expression text box.

## ⚙️ Configure AI agent with Openai LLM + database + access to Gmail
- Target: Three 
<br><br>
- Click on `Chat Model` + icon under `AI Agent`:
    - Select `OpenAI chat model`
    - Credentials to connect with > Dropdown select `Create New Credential`
- Vist [openai site](https://platform.openai.com/) 
  - Create account > Top right, settings: Click on `gear icon` > Click `API Keys`
  - Click `Create a new secret key` > Type `name` > Click on `Create secret key` > Click on `Copy`
<br><br>
- On n8n in Openai account > Enter `API Key` > Click on `Save`
  - Choose LLM model: gtp-4.1-mini
<br><br>
- To remember previous chats we need memory so click on `Memory`:
    - Select `Simply Memory`
<br><br>
- Click on `'Tool'`:
    - Select `'Gmail'`
    - Click `Email Type` > Select `Text`
      - Click `To` > Click `Expression` for dynamic chat email addresses > `{{` will generate options > {{ $fromAI("emailRecipient") }}
      - Click `Subject` > Click `Expression` > {{ $fromAI("emailSubject") }}
      - Click `Message` > Click `Expression` > {{ $fromAI("emailBody") }}
      - Click on `Add Option` > Click `Append n8n Attribution` > toggle off > Removes the tag: `Send by n8n from emails`
      - Note: If `{{ }}` fields are `red` > Click `Execute step` > Enter `initial` set of `real values`> `{{ }}` will turn green
<br><br>
- Click on the top left name for the windows `Gmail` to rename it to `send_email`, for use in other tools later.

## ☁ Google cloud Configure AI agent with Openai LLM + database + access to Gmail
- Click on `Tool` + icon under `AI Agent` 
  - Select a Google services, these steps below will have to be repeated for each google services, e.g `Google sheet/Gmail/etc.`
  - Using n8n `cloud`: Under `Credentials to connect with` > From dropdown select `Create New Credential` > Click on `open docs`
<br><br>  
- Click on `Create Google account` > Click top left project dropdown > Click on `New Project`
  - Type project name, e.g: `n8n projects` > Click on `Create` > Top right notifications > Click `Select Project`
  - Top left click on `hamburger menu` > Hover on `API and Services` > Click on `OAuth consent screen`
<br><br>
- Click on `Get Started` if you see this option, if not click on `Create OAuth Client`
  - Type App name e.g: `n8n_email_sender` > Select `External`
  - Type contact email `email_address`
  - Type developer email `email_address`
  - Click on `Save and continue` > On next page click on `Save and continue`
<br><br>
- CLick on `Create OAuth Client`
  - Select `Web Application` > Type name: `n8n_sheets`
  - From n8n, copy URL `OAuth Redirect URL` > Google Cloud, Authorized redirect URIs, click on `Add URI` > Paste `OAuth Redirect URL`
  - Click `Create`
    - Copy `Client ID` from Google Cloud > Paste into n8n: `Client ID`
    - Copy `Client Secret` from Google Cloud > Paste into n8n: `Client secret`
  - In GCP click `Audience` from the left menu > Under Test User, click on `Add users`
    - Enter your n8n `email address(es)` > Click on `Add`
  - In n8n click on `Sign in with Google`  
  > - To add more services, such as Gmail, repeat this process, click from the left menu > `Client` > `Create client` 

### 🕹️ Enable Google services in Google Cloud:
  - On `Google Cloud` > Left column, click `Enabled APIs and services`
    - Search for `Google Drive API` > Click on `Enable`
    - Search for `Gmail API` > Click on `Enable`
    - Search for `Google Sheets API` > Click on `Enable`

## 📑 Add a contacts book (so emails can be sent by asking to email a particular person)
- Create a contacts book: Go to `Google sheets`
- Create a table of contacts with `Name`, `Email`, `Phone Number` 
- Name the Google spreadsheet: `contacts_database`
<br><br>
- Click on `Tool`  within the `AI Agent`
    - Click `Google sheets`
    - Credentials to connect with > Dropdown select `Create New Credential` > ...enter_method_here... > ✔
<br><br>
- Click on `Tool`
  - Click on `Documents` > Select `contacts_database`
<br><br>
- Click on the top left name for the windows `Google Sheets` to rename it to `contacts_database`, for use in other tools later.
  - Click on `AI Agent` > `Add Option` > `System Message`
  - Click on `Expression` and click the `enlarge button` on the bottom right on the box.
  - Enter the system prompt ./system_prompt.txt into the expression text box.

## 🚀 Run the program
- Test email > click on the `When chat message received` box and type in: 
`Can you send an email to <your_chosen_gmail_address> stating the top give performing tech stocks of today, the week and this month with ticker symbols and stock prices within a table format where the response data should be a text snippet.`
  - Click on `send_email` then click on `Operations` to see the results!
  - ...Insert_image_of_results_table.png...