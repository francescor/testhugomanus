# Instructions for Managing Your Hugo Blog (mysecond.website)

This document provides instructions for running, maintaining, and configuring your Hugo blog hosted on GitHub Pages.

## Project Overview

*   **Framework:** Hugo (Static Site Generator)
*   **Theme:** Ananke
*   **Hosting:** GitHub Pages
*   **Deployment:** Automated via GitHub Actions (`.github/workflows/deploy.yml`)
*   **Content:** Markdown files located in `content/en/` (English) and `content/it/` (Italian).
*   **Configuration:** Main settings are in `config.toml`.

## Adding/Editing Content

1.  **Navigate to the content directory:** Go into either `content/en/` for English posts or `content/it/` for Italian posts.
2.  **Create a new Markdown file:** You can create a new file (e.g., `my-new-post.md`) or use the Hugo command:
    ```bash
    # Example for an English post
    hugo new posts/my-new-post.md
    # Example for an Italian post (assuming you are in the project root)
    hugo new it/posts/mio-nuovo-post.md
    ```
    *(Note: You might need to create the `posts` subdirectory first if it doesn't exist.)*
3.  **Edit the file:** Add your content in Markdown format. Ensure the front matter (the section between `---` at the top) includes at least a `title`.
    ```markdown
    ---
    title: "My Awesome New Post"
    date: 2025-05-01T10:00:00Z
    draft: false # Set to false to publish
    ---

    This is the content of my post...
    ```
4.  **Commit and Push:** Save your changes, commit them to Git, and push to the `main` branch on GitHub. This will automatically trigger the deployment workflow.

## Local Development

To preview your site locally before pushing changes:

1.  **Install Hugo:** Make sure you have Hugo (extended version recommended) installed on your computer. See [Hugo Installation Guide](https://gohugo.io/getting-started/installing/).
2.  **Install Node.js:** The Ananke theme requires Node.js for its assets. Install Node.js if you haven't already.
3.  **Install Theme Dependencies:** Navigate to the theme directory and install dependencies:
    ```bash
    cd themes/ananke
    npm install package-lock.json
    cd ../.. # Go back to project root
    ```
4.  **Install and Run the Hugo server:**
    ```bash
    sudo dnf install hugo
    hugo server -D
    ```
    The `-D` flag builds draft posts as well.
5.  **Open your browser:** Go to `http://localhost:1313/` (or the address shown in the terminal) to view your site.

## Deployment

Deployment is handled automatically by the GitHub Actions workflow in `.github/workflows/deploy.yml`. Simply push your changes (commits) to the `main` branch of your GitHub repository (`francescor/testhugomanus`). The workflow will build the site and deploy it to GitHub Pages.

You can monitor the workflow status in the **Actions** tab of your repository.

## Configuration (`config.toml`)

*   **`baseURL`:** This is crucial. It should match the URL where your site is served.
    *   For the default GitHub Pages URL: `baseURL = "https://francescor.github.io/testhugomanus/"`
    *   If you set up the custom domain `mysecond.website`: `baseURL = "https://mysecond.website/"`
*   **`[params]`:** Contains theme-specific parameters like the author name (`author = "Masaccio"`).
*   **`[languages]`:** Defines the English (`en`) and Italian (`it`) versions of the site, including their titles, content directories, and menus.
*   **`[[languages.en.menu.main]]` / `[[languages.it.menu.main]]`:** Defines the items in the main navigation menu for each language.

## Feature Configuration

These features require manual setup:

1.  **Newsletter (`content/en/newsletter.md`, `content/it/newsletter.md`):**
    *   Sign up for a form backend service like [Formspree](https://formspree.io/).
    *   Get your unique form endpoint URL.
    *   Replace `YOUR_FORMSPREE_ENDPOINT` in both newsletter Markdown files with your actual URL.
2.  **Contact Page (`content/en/contact.md`, `content/it/contact.md`):**
    *   Replace the placeholder link `https://t.me/your_channel_placeholder` with the actual URL of your Telegram channel.
3.  **News Feed (`content/en/news.md`, `content/it/news.md`):**
    *   This requires integration with the Telegram API.
    *   You'll likely need a Telegram Bot token and your channel ID.
    *   Implement a script (e.g., using Python or JavaScript, potentially run during the Hugo build via GitHub Actions or fetched client-side) to get messages from your Telegram channel and display them on these pages.
4.  **AI Content Suggestions (`.github/workflows/ai_suggestions.yml`):**
    *   This workflow provides a framework but needs implementation.
    *   Write a script (e.g., Python) that:
        *   Reads changed Markdown files.
        *   Calls an AI API (e.g., OpenAI, Gemini) with the content to get suggestions or translations.
        *   Uses the GitHub CLI (`gh`) or API to post the results as comments on commits or pull requests.
    *   Store your AI API key securely as a secret in your GitHub repository settings (e.g., `AI_API_KEY`).
    *   Update the workflow file to run your script, potentially installing necessary tools like Python and the GitHub CLI.

## Custom Domain (`mysecond.website`)

To use `mysecond.website`:

1.  **Configure DNS:** Point your domain's DNS records (usually A records or CNAME) to GitHub Pages according to their documentation: [Configuring a custom domain for your GitHub Pages site](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site).
2.  **Update GitHub Pages Settings:** In your repository's **Settings** -> **Pages**, enter `mysecond.website` in the **Custom domain** field and save.
3.  **Update `baseURL`:** Change the `baseURL` in your `config.toml` file back to `baseURL = "https://mysecond.website/"`.
4.  **Commit and Push:** Push the `config.toml` change to GitHub to trigger a redeployment with the correct base URL for the custom domain.

As for DNS: These are GitHub Pages' IP addresses. You can verify the current IPs in GitHub's documentation: [Managing a custom domain for your GitHub Pages site](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site#configuring-an-apex-domain).
```
Add the following four A records:

    Host: @, Value: 185.199.108.153, TTL: Automatic (or 30 min)
    Host: @, Value: 185.199.109.153, TTL: Automatic (or 30 min)
    Host: @, Value: 185.199.110.153, TTL: Automatic (or 30 min)
    Host: @, Value: 185.199.111.153, TTL: Automatic (or 30 min)
```


Keep this file for future reference. Good luck with your blog!
