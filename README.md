This is the final and most rewarding piece of the setup. Let's integrate this directly into your plan.

Here is the complete, updated plan to build your blog with Hugo, publish to GitHub Pages, and use your custom domain, `blog.likeshumidity.com`.

### Plan: The Hugo + GDocs Workflow (with Custom Domain)

#### 1\. One-Time Setup (The 40-Minute Foundation)

You will do these steps only once.

1.  **Install Hugo:**

      * **macOS (Homebrew):** `brew install hugo`
      * **Windows (Scoop):** `scoop install hugo-extended`

2.  **Create Your Site & Git Repo:**

    ```bash
    hugo new site my-blog
    cd my-blog
    git init
    ```

3.  **Install the PaperMod Theme:**

    ```bash
    git submodule add https://github.com/adityatelange/hugo-PaperMod.git themes/PaperMod
    ```

4.  **Configure Hugo for Your Domain:** This is a **critical** new step.

      * Open the `hugo.toml` file at the root of your `my-blog` folder.
      * Delete everything in it and replace it with this (adjust the `title`):

    <!-- end list -->

    ```toml
    # This is your new hugo.toml file

    baseURL = "https://blog.likeshumidity.com/"
    title = "My Blog's Awesome Title"
    theme = "PaperMod"

    [params]
      # This is for the PaperMod theme
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

      * **Why?** Setting the `baseURL` is essential. Hugo will use this to generate all your links (e.g., `https://blog.likeshumidity.com/posts/my-post/` instead of a broken local link).

5.  **Set Up Your Domain (DNS):** Now, let's go to your domain registrar (the site where you *bought* `likeshumidity.com`, like GoDaddy, Namecheap, Google Domains, etc.).

      * Find the **DNS Management** settings for your domain.
      * You need to add **one new record**.
      * **Type:** `CNAME`
      * **Host (or Name):** `blog` (Your registrar might automatically add the `.likeshumidity.com` part)
      * **Value (or Target):** `[your-username].github.io.`
          * **Important:** Replace `[your-username]` with your *actual* GitHub username.
          * **Crucial:** Note the period `.` at the very end. Some registrars require it; some don't. It's safest to add it.

    > **Note:** DNS changes can take a few minutes or (in rare cases) up to 24 hours to "propagate" across the internet.

6.  **Create GitHub Repo & Push:**

      * Go to GitHub. Create a new, public repository named **`[your-username].github.io`**.
      * Back in your terminal, link this new repo to your local folder and push your site for the first time:

    <!-- end list -->

    ```bash
    # Make sure you've saved your hugo.toml changes
    git add .
    git commit -m "Initial site setup and theme"

    # Run these lines from GitHub's "push an existing repository" instructions
    git remote add origin https://github.com/[your-username]/[your-username].github.io.git
    git branch -M main
    git push -u origin main
    ```

7.  **Add the Auto-Publish Action:**

      * In your `my-blog` folder, create the directory path: `.github/workflows/`
      * Inside that `workflows` folder, create a file named `deploy.yml`
      * Copy and paste the *entire* block of code below into `deploy.yml`:

    <!-- end list -->

    ```yaml
    name: Deploy Blog to GitHub Pages

    on:
      push:
        branches:
          - main
      workflow_dispatch:

    permissions:
      contents: read
      pages: write
      id-token: write

    jobs:
      build:
        runs-on: ubuntu-latest
        steps:
          - name: Checkout
            uses: actions/checkout @v4
            with:
              submodules: true  # Fetches your theme
              fetch-depth: 0

          - name: Setup Hugo
            uses: peaceiris/actions-hugo @v3
            with:
              hugo-version: 'latest'
              extended: true

          - name: Build
            run: hugo --minify
            
          - name: Upload Artifact
            uses: actions/upload-pages-artifact @v3
            with:
              path: ./public

      deploy:
        needs: build
        runs-on: ubuntu-latest
        environment:
          name: github-pages
          url: ${{ steps.deployment.outputs.page_url }}
        steps:
          - name: Deploy to GitHub Pages
            id: deployment
            uses: actions/deploy-pages @v4
    ```

8.  **Link Your Domain in GitHub (The Final Step):**

      * Push your new `deploy.yml` file to GitHub:

        ```bash
        git add .github/workflows/deploy.yml
        git commit -m "Add GitHub Actions deploy workflow"
        git push origin main
        ```

      * Go to your new **GitHub repo's Settings \> Pages**.

      * Under "Build and deployment," set the **Source** to **"GitHub Actions"**.

      * Now, find the **"Custom domain"** section.

      * Type `blog.likeshumidity.com` into the box and click **Save**.

      * GitHub will check your DNS. If it finds the `CNAME` record you set up in Step 5, it will show a green checkmark.

      * **Crucially, check the box for "Enforce HTTPS."** This gives you the free SSL certificate.

Your one-time setup is **100% complete**. Your site is now live at `https://blog.likeshumidity.com` and will auto-publish on every `git push`.

-----

#### 2\. Your Hugo-Specific Writing Workflow (Per Post)

This is your repeatable process for each article.

1.  **Create New Post (CLI):**

    ```bash
    # This creates: content/posts/my-new-post.md
    hugo new "posts/my-new-post.md"
    ```

2.  **Draft / AI Assist:**

      * Open `content/posts/my-new-post.md` in Obsidian or VIM.
      * Write your notes/draft *below* the `---` (front matter) block.

3.  **Local Preview:**

    ```bash
    hugo server -D
    ```

      * Open `http://localhost:1313` in your browser to see a live preview.

4.  **Edit & Review in GDocs:**

      * Copy Markdown from VIM/Obsidian -\> Paste into a new Google Doc.
      * Edit, collaborate, and share.

5.  **Convert Back to Markdown:**

      * Use the **"Docs to Markdown"** add-on (UI) or **Pandoc** (CLI).
      * Paste the final, clean Markdown back into your `.md` file, replacing the draft.

6.  **Finalize & Publish:**

      * In the front matter (the `---` block) of your `.md` file, change `draft: true` to **`draft: false`**.
      * Save the file.

7.  **Push to GitHub:**

    ```bash
    git add .
    git commit -m "Publish: My New Post"
    git push origin main
    ```

Your GitHub Action will now automatically take over, build your site with the new post, and publish it to `https://blog.likeshumidity.com`.

Would you like me to show you how to create your first post using the PaperMod theme, including how to add an image and a code block?