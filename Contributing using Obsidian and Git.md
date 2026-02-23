# Note that this is a work in progress
All offers of assistance (even just to test, especially on Android pads or phones) gratefully received.

*BWM 2026-02-23*

# Pre-requisites. 
You’ll need to ensure all the [Wiki contribution pre-requisites](Wiki%20contribution%20pre-requisites.md) are in place. But you’ll only have to do them once:-)

# The repository
You’ll need to connect to and clone the repository
- The repository is at [github.com/hobart-hackerspace/wiki-obsidian](https://github.com/hobart-hackerspace/wiki-obsidian)
- Make a local copy of the repository by cloning it. 
	- On a laptop or desktop, use your chosen `git` tool to clone the repository into a workspace directory.
	- 
- If you want to be able to push your contributions directly to the repository,  you’ll need to be added to the Hackerspace Github group. 
    - Send a note to github@hobarthackerspace.org.au asking to be added as a user to the members list.
    - Alternatively, you can suggest changes by creating a “Pull” request.
## Repository branches
If you’re making more than simple changes, we recommend that you set up your own named branch to make contributions. It makes it far less likely that you’ll run into clashes when updating the repository.

*Git* is a software development tool . As such, it allows multiple branches in the “evolutionary tree” of the source code. We keep it simple; there is just a single “normal” branch, entitled `main`. This is the branch that tracks the current visible state of the Wiki. If you set up your own named branch to make contributions, you can then merge that with the `main` branch as needed. Tour named branch can be a workspace for you to try things, especially if your’e not sure how they will be rendered into HTML pages.

# Making contributions
## Your Obsidian vault
You need to make the cloned directory into an *Obsidian* vault.
- Open *Obsidian* and go to “Manage Vaults” 
    - See the [relevant Obsidian Help](https://help.obsidian.md/vault) for details
- You may choose whatever name you like for the vault - it will be local to you.

## Work in your local copy of the repository

1. Use your favourite `git` tool (command line, or GUI tool).
2. Do a `pull` from the repository to ensure your copy is fully up to date.
3. Make a new personal branch from this and check it out.
    - If you already have a personal branch, merge the current state of `main` into it.

## Open the vault in *Obsidian*

*Obsidian* will automatically work with whichever branch you have checked out.
1. Ensure that you’re [logged in to *Obsidian*](https://help.obsidian.md/publish/setup).
2. Make your contributions (additions and/or changes).
    - Note that Obsidian has two ways of displaying pages: reading and editing. Within editing mode there are a further two modes:  “Live preview” or “Source mode” (settable from the “Options” gear wheel icon).
	    - “Live preview” is a WSIWYG editor similar to Word. Use the right-click options menu to format, add links, etc. This is the mode to use if you are used to *Word* or similar tools.
	    - “Source mode” is what it says — you see the whole page as Markdown source code. Use that if you’re familiar with Markdown.
    - The [*Obsidian* Formatting Help pages](https://help.obsidian.md/syntax) have lots of advice on how to format your contributions.
    - Use the other existing pages as models for consistent page structure, image formatting, table formatting, source code contributions etc.

## Publish your drafts to see how they look
When you’re happy with your contributions:
1. [Publish them](https://help.obsidian.md/publish/setup#Open+Publish)
	- If you have any pages that have attachments (for example images or attached PDFs), ensure that you click on `Add linked` at the top of the **`Publish`** panel, so that the attachments get published along with their host pages.
2. Check the page formatting on [the wiki website](https://publish.obsidian.md/hhs-wiki/Hackerspace+Wiki)
3. Make any adjustments and re-publish 
4. Refresh the page to see the new content. You may have to do a [“hard refresh”]( https://www.wikihow.com/Force-Refresh-in-Your-Internet-Browser) to clear the browser cache.
	- Note that some part of the publish/browser cache combination seems to have limits on how frequently you can refresh a page. Sometimes if a page takes a few goes to get the published details right, it won’t change when viewed in your browser, even if you do a hard refresh. Try a different browser or take a coffee break.
## Check your changes in
If that’s all ok, check the changes into your branch
1. Please use appropriate useful comments. 
    - If you’ve done a range of alterations, feel free to check in batches of changes with different comments for each batch . 
2. If you made some experimental changes that you don’t want to keep, revert them, so that ‘git status’ shows nothing outstanding. 
3. Push your committed changes to the GitHub repository. 
4. If you’re working in your own named branch, checkout the `main` branch and merge your changes into it.
5. Push the merged `main` back up to GitHub.
## Have someone else proof them
It’s always good practice to have someone else look over any changes.
- We all occasionally have slips of the finger or the mind
- There’s no loss of face in finding these early!!

Then the entire cycle can start again…
