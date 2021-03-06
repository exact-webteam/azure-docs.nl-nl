---
title: 'Zelf studie: gebruikers aanmelden en de Microsoft Graph-API aanroepen in een Elektroon-desktop-app | Azure'
titleSuffix: Microsoft identity platform
description: In deze zelf studie bouwt u een Elektror-desktop-app die gebruikers kan aanmelden en de verificatie code stroom kunt gebruiken om een toegangs token te verkrijgen van het micro soft Identity-platform en de Microsoft Graph-API aan te roepen.
services: active-directory
author: derisen
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: tutorial
ms.date: 01/12/2021
ms.author: v-doeris
ms.openlocfilehash: 82e7d01442d30356a3d8ac68262a1ac49990666c
ms.sourcegitcommit: 227b9a1c120cd01f7a39479f20f883e75d86f062
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/18/2021
ms.locfileid: "100651184"
---
# <a name="tutorial-sign-in-users-and-call-the-microsoft-graph-api-in-an-electron-desktop-app"></a>Zelf studie: gebruikers aanmelden en de Microsoft Graph-API aanroepen in een Elektroon-desktop-app

In deze zelf studie bouwt u een elektron-bureaublad toepassing die zich aanmeldt bij gebruikers en aanroepen Microsoft Graph met behulp van de autorisatie code stroom met PKCE. De bureau blad-app die u bouwt, maakt gebruik [van de micro soft Authentication Library (MSAL) voor Node.js](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-node).

Volg de stappen in deze zelf studie voor het volgende:

> [!div class="checklist"]
> - De app registreren in de Azure Portal
> - Een werkend bureau blad-app-project maken
> - Verificatie logica toevoegen aan uw app
> - Een methode toevoegen om een web-API aan te roepen
> - App-registratie Details toevoegen
> - De app testen

## <a name="prerequisites"></a>Vereisten

