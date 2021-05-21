---
title: "Tutorial: Add Microsoft login button to React SPA"
description: Azure Active Directory authentication presented in this tutorial is a login and logout button, and access to a user's username (email). Develop the TypeScript application with an Azure client-side SDK, `@azure/msal-browser`, to manage the interaction of the user in the single page application (SPA).
ms.topic: tutorial
ms.date: 05/21/2021
ms.custom: devx-track-js, "azure-sdk-javascript-@azure/msal-browser-2.7.0"
ms.history: [
"20210216:fix public issue 443"
]
---

# Add Microsoft login button to a single page application for authentication

Azure authentication presented in this TypeScript tutorial is a login and logout button, and provides access to a user's account. Develop the application with an Azure client-side SDK, `@azure/msal-browser`, to manage the interaction of the user in the single page application (SPA).

* [Sample code](https://github.com/Azure-Samples/js-e2e-client-azure-login-button)

## Application architecture and functionality

The SPA built in this tutorial is a React app (create-react-app) with the following tasks:

- Login using a Microsoft-supported login such as Office 365 or Outlook.com
- Log off from the application

To provide a quick and simple single page application, the sample uses **create-react-app** with TypeScript. This front-end framework provides several shortcuts in typical client development with Azure SDKs:

- Bundling, required for Azure SDKs used in a client-application
- Environment variables in the `.env` file

## 1. Set up development environment

Verify the following software is installed on your local computer.

- An Azure user account with an active subscription. [Create one for free](https://azure.microsoft.com/free/).
- [Node.js and npm](https://nodejs.org/en/download) - installed to your local machine.
- [Visual Studio Code](https://code.visualstudio.com/) or an equivalent IDE with Bash shell or terminal - installed to your local machine. 

## 2. Keep value for environment variable

Set aside a place to copy your App registration's client ID value, such as a text file. You will get this client ID in step 5 of the next section. The value will be used as an environment variable for the web app.  

## 3. Create App registration for authentication

1. **Sign in** to [Azure portal](https://portal.azure.com/?quickstart=True#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredApps) for the Default Directory's App registrations.
1. Select **+ New Registration**.
1. **Enter your app registration data** using the following table:

   | Field                   | Value                                                                                                                                                                      |Description|
   | ----------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |--|
   | Name                    | `Simple Auth Tutorial`|This is the app name user's will see on the permission form when they sign in to your app.                                                 |
   | Supported account types | **Accounts in any organizational directory (Any Azure AD directory - Multitenant) and personal Microsoft accounts**|This will cover most account types. |
   | Redirect URI type           | **Single Page Application (SPA)**                                                                                        | |
   | Redirect URI value           | `http://localhost:3000` | The url to return to after authentication succeeds or fails.                                                                                        |

    :::image type="content" source="../media/tutorial-login-button/azure-active-directory-create-new-app-registration.png" alt-text="Azure new app registration.":::  

1. Select **Register**. Wait for the app registration process to complete.
1. **Copy the Application (client) ID** from the Overview section of the app registration. You will add this value to your environment variable for the client app later.

## 4. Create React single page application for TypeScript

1. In a Bash shell, **create a create-react-app** using TypeScript as the language:

   ```bash
   npx create-react-app tutorial-demo-login-button --template typescript
   ```

1. Change into the new directory and install the `@azure/msal-browser` authentication package:

   ```bash
   cd tutorial-demo-login-button && npm install @azure/msal-browser
   ```

1. Create a `.env` file at the root level and add the following line:

    :::code language="env" source="~/../js-e2e-client-azure-login-button/.env"  :::

    The `.env` file is read as part of the create-react-app framework. This file is where you can store the client ID for local development. 

1. Copy your Application (client) ID into the value.

## 5. Add login and logoff buttons

1. Create a subfolder named `azure`, for the Azure-specific files, within the `./src` folder. 

1. Create a new file for authentication configuration in the `azure` folder, named `azure-authentication-config.ts` and copy in the following code:

    :::code language="typescript" source="~/../js-e2e-client-azure-login-button/src/azure/azure-authentication-config.ts"  highlight="3-4,, 11-12":::

    This file reads your application ID in from the `.env` file, sets session as the browser storage instead of cookies, and provides logging that is considerate of personal information.

1. Create a new file for the Azure authentication middleware in the `azure` folder, named `azure-authentication-context.ts` and copy in the following code:

    :::code language="typescript" source="~/../js-e2e-client-azure-login-button/src/azure/azure-authentication-context.ts"  highlight="43, 58, 65":::

    This file provides the authentication to Azure for a user with the `login` and `logout` functions.

1. Create a new file for the user interface button component file in the `azure` folder, named `azure-authentication-component.tsx` and copy in the following code:

   :::code language="typescript" source="~/../js-e2e-client-azure-login-button/src/azure/azure-authentication-component.tsx"  highlight="3, 11, 23, 29, 36":::

   This button component logs in a user, and passes back the user account to the calling/parent component.

   The button text and functionality is toggled based on if the user is currently logged in, captured with the `onAuthenticated` function as a property passed into the component.

   When a user logs in, the button calls Azure authentication library method, `authenticationModule.login` with `returnedAccountInfo` as the callback function. The returned user account is then passed back to the parent component with the `onAuthenticated` function.


1. Open the `./src/App.tsx` file and replace all the code with the following code to incorporate the Login/Logout button component:

   :::code language="typescript" source="~/../js-e2e-client-azure-login-button/src/App.tsx"  highlight="10, 37-42":::

    After a user logs on, and the authentication redirects back to this app, the currentUser object is displayed. 

## 6. Run React SPA app with login button

1. At the Visual Studio Code terminal, start the app:

    ```bash
    npm run start
    ```

    Watch the VSCode integrated for the notice that the app is completely started.

    ```console
    Compiled successfully!
    
    You can now view js-e2e-client-azure-login-button in the browser.
    
      Local:            http://localhost:3000
      On Your Network:  http://x.x.x.x:3000
    
    Note that the development build is not optimized.
    To create a production build, use yarn build.
    ```

1. Open the web app in a browser, `http://localhost:3000`.

1. Select the **Login** button in the web browser. 

    :::image type="content" source="../media/tutorial-login-button/create-react-app-before-authentication-login-button-display.png" alt-text="Select the **Login** button.":::  

1. Select a user account. It doesn't have to be the same account you used to access the Azure portal, but it should be an account that provides Microsoft authentication.

    :::image type="content" source="../media/tutorial-login-button/authentication-popup-select-user-account.png" alt-text="Select a user account. It doesn't have to be the same account you used to access the Azure portal, but it should be an account that provides Microsoft authentication.":::

1. Review the pop-up showing the 1) user name, 2) app name, 3) permissions you are agreeing to, and then select **Yes**.

    :::image type="content" source="../media/tutorial-login-button/authentication-popup-let-this-app-access-your-info.png" alt-text="Review the pop-up showing the 1) user name, 2) app name, 3) permissions you are agreeing to, and then select `Yes`.":::

1. Review the user account information. 

    :::image type="content" source="../media/tutorial-login-button/create-react-app-after-authentication-login-button-succeeds.png" alt-text="Review the user account information.":::  

1. Select the **Logout** button from the app. The app also provides convenient links to the Microsoft user apps to revoke permissions. 

## 7. Store application-specific user information

Optionally, you can store the user ID in your own application database to correlate between the Microsoft provider identity and the user's data required in your own application. The [token claim](/azure/active-directory/develop/id-tokens) contains two IDs you may want to keep track of are:

* **sub**: The ID that is specific to the user, for your specific Active Directory app ID. This is the ID you should store in your own application database, if you need to correlate your custom app's data to your Microsoft Identity provider user. 
* **oid**: This is the universal user ID across all apps in the Microsoft Identity provider. Store this value if you need to track your user across several apps in the Identity provider.

## 8. Clean up resources

When you are done using this tutorial, delete the application from the Azure portal [App registration list](https://portal.azure.com/?quickstart=True#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredApps).

If you want to keep the app but revoke the permission given to the app by your specific user account, use one of the following links:

* [Revoke AAD permission](https://myapps.microsoft.com/)
* [Revoke Consumer permission](https://account.live.com/consent/manage)

## Next steps

This app provides user authentication for your app, and returns user information. The authentication functionality can stop here for a simple version of an app or you can add functionality to manage the app's user management and user authorization to app features. 

User management can be stored in an Azure Active Directory or your own database, depending on the functionality and tools you select. 

User authorization can be provided by Azure, or you can develop authorization without Azure, or you can combine the two for a custom experience of authorization, roles, and app features. 

* Continuing using the [MSAL library](/azure/active-directory/develop/msal-overview) to get the user profile and provide silent sign-on
* Add [Microsoft Graph](/graph/overview) to access user accounts in Microsoft 365 include email and calendar appointments