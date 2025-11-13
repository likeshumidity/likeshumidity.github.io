# How This Website Was Built

This document outlines the setup for the blog at `blog.likeshumidity.com`. It uses Hugo for static site generation, the PaperMod theme, and is automatically deployed to GitHub Pages.

### Completed Setup Steps

The following steps have already been completed to establish the foundation of this blog.

1.  **Hugo Site Creation:**
    The Hugo site was created in the `blog.likeshumidity.com` directory.

2.  **Git Repository:**
    A Git repository was initialized, and the code was pushed to the `likeshumidity/likeshumidity.github.io` repository on GitHub.

3.  **PaperMod Theme Installation:**
    The PaperMod theme was installed as a Git submodule:
    ```bash
    git submodule add https://github.com/adityatelange/hugo-PaperMod.git themes/PaperMod
    ```

4.  **Hugo Configuration (`hugo.toml`):**
    The site is configured with the following settings. The `baseURL` is set to the custom domain.
    ```toml
    baseURL = "https://blog.likeshumidity.com/"
    title = "likesHumdity Blog"
    theme = "PaperMod"

    [params]
      defaultTheme = "auto" # auto, dark, light
      ShowPostNavLinks = true
      ShowCodeCopyButtons = true

    [menu]
      [[menu.main]]
        identifier = "posts"
        name = "Posts"
        url = "/posts/"
        weight = 1
      [[menu.main]]
        identifier = "about"
        name = "About"
        url = "/about/"
        weight = 2
    ```

5.  **Auto-Publishing with GitHub Actions:**
    A GitHub Actions workflow was created at `.github/workflows/deploy.yml`. This action automatically builds the Hugo site and deploys it to the `gh-pages` branch upon every push to the `main` branch.

### Remaining User Actions

To make the site fully operational with your custom domain, you need to complete these final steps.

1.  **Set Up Your Domain (DNS):**
    Go to your domain registrar (the site where you bought `likeshumidity.com`) and add the following DNS record:

    *   **Type:** `CNAME`
    *   **Host (or Name):** `blog`
    *   **Value (or Target):** `likeshumidity.github.io.`
        *   **Note:** The period `.` at the end is important. Some registrars require it.

    > DNS changes can take a few minutes to a few hours to propagate.

2.  **Link Your Domain in GitHub:**
    *   Go to the repository settings for `likeshumidity/likeshumidity.github.io`.
    *   Navigate to the **Pages** section.
    *   Under "Build and deployment," set the **Source** to **"GitHub Actions"**.
    *   In the **"Custom domain"** section, enter `blog.likeshumidity.com` and click **Save**.
    *   Once GitHub verifies your DNS settings, check the box for **"Enforce HTTPS"**.

Your setup is now complete. Your site will be live at `https://blog.likeshumidity.com` and will update automatically whenever you push changes to the `main` branch.

---

### How to Write and Publish a New Post

This is your repeatable process for each article.

1.  **Create New Post:**
    From the `blog.likeshumidity.com` directory, run:
    ```bash
    # Example: creates content/posts/my-new-post.md
    hugo new "posts/my-new-post.md"
    ```

2.  **Write Content:**
    Open the newly created Markdown file (e.g., `content/posts/my-new-post.md`) in your favorite editor and write your post.

3.  **Preview Locally:**
    To see a live preview of your site, run:
    ```bash
    hugo server -D
    ```
    Then open `http://localhost:1313` in your browser. The `-D` flag builds draft posts.

4.  **Edit & Review in GDocs (Optional):**
    *   Copy the Markdown from your editor and paste it into a new Google Doc.
    *   Use Google Docs for collaborative editing, commenting, and sharing.

5.  **Convert Back to Markdown (If Applicable):**
    *   Use the **"Docs to Markdown"** add-on in Google Docs or an external tool like **Pandoc** to convert the final content back to Markdown.
    *   Paste the clean Markdown back into your `.md` file.

6.  **Finalize & Publish:**
    When you are ready to publish, change `draft: true` to `draft: false` in the front matter (the `---` block) of your `.md` file.

7.  **Push to GitHub:**
    Commit your changes and push them to the `main` branch.
    ```bash
    git add .
    git commit -m "Publish: My New Post Title"
    git push origin main
    ```
    The GitHub Action will automatically build and deploy your new post.