- [Node.js](https://nodejs.org/en/download/)
- [Vangst](https://www.electronjs.org/)
- [Visual Studio Code](https://code.visualstudio.com/download) of een andere code-editor

## <a name="register-the-application"></a>De toepassing registreren

Voer eerst de stappen in [een toepassing registreren met het micro soft Identity-platform](quickstart-register-app.md) uit om uw app te registreren.

Gebruik de volgende instellingen voor de registratie van uw app:

- Naam: `ElectronDesktopApp` (aanbevolen)
- Ondersteunde account typen: **accounts in elke organisatie Directory (een Azure AD-adres lijst met meerdere tenants) en persoonlijke micro soft-accounts (bijvoorbeeld Skype, Xbox)**
- Platform type: **mobiele en desktop toepassingen**
- Omleidings-URI: `msal://redirect`

## <a name="create-the-project"></a>Het project maken

Maak een map om uw toepassing te hosten, bijvoorbeeld *ElectronDesktopApp*.

1. Ga eerst naar de projectmap in uw terminal en voer de volgende `npm`-opdrachten uit:

    ```console
    npm init -y
    npm install --save @azure/msal-node axios bootstrap dotenv jquery popper.js
    npm install --save-dev babel electron@10.1.6 webpack
    ```

2. Maak vervolgens een map met de naam *app*. Maak in deze map een bestand met de naam *index.html* dat als gebruikers interface dient. Voeg de volgende code toe:

    ```html
    <!DOCTYPE html>
    <html lang="en">

    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0, shrink-to-fit=no">
        <meta http-equiv="Content-Security-Policy" content="script-src 'self' 'unsafe-inline';" />
        <title>MSAL Node Electron Sample App</title>

        <!-- adding Bootstrap 4 for UI components  -->
        <link rel="stylesheet" href="../node_modules/bootstrap/dist/css/bootstrap.min.css">

        <link rel="SHORTCUT ICON" href="https://c.s-microsoft.com/favicon.ico?v2" type="image/x-icon">
    </head>

    <body>
        <nav class="navbar navbar-expand-lg navbar-dark bg-primary">
            <a class="navbar-brand">Microsoft identity platform</a>
            <div class="btn-group ml-auto dropleft">
                <button type="button" id="signIn" class="btn btn-secondary" aria-expanded="false">
                    Sign in
                </button>
                <button type="button" id="signOut" class="btn btn-success" hidden aria-expanded="false">
                    Sign out
                </button>
            </div>
        </nav>
        <br>
        <h5 class="card-header text-center">Electron sample app calling MS Graph API using MSAL Node</h5>
        <br>
        <div class="row" style="margin:auto">
            <div id="cardDiv" class="col-md-3" style="display:none">
                <div class="card text-center">
                    <div class="card-body">
                        <h5 class="card-title" id="WelcomeMessage">Please sign-in to see your profile and read your mails
                        </h5>
                        <div id="profileDiv"></div>
                        <br>
                        <br>
                        <button class="btn btn-primary" id="seeProfile">See Profile</button>
                        <br>
                        <br>
                        <button class="btn btn-primary" id="readMail">Read Mails</button>
                    </div>
                </div>
            </div>
            <br>
            <br>
            <div class="col-md-4">
                <div class="list-group" id="list-tab" role="tablist">
                </div>
            </div>
            <div class="col-md-5">
                <div class="tab-content" id="nav-tabContent">
                </div>
            </div>
        </div>
        <br>
        <br>

        <script>
            window.jQuery = window.$ = require('jquery');
            require("./renderer.js");
        </script>

        <!-- importing bootstrap.js and supporting js libraries -->
        <script src="../node_modules/jquery/dist/jquery.js"></script>
        <script src="../node_modules/popper.js/dist/umd/popper.js"></script>
        <script src="../node_modules/bootstrap/dist/js/bootstrap.js"></script>
    </body>

    </html>
    ```

3. Maak vervolgens een bestand met de naam *main.js* en voeg de volgende code toe:

    ```JavaScript
    require('dotenv').config()

    const path = require('path');
    const { app, ipcMain, BrowserWindow } = require('electron');
    const { IPC_MESSAGES } = require('./constants');

    const { callEndpointWithToken } = require('./fetch');
    const AuthProvider = require('./AuthProvider');

    const authProvider = new AuthProvider();
    let mainWindow;

    function createWindow () {
        mainWindow = new BrowserWindow({
            width: 800,
            height: 600,
            webPreferences: {
            nodeIntegration: true
            }
        });

        mainWindow.loadFile(path.join(__dirname, './index.html'));
        };

    app.on('ready', () => {
        createWindow();
    });

    app.on('window-all-closed', () => {
        app.quit();
    });


    // Event handlers
    ipcMain.on(IPC_MESSAGES.LOGIN, async() => {
        const account = await authProvider.login(mainWindow);

        await mainWindow.loadFile(path.join(__dirname, './index.html'));

        mainWindow.webContents.send(IPC_MESSAGES.SHOW_WELCOME_MESSAGE, account);
    });

    ipcMain.on(IPC_MESSAGES.LOGOUT, async() => {
        await authProvider.logout();
        await mainWindow.loadFile(path.join(__dirname, './index.html'));
    });

    ipcMain.on(IPC_MESSAGES.GET_PROFILE, async() => {

        const tokenRequest = {
            scopes: ['User.Read'],
        };

        const token = await authProvider.getToken(mainWindow, tokenRequest);
        const account = authProvider.account

        await mainWindow.loadFile(path.join(__dirname, './index.html'));

        const graphResponse = await callEndpointWithToken(`${process.env.GRAPH_ENDPOINT_HOST}${process.env.GRAPH_ME_ENDPOINT}`, token);

        mainWindow.webContents.send(IPC_MESSAGES.SHOW_WELCOME_MESSAGE, account);
        mainWindow.webContents.send(IPC_MESSAGES.SET_PROFILE, graphResponse);
    });

    ipcMain.on(IPC_MESSAGES.GET_MAIL, async() => {

        const tokenRequest = {
            scopes: ['Mail.Read'],
        };

        const token = await authProvider.getToken(mainWindow, tokenRequest);
        const account = authProvider.account;

        await mainWindow.loadFile(path.join(__dirname, './index.html'));

        const graphResponse = await callEndpointWithToken(`${process.env.GRAPH_ENDPOINT_HOST}${process.env.GRAPH_MAIL_ENDPOINT}`, token);

        mainWindow.webContents.send(IPC_MESSAGES.SHOW_WELCOME_MESSAGE, account);
        mainWindow.webContents.send(IPC_MESSAGES.SET_MAIL, graphResponse);
    });
    ```

In het bovenstaande code fragment initialiseert we een hoofd venster van het elektro werk en maken ze een aantal gebeurtenis-handlers voor interacties met het elektron-venster. We importeren ook configuratie parameters, instantiëren van *authProvider* -klasse voor het afhandelen van aanmelding, afmelding en het ophalen van tokens, en aanroepen van de Microsoft Graph-API.

4. Maak in dezelfde map (*app*) een ander bestand met de naam *renderer.js* en voeg de volgende code toe:

    ```JavaScript
    const { ipcRenderer } = require('electron');
    const { IPC_MESSAGES } = require('./constants');

    // UI event handlers
    document.querySelector('#signIn').addEventListener('click', () => {
        ipcRenderer.send(IPC_MESSAGES.LOGIN);
    });

    document.querySelector('#signOut').addEventListener('click', () => {
        ipcRenderer.send(IPC_MESSAGES.LOGOUT);
    });

    document.querySelector('#seeProfile').addEventListener('click', () => {
        ipcRenderer.send(IPC_MESSAGES.GET_PROFILE);
    });

    document.querySelector('#readMail').addEventListener('click', () => {
        ipcRenderer.send(IPC_MESSAGES.GET_MAIL);
    });

    // Main process message subscribers
    ipcRenderer.on(IPC_MESSAGES.SHOW_WELCOME_MESSAGE, (event, account) => {
        showWelcomeMessage(account);
    });

    ipcRenderer.on(IPC_MESSAGES.SET_PROFILE, (event, graphResponse) => {
        updateUI(graphResponse, `${process.env.GRAPH_ENDPOINT_HOST}${process.env.GRAPH_ME_ENDPOINT}`);
    });

    ipcRenderer.on(IPC_MESSAGES.SET_MAIL, (event, graphResponse) => {
        updateUI(graphResponse, `${process.env.GRAPH_ENDPOINT_HOST}${process.env.GRAPH_MAIL_ENDPOINT}`);
    });

    // DOM elements to work with
    const welcomeDiv = document.getElementById("WelcomeMessage");
    const signInButton = document.getElementById("signIn");
    const signOutButton = document.getElementById("signOut");
    const cardDiv = document.getElementById("cardDiv");
    const profileDiv = document.getElementById("profileDiv");
    const tabList = document.getElementById("list-tab");
    const tabContent = document.getElementById("nav-tabContent");

    function showWelcomeMessage(account) {
        cardDiv.style.display = "initial";
        welcomeDiv.innerHTML = `Welcome ${account.name}`;
        signInButton.hidden = true;
        signOutButton.hidden = false;
    }

    function clearTabs() {
        tabList.innerHTML = "";
        tabContent.innerHTML = "";
    }

    function updateUI(data, endpoint) {

        console.log(`Graph API responded at: ${new Date().toString()}`);

        if (endpoint === `${process.env.GRAPH_ENDPOINT_HOST}${process.env.GRAPH_ME_ENDPOINT}`) {
            setProfile(data);
        } else if (endpoint === `${process.env.GRAPH_ENDPOINT_HOST}${process.env.GRAPH_MAIL_ENDPOINT}`) {
            setMail(data);
        }
    }

    function setProfile(data) {
        profileDiv.innerHTML = ''

        const title = document.createElement('p');
        const email = document.createElement('p');
        const phone = document.createElement('p');
        const address = document.createElement('p');

        title.innerHTML = "<strong>Title: </strong>" + data.jobTitle;
        email.innerHTML = "<strong>Mail: </strong>" + data.mail;
        phone.innerHTML = "<strong>Phone: </strong>" + data.businessPhones[0];
        address.innerHTML = "<strong>Location: </strong>" + data.officeLocation;

        profileDiv.appendChild(title);
        profileDiv.appendChild(email);
        profileDiv.appendChild(phone);
        profileDiv.appendChild(address);
    }

    function setMail(data) {
        const mailInfo = data;
        if (mailInfo.value.length < 1) {
            alert("Your mailbox is empty!")
        } else {
            clearTabs();
            mailInfo.value.slice(0, 10).forEach((d, i) => {
                    createAndAppendListItem(d, i);
                    createAndAppendContentItem(d, i);
            });
        }
    }

    function createAndAppendListItem(d, i) {
        const listItem = document.createElement("a");
        listItem.setAttribute("class", "list-group-item list-group-item-action")
        listItem.setAttribute("id", "list" + i + "list")
        listItem.setAttribute("data-toggle", "list")
        listItem.setAttribute("href", "#list" + i)
        listItem.setAttribute("role", "tab")
        listItem.setAttribute("aria-controls", i)
        listItem.innerHTML = d.subject;
        tabList.appendChild(listItem);
    }

    function createAndAppendContentItem(d, i) {
        const contentItem = document.createElement("div");
        contentItem.setAttribute("class", "tab-pane fade")
        contentItem.setAttribute("id", "list" + i)
        contentItem.setAttribute("role", "tabpanel")
        contentItem.setAttribute("aria-labelledby", "list" + i + "list")

        if (d.from) {
            contentItem.innerHTML = "<strong> from: " + d.from.emailAddress.address + "</strong><br><br>" + d.bodyPreview + "...";
            tabContent.appendChild(contentItem);
        }
    }
    ```

5. Maak ten slotte een bestand met de naam *constants.js* dat de teken reeks constanten opslaat voor het beschrijven van de toepassings **gebeurtenissen**:

    ```JavaScript
    const IPC_MESSAGES = {
        SHOW_WELCOME_MESSAGE: 'SHOW_WELCOME_MESSAGE',
        LOGIN: 'LOGIN',
        LOGOUT: 'LOGOUT',
        GET_PROFILE: 'GET_PROFILE',
        SET_PROFILE: 'SET_PROFILE',
        GET_MAIL: 'GET_MAIL',
        SET_MAIL: 'SET_MAIL'
    }

    module.exports = {
        IPC_MESSAGES: IPC_MESSAGES,
    }
    ```

U hebt nu een eenvoudige gebruikers interface en interacties voor uw elektron-app. Nadat u de rest van de zelfstudie hebt voltooid, moeten de bestands- en mapstructuur van het project er ongeveer als volgt uitzien:

```
ElectronDesktopApp/
├── App
│   ├── authProvider.js
│   ├── constants.js
│   ├── fetch.js
│   ├── main.js
│   ├── renderer.js
│   ├── index.html
├── package.json
└── .env
```

## <a name="add-authentication-logic-to-your-app"></a>Verificatie logica toevoegen aan uw app

Maak in de map *app* een bestand met de naam *AuthProvider.js*. Dit bevat een verificatie provider klasse waarmee aanmelding, afmelding, token aanschaf, account selectie en gerelateerde verificatie taken worden verwerkt met behulp van het MSAL-knoop punt. Voeg de volgende code toe:

```JavaScript
const { PublicClientApplication, LogLevel, CryptoProvider } = require('@azure/msal-node');
const { protocol } = require('electron');
const path = require('path');
const url = require('url');

/**
 * To demonstrate best security practices, this Electron sample application makes use of 
 * a custom file protocol instead of a regular web (https://) redirect URI in order to 
 * handle the redirection step of the authorization flow, as suggested in the OAuth2.0 specification for Native Apps.
 */
const CUSTOM_FILE_PROTOCOL_NAME = process.env.REDIRECT_URI.split(':')[0]; // e.g. msal://redirect

/**
 * Configuration object to be passed to MSAL instance on creation. 
 * For a full list of MSAL Node configuration parameters, visit:
 * https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-node/docs/configuration.md 
 */
const MSAL_CONFIG = {
    auth: {
        clientId: process.env.CLIENT_ID,
        authority: `${process.env.AAD_ENDPOINT_HOST}${process.env.TENANT_ID}`,
        redirectUri: process.env.REDIRECT_URI,
    },
    system: {
        loggerOptions: {
            loggerCallback(loglevel, message, containsPii) {
                console.log(message);
            },
            piiLoggingEnabled: false,
            logLevel: LogLevel.Verbose,
        }
    }
};

class AuthProvider {

    clientApplication;
    cryptoProvider;
    authCodeUrlParams;
    authCodeRequest;
    pkceCodes;
    account;

    constructor() {
        /**
         * Initialize a public client application. For more information, visit:
         * https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-node/docs/initialize-public-client-application.md 
         */
        this.clientApplication = new PublicClientApplication(MSAL_CONFIG);
        this.account = null;

        // Initialize CryptoProvider instance
        this.cryptoProvider = new CryptoProvider();

        this.setRequestObjects();
    }

    /**
     * Initialize request objects used by this AuthModule.
     */
    setRequestObjects() {
        const requestScopes =  ['openid', 'profile', 'User.Read'];
        const redirectUri = process.env.REDIRECT_URI;

        this.authCodeUrlParams = {
            scopes: requestScopes,
            redirectUri: redirectUri
        };

        this.authCodeRequest = {
            scopes: requestScopes,
            redirectUri: redirectUri,
            code: null
        }

        this.pkceCodes = {
            challengeMethod: "S256", // Use SHA256 Algorithm
            verifier: "", // Generate a code verifier for the Auth Code Request first
            challenge: "" // Generate a code challenge from the previously generated code verifier
        };
    }

    async login(authWindow) {
        const authResult = await this.getTokenInteractive(authWindow, this.authCodeUrlParams);
        return this.handleResponse(authResult);
    }

    async logout() {
        if (this.account) {
            await this.clientApplication.getTokenCache().removeAccount(this.account);
            this.account = null;
        }
    }

    async getToken(authWindow, tokenRequest) {
        let authResponse;
        
        authResponse = await this.getTokenInteractive(authWindow, tokenRequest);
        
        return authResponse.accessToken || null;
    }

    // This method contains an implementation of access token acquisition in authorization code flow
    async getTokenInteractive(authWindow, tokenRequest) {

        /**
         * Proof Key for Code Exchange (PKCE) Setup
         * 
         * MSAL enables PKCE in the Authorization Code Grant Flow by including the codeChallenge and codeChallengeMethod parameters
         * in the request passed into getAuthCodeUrl() API, as well as the codeVerifier parameter in the
         * second leg (acquireTokenByCode() API).
         * 
         * MSAL Node provides PKCE Generation tools through the CryptoProvider class, which exposes
         * the generatePkceCodes() asynchronous API. As illustrated in the example below, the verifier
         * and challenge values should be generated previous to the authorization flow initiation.
         * 
         * For details on PKCE code generation logic, consult the 
         * PKCE specification https://tools.ietf.org/html/rfc7636#section-4
         */

        const {verifier, challenge} = await this.cryptoProvider.generatePkceCodes();

        this.pkceCodes.verifier = verifier;
        this.pkceCodes.challenge = challenge;

        const authCodeUrlParams = { 
            ...this.authCodeUrlParams, 
            scopes: tokenRequest.scopes,
            codeChallenge: this.pkceCodes.challenge, // PKCE Code Challenge
            codeChallengeMethod: this.pkceCodes.challengeMethod // PKCE Code Challenge Method 
        };

        const authCodeUrl = await this.clientApplication.getAuthCodeUrl(authCodeUrlParams);

        protocol.registerFileProtocol(CUSTOM_FILE_PROTOCOL_NAME, (req, callback) => {
            const requestUrl = url.parse(req.url, true);
            callback(path.normalize(`${__dirname}/${requestUrl.path}`));
        });

        const authCode = await this.listenForAuthCode(authCodeUrl, authWindow);
        
        const authResponse = await this.clientApplication.acquireTokenByCode({ 
            ...this.authCodeRequest, 
            scopes: tokenRequest.scopes, 
            code: authCode,
            codeVerifier: this.pkceCodes.verifier // PKCE Code Verifier 
        });
        
        return authResponse;
    }

    // Listen for authorization code response from Azure AD
    async listenForAuthCode(navigateUrl, authWindow) {
        
        authWindow.loadURL(navigateUrl);

        return new Promise((resolve, reject) => {
            authWindow.webContents.on('will-redirect', (event, responseUrl) => {
                try {
                    const parsedUrl = new URL(responseUrl);
                    const authCode = parsedUrl.searchParams.get('code');
                    resolve(authCode);
                } catch (err) {
                    reject(err);
                }
            });
        });
    }

    /**
     * Handles the response from a popup or redirect. If response is null, will check if we have any accounts and attempt to sign in.
     * @param response 
     */
    async handleResponse(response) {
        if (response !== null) {
            this.account = response.account;
        } else {
            this.account = await this.getAccount();
        }

        return this.account;
    }

    /**
     * Calls getAllAccounts and determines the correct account to sign into, currently defaults to first account found in cache.
     * https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-common/docs/Accounts.md
     */
    async getAccount() {
        const cache = this.clientApplication.getTokenCache();
        const currentAccounts = await cache.getAllAccounts();

        if (currentAccounts === null) {
            console.log('No accounts detected');
            return null;
        }

        if (currentAccounts.length > 1) {
            // Add choose account code here
            console.log('Multiple accounts detected, need to add choose account code.');
            return currentAccounts[0];
        } else if (currentAccounts.length === 1) {
            return currentAccounts[0];
        } else {
            return null;
        }
    }
}

module.exports = AuthProvider;
```

In het bovenstaande code fragment hebben we eerst MSAL-knoop punt geïnitialiseerd `PublicClientApplication` door een configuratie object () door te geven `msalConfig` . We zijn vervolgens `login` beschikbaar `logout` en de `getToken` methoden die moeten worden aangeroepen door de main module (*main.js*). In `login` en `getToken` verkrijgen wij respectievelijk id-en toegangs tokens door eerst een autorisatie code aan te vragen en deze vervolgens uit te wisselen met een token met behulp van open bare API van het MSAL-knoop punt `acquireTokenByCode` .

## <a name="add-a-method-to-call-a-web-api"></a>Een methode toevoegen om een web-API aan te roepen

Maak een ander bestand met de naam *fetch.js*. Dit bestand bevat een Axios HTTP-client voor het aanbrengen van REST-aanroepen naar de Microsoft Graph-API.

```JavaScript
const axios = require('axios');

/**
 * Makes an Authorization 'Bearer' request with the given accessToken to the given endpoint.
 * @param endpoint 
 * @param accessToken 
 */
async function callEndpointWithToken(endpoint, accessToken) {
    const options = {
        headers: {
            Authorization: `Bearer ${accessToken}`
        }
    };

    console.log('Request made at: ' + new Date().toString());

    const response = await axios.default.get(endpoint, options);

    return response.data;
}

module.exports = {
    callEndpointWithToken: callEndpointWithToken,
};
```

## <a name="add-app-registration-details"></a>App-registratie Details toevoegen

Maak tot slot een omgevings bestand om de app-registratie gegevens op te slaan die worden gebruikt bij het ophalen van tokens. Als u dit wilt doen, maakt u een bestand met de naam *. env* in de hoofdmap van het voor beeld (*ElectronDesktopApp*) en voegt u de volgende code toe:

```
# Credentials
CLIENT_ID=Enter_the_Application_Id_Here
TENANT_ID=Enter_the_Tenant_Id_Here

# Configuration
REDIRECT_URI=msal://redirect

# Endpoints
AAD_ENDPOINT_HOST=Enter_the_Cloud_Instance_Id_Here
GRAPH_ENDPOINT_HOST=Enter_the_Graph_Endpoint_Here

# RESOURCES
GRAPH_ME_ENDPOINT=v1.0/me
GRAPH_MAIL_ENDPOINT=v1.0/me/messages

# SCOPES
GRAPH_SCOPES=User.Read Mail.Read
```

Vul deze gegevens in met de waarden die u hebt opgehaald via de Azure app Registration-portal:

- `Enter_the_Tenant_Id_here` een van de volgende waarden zijn:
  - Als uw toepassing ondersteuning biedt voor *accounts in deze organisatiemap*, vervang deze waarde dan door de **Tenant-id** of **Tenantnaam**. Bijvoorbeeld `contoso.microsoft.com`.
  - Als uw toepassing ondersteuning biedt voor *accounts in elke organisatiemap*, vervangt u waarde door `organizations`.
  - Als uw toepassing *accounts in elke organisatiemap en persoonlijke Microsoft-accounts* ondersteunt, vervang deze waarde dan door `common`.
  - Als u de ondersteuning wilt beperken tot *alleen persoonlijke Microsoft-accounts*, vervang deze waarde dan door `consumers`.
- `Enter_the_Application_Id_Here`: De **Id van de toepassing (client)** van de toepassing die u hebt geregistreerd.
- `Enter_the_Cloud_Instance_Id_Here`: Het Azure-cloudexemplaar waarin uw toepassing is geregistreerd.
  - Voer `https://login.microsoftonline.com/` in voor de hoofd- (of *globale*) Azure-cloud.
  - Voor **nationale** clouds (bijvoorbeeld China) kunt u de juiste waarden vinden in [Nationale clouds](authentication-national-cloud.md).
- `Enter_the_Graph_Endpoint_Here` is het exemplaar van de Microsoft Graph API waarmee de toepassing moet communiceren.
  - Vervang beide exemplaren van deze teken reeks door `https://graph.microsoft.com/` voor het **wereldwijde** Microsoft Graph API-eindpunt.
  - Zie [Nationale cloudimplementaties](/graph/deployments) in de Microsoft Graph-documentatie voor eindpunten in **nationale** cloudimplementaties.

## <a name="test-the-app"></a>De app testen

Het maken van de toepassing is voltooid en u bent nu klaar om de elektron-desktop-app te starten en de functionaliteit van de app te testen.

1. Start de app door de volgende opdracht uit te voeren in de hoofdmap van de projectmap:

```console
electron App/main.js
```

2. In het hoofd venster van de toepassing ziet u de inhoud van uw *index.html* -bestand en de knop **Aanmelden** .

## <a name="test-sign-in-and-sign-out"></a>Aanmelden en afmelden testen

Nadat de *index.html* -bestand is geladen, selecteert u **Aanmelden**. U wordt gevraagd om u aan te melden met het micro soft Identity-platform:

:::image type="content" source="media/tutorial-v2-nodejs-desktop/desktop-01-signin-prompt.png" alt-text="aanmeldings prompt":::

Als u akkoord gaat met de aangevraagde machtigingen, wordt in de webtoepassingen uw gebruikersnaam weer gegeven, met een geslaagde aanmelding:

:::image type="content" source="media/tutorial-v2-nodejs-desktop/desktop-03-after-signin.png" alt-text="geslaagde aanmelding":::

## <a name="test-web-api-call"></a>Web-API-aanroep testen

Nadat u zich hebt aangemeld, selecteert u **Profiel bekijken** om de profielgegevens van de gebruiker weer te geven die zijn geretourneerd in het antwoord van de aanroep van de Microsoft Graph API:

:::image type="content" source="media/tutorial-v2-nodejs-desktop/desktop-04-profile.png" alt-text="Profiel gegevens van Microsoft Graph":::

Selecteer **E-mails lezen** om de berichten in het gebruikers account weer te geven. Er wordt een venster met een toestemming weer gegeven:

:::image type="content" source="media/tutorial-v2-nodejs-desktop/desktop-05-consent-mail.png" alt-text="toestemming scherm voor lezen. mail machtiging":::

Na toestemming geeft u de berichten weer die zijn geretourneerd in het antwoord van de aanroep van de Microsoft Graph-API:

:::image type="content" source="media/tutorial-v2-nodejs-desktop/desktop-06-mails.png" alt-text="e-mail gegevens van Microsoft Graph":::

## <a name="how-the-application-works"></a>Hoe de toepassing werkt

Wanneer een gebruiker de **aanmeldings** knop voor de eerste keer selecteert, wordt de `getTokenInteractive` methode Get van *AuthProvider.js* aangeroepen. Met deze methode wordt de gebruiker omgeleid om zich aan te melden met het *micro soft Identity platform-eind punt* en de referenties van de gebruiker te valideren, en wordt vervolgens een **autorisatie code** opgehaald. Deze code wordt vervolgens uitgewisseld voor een toegangs token met behulp `acquireTokenByCode` van een open bare API van het MSAL-knoop punt.

Op dit moment wordt een met PKCE beveiligde autorisatiecode verzonden naar het eindpunt van het met CORS beveiligde token en wordt deze uitgewisseld voor tokens. Een ID-token, toegangs token en vernieuwings token worden ontvangen door uw toepassing en verwerkt door het MSAL-knoop punt, en de informatie in de tokens wordt opgeslagen in de cache.

Het ID-token bevat basisinformatie over de gebruiker, zoals de weergavenaam. Het toegangstoken heeft een beperkte levensduur en verloopt na 24 uur. Als u van plan bent om deze tokens te gebruiken voor toegang tot beveiligde bronnen, *moet* uw back-endserver de server valideren om te garanderen dat het token is uitgegeven aan een geldige gebruiker voor uw toepassing.

De bureau blad-app die u in deze zelf studie hebt gemaakt, maakt een REST-aanroep van de Microsoft Graph-API met behulp van een toegangs token als Bearer-token in de aanvraag header ([RFC 6750](https://tools.ietf.org/html/rfc6750)).

De Microsoft Graph API vereist het bereik *user.read* om het profiel van een gebruiker te lezen. Dit bereik wordt standaard automatisch toegevoegd in elke toepassing die is geregistreerd in de Azure-portal. Overige API's voor Microsoft Graph, evenals aangepaste API's voor uw back-endserver, vereisen mogelijk aanvullende bereiken. De Microsoft Graph API vereist bijvoorbeeld het bereik *Mail.Read* om de e-mail van de gebruiker op te sommen.

Wanneer u scopes toevoegt, kunnen uw gebruikers worden gevraagd om extra toestemming te geven voor de toegevoegde scopes.

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]

## <a name="next-steps"></a>Volgende stappen

Als u meer wilt weten over het ontwikkelen van Node.js en elektroden op desktop computers op het micro soft-identiteits platform, raadpleegt u onze scenario reeks met meerdere onderdelen:

> [!div class="nextstepaction"]
> [Scenario: Bureaublad-app die web-API's aanroept](scenario-desktop-overview.md)
