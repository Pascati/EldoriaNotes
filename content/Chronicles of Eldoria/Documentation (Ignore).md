# Pushing Updates
> [!INFO] I successfully moved everything to the 'quartz/content' folder! Huzzah!! So skip ahead to Step 3

1. Delete all files in the following folder:
>	C:\Windows\System32\quartz\content

2. Copy/paste all obsidian files to the following path:
>	C:\Windows\System32\quartz\content

3. Run the following code in Windows Powershell (always run as Administrator). **Run lines separately**
>	cd quartz
>	npx quartz sync
- If Github asks for a password:
	- Refer to this thread/article (create a new token in github developer settings):
		- https://github.com/orgs/community/discussions/29193
		- [Managing Your Personal Access Tokens](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens)
	- Regenerate PAT (personal access token) through Github -> Settings -> Developer Settings -> Personal Access Tokens (classic)
		- Consider creating a permanent token if this issue comes up again
		- See [.gitconfig] file (C:\Users\Phant) for authorization logs

## JUST IN CASE
Cloudflare link: https://dash.cloudflare.com/bf1eb94bee741869f5fdbdc16a96ed04/pages/view/eldorianotes (probably should log in first)


# Configuration/Customization
>[!info] Actual instructions and themes can be found [here](https://github.com/saberzero1/quartz-themes)!

## Theme Notes
- **THEME USED:** anuppuccin
- **THEME COLOR SCHEME:** [catppuccin](https://github.com/catppuccin/catppuccin)
- ***NOTE:*** I ended up downloading it manually 💀 but I'm keeping the fancy instructions JUST IN CASE (for instructions on manual install, go to the link above and CTRL+F search for "Manual install")


1. Go to the following folder and open *deploy.yml* in Notepad (run as administrator):
> [!info] Remove the extra backslash before '.github'!
>	C:\Windows\System32\quartz\\.github\workflows

2. Add the following before the *permissions* section:
>	env:
>		 THEME_NAME: \<THEME-NAME\>

3. Add the following text in the *build* step:
>	- name: Fetch Quartz Theme
>	   run: curl -s -S https://raw.githubusercontent.com/saberzero1/quartz-themes/master/action.sh | bash -s -- $THEME_NAME

**NOTE:** changed *action.sh* to *action.bat*, since it was recommended that Windows users make the switch

*deploy.yml* should look like this:
```
name: Deploy Quartz site to GitHub Pages

on:
  push:
    branches:
      - v4

env:
  THEME_NAME: <THEME-NAME>

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: "pages"
  cancel-in-progress: false

jobs:
  build:
    runs-on: ubuntu-22.04
    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0 # Fetch all history for git info
      - uses: actions/setup-node@v4
        with:
          node-version: 22
      - name: Install Dependencies
        run: npm ci
      - name: Fetch Quartz Theme
        run: curl -s -S https://raw.githubusercontent.com/saberzero1/quartz-themes/master/action.sh | bash -s -- $THEME_NAME
      - name: Build Quartz
        run: npx quartz build
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: public

  deploy:
    needs: build
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

# Style Settings Plugin
> [!INFO] In case of a reset, copy-paste the below settings into the style settings plugin!

```
{
  "anuppuccin-theme-settings@@anuppuccin-theme-accents": "ctp-accent-pink",
  "anuppuccin-theme-settings@@anp-header-color-toggle": true,
  "anuppuccin-theme-settings@@anp-alt-rainbow-style": "anp-simple-rainbow-color-toggle",
  "anuppuccin-theme-settings@@anp-simple-rainbow-title-toggle": true,
  "anuppuccin-theme-settings@@anp-simple-rainbow-collapse-icon-toggle": true,
  "anuppuccin-theme-settings@@anp-rainbow-subfolder-color-toggle": true,
  "anuppuccin-theme-settings@@anp-colorful-frame": false,
  "anuppuccin-theme-settings@@anp-canvas-dark-bg": false,
  "anuppuccin-theme-settings@@anp-list-toggle": false,
  "anuppuccin-theme-settings@@anp-custom-checkboxes": false,
  "anuppuccin-theme-settings@@anp-speech-bubble": false,
  "anuppuccin-theme-settings@@anp-h1-color-custom": "anp-h1-lavender",
  "anuppuccin-theme-settings@@anp-h2-color-custom": "anp-h2-blue",
  "anuppuccin-theme-settings@@anp-h3-color-custom": "anp-h3-sky",
  "anuppuccin-theme-settings@@anp-h5-color-custom": "anp-h5-green",
  "anuppuccin-theme-settings@@anp-header-margin-value": 12,
  "anuppuccin-theme-settings@@anp-header-divider-color-toggle": true,
  "anuppuccin-theme-settings@@bold-weight": "700",
  "anuppuccin-theme-settings@@anp-italic-custom": "anp-italic-blue",
  "anuppuccin-theme-settings@@anp-decoration-toggle": true
}
```