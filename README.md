<p align="center">
  <img src="https://github.com/globus/template-data-portal/assets/694253/8c55d1e3-1ecd-47ea-8288-73c79cd2c953" height="100px" alt="Globus" />
   <br/>
  <strong>🧪 BETA 🧪</strong>
  <br/>
  <strong>This is template repository used to generate a Globus-powered research data portal.</strong>
  <br/>
</p>

----

View the result at: [globus.github.io/template-data-portal](https://globus.github.io/template-data-portal).

While this repository is a working example of a data portal, it is also a template for [creating your own static research data portal](#creating-your-own-static-research-data-portal).

----

# Features + Functionality

## Data Portal

- **Powered by [Globus](https://www.globus.org/)**
- List the directory contents of a configured Globus collection at a specific path.
  - Support for [HTTPS Access](https://docs.globus.org/globus-connect-server/v5.4/https-access-collections/) download of files where available.
- Transfer data from the configured collection to another Globus collection.
- Configure multiple collections and paths allowing users to select their preferred source.
- Ability to host custom content alongside the data portal.

## GitHub Repository

- 📄 **Hosted via GitHub Pages** – Users can access your data portal at this repository's GitHub Pages URL. Use all the functionality built-in to GitHub pages to suit your needs, including [configuring a custom domain](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/about-custom-domains-and-github-pages).

- 🚀 **Automated Deployments via GitHub Actions** – Any file changes will result in the deployment (and rebuild) of your data portal.
   - You can manually trigger a deployment by navigating to the **Actions** tab and selecting the **static** workflow.
   
- 🤖 **Dependabot** – A default [Dependabot](https://docs.github.com/en/code-security/dependabot) configuration ([`.github/dependabot.yml`](.github/dependabot.yml)) to keep your repository up-to-date with latest changes to [globus/static-data-portal](https://github.com/globus/static-data-portal).


----

### Creating Your Own Research Data Portal

1. Create a new repository from the [globus/template-data-portal](https://github.com/globus/template-data-portal) template.
   * Using the following URL: https://github.com/new?template_name=template-data-portal&template_owner=globus
   * Or, using the GitHub UI: <img width="188" alt="Screenshot 2024-03-11 at 12 24 22 PM" src="https://github.com/globus/template-data-portal/assets/694253/abffa5a5-86c8-47d9-be4b-f249d34505ab">
1. [Update your repository to allow publishing with GitHub Actions](https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site#publishing-with-a-custom-github-actions-workflow).
   - **IMPORTANT** The built-in GitHub Action workflows in your new repository will fail until you've updated this setting.
1. [Ensure your GitHub Pages are configured to Enforce HTTPS](https://docs.github.com/en/pages/getting-started-with-github-pages/securing-your-github-pages-site-with-https)
1. Register an application on Globus – https://app.globus.org/settings/developers
   * You'll be creating an OAuth public client; This option is presented as _"Register a thick client or script that will be installed and run by users on their devices"_.
   * Update the **Redirects** to include your GitHub Pages URL + `/authenticate`, i.e., `https://{username}.github.io/{repository}/authenticate`
     * If you have [configured your GitHub Pages to use a custom domain](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site), this will be `https://{domain}/authenticate`
     * It is important to note that Globus Auth **requires HTTPS**.
   * Optional: Specify the **Privacy Policy URL** and **Terms & Conditions URL** to the portal-provided routes, i.e. `https://globus.github.io/example-data-portal/privacy-policy`
   * All other fields can be left as default or changed to suit your needs.
1. Update the `static.json` to include:
   * `data.attributes.globus.application.client_id` – The UUID of the client created in **the previous step**.
   * `data.attributes.globus.transfer.collection_id` – The Collection UUID that will be the "source" of your data portal.
   * Optional: Set the `data.attributes.globus.transfer.path` to a specific path within the collection, or remove the `path` property to resolve to the default directory.
   * See the [static.json](#staticjson) type definitions for more configuration options.
1. **That's it!** Your portal will be available at your GitHub Pages URL. The changes made (and any future changes) to the `static.json` will trigger a GitHub Action that will automatically build and deploy your research data portal to your GitHub Pages URL.

### Maintaining Your Data Portal

- As you make changes to files in your repository, the GitHub Action will automatically rebuild and deploy your portal.
- Globus will provide updates to the underlying `@globus/static-data-portal` package that powers your portal. Merging pull requests automatically opened from Dependabot will keep your portal up-to-date with the latest changes.

#### Common Changes after Creating Your Portal

- **Updating the Title** – Update the `data.attributes.content.title` in the `static.json`.
- **Adding Custom Content** - Add a `content` directory with Markdown, or [MDX](https://mdxjs.com/) files to be rendered as pages in your portal.
- **Edit/Remove the `CITATION` file** – Update the [`CITATION.cff`](CITATION.cff) file to reflect the appropriate citation information for your research data portal – [learn more about `CITATION` files](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/about-citation-files).
- **Removing this section of the README** – Remove this section from the `README.md` file or update the README to meet your needs.

----

### `static.json`

The type used for `data` by the [@globus/static-data-portal generator](https://github.com/globus/static-data-portal).

#### Type declaration

| Name | Type | Description |
| :------ | :------ | :------ |
| `attributes` | \{ `content`: \{ `image?`: `string` ; `privacy_policy?`: `string` ; `tagline?`: `string` ; `terms_of_service?`: `string` ; `title`: `string`  } ; `globus`: \{ `application`: \{ `client_id`: `string` ; `redirect_uri?`: `string`  } ; `transfer`: \{ `collection_id`: `string` ; `path?`: `string`  }  } ; `theme?`: `ThemeSettings`  } | - |
| `attributes.content` | \{ `image?`: `string` ; `privacy_policy?`: `string` ; `tagline?`: `string` ; `terms_of_service?`: `string` ; `title`: `string`  } | - |
| `attributes.content.image?` | `string` | The URL of the portal's header image. |
| `attributes.content.privacy_policy?` | `string` | A privacy policy to be rendered at `/privacy-policy`. This is especially useful for associating the published URL with your registered Globus Auth application. |
| `attributes.content.tagline?` | `string` | - |
| `attributes.content.terms_of_service?` | `string` | Terms and conditions to be rendered at `/terms-and-conditions`. This is especially useful for associating the published URL with your registered Globus Auth application. |
| `attributes.content.title` | `string` | The title of the research data portal. |
| `attributes.globus` | \{ `application`: \{ `client_id`: `string` ; `redirect_uri?`: `string`  } ; `transfer`: \{ `collection_id`: `string` ; `path?`: `string`  }  } | - |
| `attributes.globus.application` | \{ `client_id`: `string` ; `redirect_uri?`: `string`  } | Information about your registered Globus Auth Application (Client) **`See`** https://docs.globus.org/api/auth/developer-guide/#developing-apps |
| `attributes.globus.application.client_id` | `string` | The UUID of the client application. |
| `attributes.globus.application.redirect_uri?` | `string` | The redirect URI for the Globus Auth login page to complete the OAuth2 flow. The portal will make a reasonable effort to determine this URI, but this field is provided as a fallback. To use the portal's built-in authorization handling, redirects should be sent to `/authenticate` on the host. **`Example`** ```ts "https://example.com/data-portal/authenticate" ``` |
| `attributes.globus.transfer` | \{ `collection_id`: `string` ; `path?`: `string`  } | Configuration for Transfer-related functionality in the portal. |
| `attributes.globus.transfer.collection_id` | `string` | The UUID of the Globus collection to list and transfer files from. |
| `attributes.globus.transfer.path?` | `string` | The path on the collection to list and transfer files from. |
| `attributes.theme?` | `ThemeSettings` | - |
| `version` | `string` | The version of the `data` object, which is used to determine how the generator will render its `attributes`. **`Example`** ```ts "1.0.0" ``` |
