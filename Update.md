# BiblioTecha
An automated personal library management system that uses Python, Gmail, API, and Google Cloud Functions to track book purchases, manage collections, and alert about duplicate books. I used methods like pandas, re, and isbnlib to attempt to structure the data, pattern matching, and working on communicating with the global book database.

This is my main.py that implements OAuth 2.0 through the token.json to access Gmail API without my password. It also used Gmail Query to filter my messages and operates when triggered, which is weekly.
    
    import functions_framework
    import os
    import json
    from google.oauth2.credentials import Credentials
    from googleapiclient.discovery import build

    @functions_framework.cloud_event
    def run_weekly_scrape(cloud_event):
    # This checks for login key in the current folder
    token_path = 'token.json'
    
    if os.path.exists(token_path):
        with open(token_path, 'r') as f:
            token_data = json.load(f)
        
        # Log in using the stored token
        creds = Credentials.from_authorized_user_info(token_data)
        service = build('gmail', 'v1', credentials=creds)

        # The BiblioTecha search query
        query = 'from:(ship-confirm@amazon.com OR orders@audible.com) "Order Confirmation"'
        results = service.users().messages().list(userId='me', q=query).execute()
        
        messages = results.get('messages', [])
        
        if not messages:
            print("Friday Scrape: No new books found this week.")
        else:
            print(f"Friday Scrape Success: Found {len(messages)} potential books for BiblioTecha!")
    else:
        print("Error: token.json not found. You must upload your token file to the function.")


requirements.txt is the dependency manifest
    
    google-api-python-client
    google-auth-oauthlib
    functions-framework

## enabled gmail API for accessing my email.
<img width="1864" height="660" alt="OAuth2" src="https://github.com/user-attachments/assets/cbb84247-b40e-41e8-8c08-532ce9df8897" />

## I added a Cloud Schduler for weekly triggers, which is on Fridays at 9AM since.
<img width="1575" height="376" alt="CloudScheduler" src="https://github.com/user-attachments/assets/5b86cd9b-9373-4a4d-b1ec-92c0b836fcc6" />

## A successful scrape 
<img width="1871" height="1254" alt="SuccessfulScrape" src="https://github.com/user-attachments/assets/6a2f6984-c635-4baa-8ec1-002f81f30119" />

## I also made a App Script to receive updates from my Python script and sent to my Google Sheets
<img width="975" height="686" alt="image" src="https://github.com/user-attachments/assets/2c785888-f0f9-44b6-8d9f-cb58c6f5a7e2" />

## I have a Google Sheet master copy, which i still need to work on little but is the Book Vault
{https://docs.google.com/spreadsheets/d/1bSOu3PkeZWJapmyebbJEaj1bxzJV3vFLb-pHhYRNdgs/edit?usp=sharing}

## Library Cloud Backlog Completed
<img width="1716" height="994" alt="image" src="https://github.com/user-attachments/assets/414c8b9d-711a-4717-91cb-a8c61d0b5e56" />

## Digital Auto Syncing Completed
<img width="1714" height="1047" alt="image" src="https://github.com/user-attachments/assets/a1902026-1cea-4d62-98fb-f542c385aa27" />

## Burndown Chart:
{https://docs.google.com/spreadsheets/d/1XWVIsQHVC30msORizZhnYbKjmshPoTg3H-2IQtNPyxs/edit?usp=sharing}
