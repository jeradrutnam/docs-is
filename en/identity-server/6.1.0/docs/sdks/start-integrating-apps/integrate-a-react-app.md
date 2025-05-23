# Integrate a React application
Follow the steps given below to authenticate users to a sample react service provider with OpenID Connect.

## Prerequisites
Only customer users can log in to applications. [Create a user account]({{base_path}}/guides/identity-lifecycles/onboard-overview) if you don't already have one.

## Add CORS configurations

To add CORS configurations:

1. Open the `deployment.toml` file found in the `<IS-Home>/repository/conf/` directory, and add the following configuration.

    ``` toml
    [cors]
    allowed_origins = [
    "https://localhost:3000"
    ]
    supported_methods = [
    "GET",
    "POST",
    "HEAD",
    "OPTIONS",
    "PUT",
    "PATCH",
    "HEAD",
    "DELETE",
    "PATCH"
    ]
    ```

2. Restart the WSO2 IS to make the changes effective.

!!! info
    For more information on CORS configurations, refer [how to configure CORS during deployment]({{base_path}}/deploy/configure-cors/#configuring-cors-during-deployment).

## Configure a service provider

1. On the WSO2 Identity Server management console (`https://<IS_HOST>:<PORT>/carbon`) go to **Main** > **Identity** > **Service Providers**.
2. Click **Add** and enter a **Service Provider Name**.
3. Click **Register** to complete the registration.
4. Expand **Inbound Authentication Configuration** > **OAuth/OpenID Connect Configuration** and click **Configure**.
5. Enter the **Callback URL** as `https://localhost:3000`.

    !!! note
        The **Callback URL** is the exact location in the service provider's application to which an access token will be sent. This URL should be the landing page to which the user is redirected after successful authentication.

6. Enable **Allow authentication without the client secret** checkbox.

    !!! warning
        If this checkbox is not enabled, users will experience login failures as we provide authentication using the client ID only.

7. Click **Add** to complete the configuration. 

    !!! note
        Make a note of the **OAuth Client Key** and **OAuth Client Secret** that appear, as they will be used to configure the sample application.

## Configure the resident identity provider

Follow this step, only if you are running the WSO2 Identity Server on a custom domain/port.

1. On the WSO2 Identity Server management console (`https://<IS_HOST>:<PORT>/carbon`), go to **Main** > **Identity** > **Identity Providers**.
2. Click **Resident** and expand **Inbound Authentication Configuration**.
3. Expand **OAuth2 / OpenID Connect Configuration** and update **Identity Provider Entity ID** value to match with your custom domain/port.

    Eg: if you are running the WSO2 Identity Server on port 9500, the updated **Identity Provider Entity ID** value should be `https://localhost:9500/oauth2/token`.

4. Click **Update** to complete the configuration.

## Download the sample
Download the latest release of the [sample react application](https://github.com/asgardeo/asgardeo-auth-react-sdk/releases/download/v5.2.3/asgardeo-react-app.zip).

## Configure the sample
Change the `config.json` file found in the `asgardeo-react-app/src` sample folder with the relevant values.

- **clientID** - Add the client id of the registered application. The client ID can be copied from **Inbound Authentication Configuration > OAuth/OpenID Connect Configuration** section of your service provider.

- **baseUrl** - `https://localhost:9443`

- **scope** - The list of OIDC scopes that are used for requesting user information. The ``openid`` and the ``profile`` scopes are mandatory to get the ID token. You can add other OIDC scopes such as ``email``.

``` 
    {
        "clientID": "<client ID>",
        "baseUrl": "https://localhost:9443",
        "signInRedirectURL": "https://localhost:3000",
        "signOutRedirectURL": "https://localhost:3000",
        "scope": [ "openid", "profile" ]
    }
``` 

## Run the sample

Run the following command at the root of the project to start up the sample application. The app will be accessible at `https://localhost:3000`.

```
npm install && npm start
```

Log in to WSO2 Identity Server management console using your user account credentials.