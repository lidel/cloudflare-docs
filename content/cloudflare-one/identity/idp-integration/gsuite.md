---
pcx-content-type: how-to
title: Google Workspace
weight: 13
---

# Google Workspace

You can integrate a Google Workspace (formerly Google Suite) account with Cloudflare Access. Unlike the instructions for [generic Google authentication](/cloudflare-one/identity/idp-integration/google/), the steps below will allow you to pull group membership information from your Google Workspace account.

Once integrated, users will login with their Google Workspace credentials to reach resources protected by Cloudflare Access or to enroll their device into Cloudflare Gateway.

Please note that you don't need to be a Google Cloud Platform user to integrate Google Workspace as an identity provider with Cloudflare Zero Trust. You will only need to open the Google Cloud Platform to access settings for your OIDC identity provider.

1.  Log in to the Google Cloud Platform [console](https://console.cloud.google.com/). This is separate from your Google Workspace console.

    ![GCP Console](/cloudflare-one/static/documentation/identity/gsuite/gcp-home.png)

2.  Click **Create Project** to create a new project. Name the project and click **Create**. You should now see a Dashboard for your project.

    ![Post Create](/cloudflare-one/static/documentation/identity/gsuite/post-create.png)

3.  On the left-hand side, select `APIs & Services` and click **Dashboard**.

4.  In the screen that loads, click **+ Enable APIs and Services** in the top toolbar.

5.  The API Library will load. Search for `admin` in the search bar.

    ![API Library](/cloudflare-one/static/documentation/identity/gsuite/api-library.png)

6.  Select `Admin SDK API` by Google.

7.  Click **Enable** on the Admin SDK API page. The Admin SDK will be added to your project.

    ![Admin SDK](/cloudflare-one/static/documentation/identity/gsuite/post-enable.png)

8.  Return to the APIs & Services page. Click **Credentials** in the navigation bar. You will see a warning that you need to configure a consent screen. Click **Configure Consent Screen**.

    ![Configure Consent Screen](/cloudflare-one/static/documentation/identity/gsuite/configure-consent-screen.png)

9.  Cloudflare Access will gather information about users in your Google Workspace account, but not other accounts. Toggle `Internal` to limit this to members in your account.

    ![Internal Users](/cloudflare-one/static/documentation/identity/gsuite/consent-internal.png)

10. Input information about the application.

In this case, you are making an application available to your users and can add your team's contact information.

![Internal Users](/cloudflare-one/static/documentation/identity/gsuite/consent-screen-contact.png)

You will not need to configure scopes in this screen and can leave these fields blank.

![Consent Screen Scope](/cloudflare-one/static/documentation/identity/gsuite/consent-screen-scope.png)

The summary page will load and you can save and exit.

![Consent Screen Summary](/cloudflare-one/static/documentation/identity/gsuite/consent-screen-summary.png)

11. Return to the **Credentials** page. Click **+ Create Credentials**

    ![Create Credentials](/cloudflare-one/static/documentation/identity/gsuite/create-credentials.png)

12. Select **OAuth client ID**.

    ![Select OAuth](/cloudflare-one/static/documentation/identity/gsuite/select-oauth.png)

13. Select `Web application` as the Application type.

14. Under **Authorized JavaScript origins**, in the **URIs** field, enter your [team domain](/cloudflare-one/glossary/#team-domain).

15. Under **Authorized redirect URIs**, in the **URIs** field, enter your team domain followed by this callback at the end of the path: `/cdn-cgi/access/callback`. For example:

```txt
https://<your-team-name>.cloudflareaccess.com/cdn-cgi/access/callback
```

![Input Team Domain](/cloudflare-one/static/documentation/identity/gsuite/input-auth-domain.png)

    Click **Create**.

16. Google will present the OAuth Client ID and Secret values. The secret field functions like a password and should be kept securely and not shared. For the purposes of this tutorial, the secret field is kept visible. Copy both values.

    ![Secret Field](/cloudflare-one/static/documentation/identity/gsuite/secret-field.png)

    The Client ID will now appear in the `APIs & Services` page.

    ![Client ID Visible](/cloudflare-one/static/documentation/identity/gsuite/client-id-visible.png)

17. On the Zero Trust dashboard, navigate to **Settings > Authentication**.

18. Under **Login methods**, click **Add new**.

19. Select **Google Workspace**.

20. Input the Client ID and Client Secret fields generated previously. Additionally, input the domain of your Google Workspace account.

21. (Optional) Enable [Proof of Key Exchange (PKCE)](https://www.oauth.com/oauth2-servers/pkce/). PKCE will be performed on all login attempts.

22. Click **Save**.

23. To complete setup, you must scroll below and visit the link generated. If you are not the Google Workspace administrator, share the link with the administrator.

  {{<Aside type="note">}}
  
  For the next steps to work correctly, you must enable the **Trust internal, domain-owned apps** option under the **Security** > **Access and data control** > **API controls** section of the Google Admin web app. 
  
  ![Enable trust internal apps](/cloudflare-one/static/documentation/identity/gsuite/trust-internal-apps.png)
  
  The option is disabled by default and must be enabled for Cloudflare Access to work correctly.

  {{</Aside>}}

24. The generated link will prompt you to login to your Google account and to authorize Cloudflare Access to view group information.

    ![Authorize Groups](/cloudflare-one/static/documentation/identity/gsuite/authorize-groups.png)

    A success page will then load from Cloudflare Access.

    ![Group Success](/cloudflare-one/static/documentation/identity/gsuite/group-success.png)

25. You can now return to the list of identity providers in the **Authentication** page of the Cloudflare Zero Trust dashboard. Select Google Workspace and click **Test**.

    Your user identity and group membership should return.

    ![Connection Works](/cloudflare-one/static/documentation/identity/gsuite/connection-works.png)

## Example API Configuration

```json
{
  "config": {
    "client_id": "<your client id>",
    "client_secret": "<your client secret>",
    "apps_domain": "mycompany.com"
  },
  "type": "google-apps",
  "name": "my example idp"
}
```
