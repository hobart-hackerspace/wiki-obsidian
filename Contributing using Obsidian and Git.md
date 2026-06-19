# Note that this is a work in progress
All offers of assistance (even just to test, especially on Android pads or phones) gratefully received.

*BWM 2026-06-19*

# Pre-requisites. 
You’ll need to ensure all the [Wiki contribution pre-requisites](Wiki%20contribution%20pre-requisites.md) are in place. Fortunately, you’ll only have to do them once:-)

# The repository
You’ll need to connect to and clone the repository
- The repository is at [github.com/hobart-hackerspace/wiki-obsidian](https://github.com/hobart-hackerspace/wiki-obsidian)
- Make a local copy of the repository by cloning it. 
	- On a laptop or desktop, use your chosen `git` tool to clone the repository into a workspace directory.
- If you want to be able to push your contributions directly to the repository,  you’ll need to be added to the Hackerspace Github group. 
    - Send a note to github@hobarthackerspace.org.au asking to be added as a user to the members list.
    - Alternatively, you can suggest changes by creating a “Pull” request.
## Repository branches
*Git* is a software development tool . As such, it allows multiple branches in the “evolutionary tree” of the source code. For day-to-day stuff, we keep it simple; there is just a single “normal” branch, entitled `main`. This is the branch that tracks the current visible state of the Wiki. 

# Making contributions
## Your Obsidian vault
Make the cloned directory into an *Obsidian* vault.
- Open *Obsidian* and go to “Manage Vaults” 
    - See the [relevant Obsidian Help](https://help.obsidian.md/vault) for details
- You may choose whatever name you like for the vault - it will be local to you.

## Work in your local copy of the repository

1. Use your favourite `git` tool to push to & pull from the repository
	- See [Wiki contribution pre-requisites](Wiki%20contribution%20pre-requisites.md#4.%20*Git*)
2. Do a `pull` from the repository to ensure your copy is fully up to date.
3. [Optional]:
	1. If you’re [making major changes](#Making%20major%20changes%20[optional]), make a new personal branch from this and check it out.
	2. Or, if you already have a personal branch, merge the current state of `main` into it.
## Open the vault in *Obsidian*

Make your contributions (additions and/or changes).
- Note that Obsidian has two ways of displaying pages: reading and editing. Within editing mode there are a further two modes:  “Live preview” or “Source mode” (settable from the “Options” gear wheel icon).
	- “Live preview” is a WSIWYG editor similar to *MS Word*. Use the right-click options menu to format, add links, etc. This is the mode to use if you are used to *Word* or similar tools.
	- “Source mode” is what it says — you see the whole page as Markdown source code. You may find that more convenient, if you’re familiar with Markdown.
- The [*Obsidian* Formatting Help pages](https://help.obsidian.md/syntax) have lots of advice on how to format your contributions.
- Use the other existing pages as models for consistent page structure, image formatting, table formatting, source code contributions etc.

## *Publish* your changes to see how they look
When you’re happy with your contributions:
1. [*Publish* them](https://help.obsidian.md/publish/setup#Open+Publish)
	- Ensure that you’re [logged in to *Obsidian*](https://help.obsidian.md/publish/setup), so that *Publish* will work.
	- If you have any pages that have attachments (for example images or attached PDFs), ensure that you click on `Add linked` at the top of the **`Publish`** panel, so that the attachments get published along with their host pages.
2. Check the page formatting on [the wiki website](https://publish.obsidian.md/hhs-wiki/Hackerspace+Wiki)
3. Make any adjustments and re-publish 
4. Refresh the page to see the new content. You may have to do a [“hard refresh”]( https://www.wikihow.com/Force-Refresh-in-Your-Internet-Browser) to clear the browser cache.
	- Note that some part of the publish/browser cache combination seems to have limits on how frequently you can refresh a page. 
	- Sometimes if a page takes a few goes to get the published details right, it won’t change when viewed in your browser, even if you do a hard refresh. 
	- Try a different browser or take a coffee break.
## Check your changes in
If that’s all ok, check the changes into your branch
1. Please use appropriate useful comments. 
    - If you’ve done a range of alterations, feel free to check in batches of changes with different comments for each batch . 
2. If you made some experimental changes that you don’t want to keep, *revert* them with your `git` tool, so that ‘git status’ shows nothing outstanding. 
3. Push your committed changes to the GitHub repository. 
4. If you’re [working in your own named branch](#Making%20major%20changes%20[optional]), checkout the `main` branch and merge your changes into it.
5. Push the merged `main` back up to GitHub.
## Have someone else proof them
It’s always good practice to have someone else look over any changes.
- We all occasionally have slips of the finger or the mind
- There’s no loss of face in finding these early!!

Then the entire cycle can start again…

## Making major changes [optional]
If you’re making more than simple changes, we recommend that you set up your own named branch to make contributions. 
- Note that *Obsidian* will automatically work with whichever branch you have checked out, as the vault is just a directory and checking out branches simply changes the directory contents, not the directory itself.
- You can then merge your branch with the `main` branch as needed. 
- If you already have a personal branch, merge the current state of `main` into it each time you go to make changes or new contributions.
- Your named branch can be a workspace for you to try things, especially if your’e not sure how they will be rendered into HTML pages. 
	- It makes much easier to fiddle around without disturbing the work that others have done. 
	- Also, it’s far less likely that you’ll run into clashes when updating the repository.


