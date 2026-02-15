# Basic use of Obsidian for content management with separate `git` application
The best-supported way of using *Obsidian* with *git* and *Obsidian Publish* is when *git* is a separate application.

## Pre-requisites. 
You’ll need to ensure all these are in place. But you’ll only have to do them once:-)

### Obsidian
You need to have *Obsidian* installed on your machine. 
- See [obsidian.md](https://obsidian.md/download) for details.
- You’ll also need to register [an Obsidian id](https://obsidian.md/auth) with your email address 
- And arrange to have this account authorised to publish to our Wiki:
    - Notify the HHS Obsidian administrator of your address
    - They will send you an invitation
    - Accept that invitation
### Git
You need to have *Git* installed on your machine
- It can be either on the command line or you can use a *Git* GUI tool such as:
    - [*GitHub desktop*](https://github.com/desktop/desktop?tab=readme-ov-file#where-can-i-get-it)
    - [*Sourcetree*](https://www.sourcetreeapp.com/), 
    - [*Visual Studio*](https://learn.microsoft.com/en-us/visualstudio/version-control/git-with-visual-studio?view=visualstudio)
    - or one of the many others.
- The [*Git* website](https://git-scm.com/) has links for installation, tools and the CLI reference.
### GitHub account
If you don’t already have a *GitHub* account, that needs to be set up first.
- Go to [github.com/signup](https://github.com/signup) and follow the prompts.
- Note that the messiest bit is setting up your authentication. 
	- *GitHub* has strong protections against fraudulent submissions to their repository. 
	- Fortunately it’s a one-off process. 
### The repository
You’ll need to connect to and clone the repository
- The repository is at [github.com/hobart-hackerspace/wiki-obsidian](https://github.com/hobart-hackerspace/wiki-obsidian)
- The repository is a private one (to protect our intellectual property), so you’ll need to be added to the Hackerspace Github group. 
    - Send a note to github@hobarthackerspace.org.au asking to be added as a user to the members list.
- Once you have access, make a local copy of the repository by cloning it.
### The repository branches
We use two shared branches, `draft` and `publish`. In addition, we recommend that you set up your own named branch to make contributions.
- `Publish` is the stable version of the content and normally published.
- `Draft` is a branch to which you can apply changes for others to comment on or edit. 
    - Publish from this branch to see your changes as web pages, but when you’re done, ensure that the final state of the published pages matches the `published` branch.
- `Your-branch` can be a workspace for you to try things, especially if you don’t know how they will be rendered into HTML pages.
### Your Obsidian vault
You need to make the cloned directory into an *Obsidian* vault.
- Open *Obsidian* and go to “Manage Vaults” 
    - See the [relevant Obsidian Help](https://help.obsidian.md/vault) for details
- You may choose whatever name you like for the vault - it will be local to you.

## Making contributions
### Work in your local copy of the repository

1. Use your favourite `git` tool (command line, or GUI tool).
2. Checkout the `draft` branch and `pull` from the repository to ensure your copy is fully up to date.
3. Make a new personal branch from this and check it out.
    - If you already have a personal branch, merge the current state of `draft` into it.

### Open the vault in *Obsidian*

*Obsidian* will automatically work with whichever branch you have checked out.
1. Ensure that you’re [logged in to *Obsidian*](https://help.obsidian.md/publish/setup).
2. Make your contributions (additions and/or changes).
    - Note that Obsidian has two ways of displaying pages: reading and editing. Within editing mode there are a further two modes:  “Live preview” or “Source mode” (settable from the “Options” gear wheel icon).
	    - “Live preview” is a WSIWYG editor similar to Word. Use the right-cllick options menu to format, add links, etc.
	    - “Source mode” is what it says — you see the whole page as Markdown source. Use that if you’re familiar with Markdown.
    - The [*Obsidian* Formatting Help pages](https://help.obsidian.md/syntax) have lots of advice on how to format your contributions.
    - Use the other existing pages as models for consistent page structure, image formatting, table formatting, source code contributions etc.

### Publish your drafts to see how they look
When you’re happy with your contributions:
1. [Publish them](https://help.obsidian.md/publish/setup#Open+Publish)
	- If you have any pages that have attachments (for example images or attached PDFs), ensure that you click on `Add linked` at the top of the **`Publish`** panel, so that the attachments get published along with their host pages.
2. Check the page formatting on [the wiki website](https://publish.obsidian.md/hhs-wiki/Hackerspace+Wiki)
3. Make any adjustments and re-publish 
4. Refresh the page to see the new content. You may have to do a [“hard refresh”]( https://www.wikihow.com/Force-Refresh-in-Your-Internet-Browser) to clear the browser cache.
	- Note that some part of the publish/browser cache combination seems to have limits on how frequently you can refresh a page. Sometimes if a page takes a few goes to get the published details right, it won’t change when viewed in your browser, even if you do a hard refresh. Try a different browser or take a coffee break.
	
### Check your changes in
If that’s all ok, check the changes into your branch
1. Please use appropriate useful comments. 
    - If you’ve done a range of alterations, feel free to check in batches of changes with different comments for each batch . 
2. If you made some experimental changes that you don’t want to keep, revert them, so that ‘git status’ shows nothing outstanding. 
3. Push your committed changes to the GitHub repository. 
4. Check out the ‘draft’ branch and merge it with your just-committed personal branch. 
5. Push that, too up to GitHub

### Have someone else proof them
It’s always good practice to have someone else look over any changes.
- Leave your new changes in place on the published site for the to do this.
- We all occasionally have slips of the finger or the mind
- There’s no loss of face in finding these early!!

### Finally, merge the new content into the `published` branch
When you’re finally happy, check out the `published` branch, merge the `draft` branch into it and push that back up to GitHub.

Then the entire cycle can start again…
