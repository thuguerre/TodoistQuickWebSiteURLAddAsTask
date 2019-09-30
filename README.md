***PROJECT UNDER DEVELOPMENT***    
***NOT EVEN IN AN ALPHA VERSION NOW :)***


# Project Description

This **unofficial** Todoist browser extension allows its user to add its current web site's URL as a new Task in his Todoist Inbox, just by clicking on a single button. It completes the official Todoist browser extension, allowing the same use case but with a 4-clicks process.    

For the moment, this project will only provide this single and quite simple functionnality. It may add other ones later, such as a right-click menu, or the possibility to add inner-page links as tasks.     

# For developers and/or contributors

## How to contribute ?

This extension project is now under developement. Contributions are still difficult for the moment. However, you will be able to contribute soon !
- By translating texts other than in English and French,
- By reporting issues, and even fixing them :)

## How it works ?

Basically, the extension has the following process, fired when the user clicks on the extension button :

1. Try to access the Todoist Access Token stored in `Browser Storage`
2. If no Access Token is found, launch [Todoist Authorization Flow](https://developer.todoist.com/sync/v8/#authorization):
    - Prompt the Todoist Authentication Form, using [Browser Identity API](https://developer.mozilla.org/en-US/docs/Mozilla/Add-ons/WebExtensions/API/identity),
    - Get a valid OAuth Code, once user has authenticated
    - Call our `Todoist Proxy API` to transform this OAuth Code in a valid Access Token. ***This step has not been yet implemented, and must be done manually by developer when testing***
    - Store the Access Token in `Browser Storage`, for a future reuse
3. Launch the Task Add Flow:
    - Get active tab, to retrieve its URL
    - Call [Todoist Rest API](https://developer.todoist.com/rest/v1/#create-a-new-task) to add the task
    - If task add is a success, confirm the user the task has been added, using [Browser Notification API](https://developer.mozilla.org/en-US/docs/Mozilla/Add-ons/WebExtensions/API/notifications)
    - If task add has failed due to Authorization reasons, revoke the Access Token and go to `Step 2` 

## How to setup the project ?

### Todoist Account

In order to make this project working and testable, you will need to own a [Todoist Account](https://todoist.com), first to authenticate the extension as any user will do, but also to get application credentials.  

You will also need to declare an application in [Todoist Application Management](https://developer.todoist.com/appconsole.html), in order to get Client Id and Secret values which will have to be set in the following credentials files.  

*I own a premium Todoist account, and currently do not know if a free account is enough.*  

### Credentials

For security reasons, Todoist API credentials are not committed to the Git repository.  
For the moment, a very basic solution is used : credentials are externalized into files which are Git-ignored.  
A better solution will be found as soon as possible.

If you download this source code, to make the Browser Extension working, you have to create a `credentials.js` file in `BrowserExtension` directory. This file must look like:  
> const CLIENT_ID = "12345";  
> const TEMP_TOKEN = "123456";  

where:
- `CLIENT_ID` value is replaced by your Todoist Application Client ID
- `TEMP_TOKEN` value is replaced by a valid token : your own [Todoist API Key](https://todoist.com/prefs/integrations), or a true token generated through the authorization flow (currently manually got from the ProxyAPI).

Moreover, to make the ProxyAPI works, you have to create a `credentials.properties` file in `TodoistProxyAPI/src/main/resources` directory. This file must look like:
> CLIENT_ID=123456  
> CLIENT_SECRET=123456  
> TEMP_CODE=123456  
> TEMP_TOKEN=123465  

where:
- `CLIENT_ID` has the same value as in `credentials.js` file.
- `CLIENT_SECRET` value is the Secret Key got from your [Todoist Application Management](https://developer.todoist.com/appconsole.html).
- `TEMP_CODE` is the code generated by the Browser Extension during Authentication Flow and manually got in the browser console (while the Proxy API is not really deployed). It allows to get a true token got from Todoist API, which can be used in `credentials.js` file, while the `Todoist Proxy API` integration is not done in Browser Extension. 
- `TEMP_TOKEN` is the token got from the previous-line-process, you want to revoke.

### How to test the extension ?

#### Under your Firefox browser
 
- Open `about:debugging` tab 
- Click on `This Firefox`
- Click on `Load Temporary Add-on`
- Choose any file in the `BrowserExtension` directory

*You can have more information about testing an extension on [this page](https://developer.mozilla.org/en-US/docs/Mozilla/Add-ons/WebExtensions/Your_first_WebExtension).*  

Do not hesitate to click on Extension `Debug` button to have a look at the extension's logs.

#### Under your Chrome browser

- Open `chrome://extensions/` tab
- Activate `Developer mode`
- Click on `Load non packed extension`
- Choose `BrowserExtension` directory    

# Resources

- [Todoist REST API Documentation](https://developer.todoist.com/rest/v1/#create-a-new-task), for Task Add Service
- [Todoist Sync API Documentation](https://developer.todoist.com/sync/v8/#authorization), for Todoist Authorization Flow, based on OAuth protocol
- [Browser Extension Mozilla Documentation](https://developer.mozilla.org/en-US/docs/Mozilla/Add-ons/WebExtensions/Your_first_WebExtension)
- [Browser Notifications API](https://developer.mozilla.org/en-US/docs/Mozilla/Add-ons/WebExtensions/API/notifications)
- [Browser Identity API](https://developer.mozilla.org/en-US/docs/Mozilla/Add-ons/WebExtensions/API/identity)
- [Chrome portability from WebExtension](https://developer.mozilla.org/en-US/docs/Mozilla/Add-ons/WebExtensions/Chrome_incompatibilities)
- [Chrome Extension Debugging](https://developer.chrome.com/extensions/tut_debugging)