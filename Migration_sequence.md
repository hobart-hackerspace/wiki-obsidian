# Migration sequence from `Wikmd` to `Obsidian Publish`

(*This is really notes for Brian because he forgets so much these days...*)

## Initial copy
1. Copy entire backup directory on to Brian's Mac
1. Entitle it `hhs-wikmd-capture-unchanged`
1. Duplicate that & call it `hhs-wiki-working-copy`
1. Within `hhs-wiki-working-copy`, delete the entire `.git` directory
	- We now have a clean slate from which to start a new `GitHub` repository
2. We add a `.gitignore` file for future use so that we don't end up tracking changes to files that only store state.
	- Create a file called `.gitignore` and add to it the following content:
``` git
# Ignore volatile UI-related state
.obsidian/workspace.json

.DS_Store
Thumbs.db
```
## `GitHub`
1. Before you can work with the `GitHub` repository you have to have relevant access and authentication set up.
	- A working `GitHub` account
	- Read-write access granted to the `hobart-hackerspace` `GitHub` repositories
		- If you don't have that, ask someone to set you up. Currently the owners are: Alistair, Tom, Leo, Brian, Trent & Shane
	- Authentication set up for your account so that you can push changes.
		- If you're using `git push` from the command-line, that's easiest if you provide a `ssh` key. [(GitHub doco tells you how)](https://docs.github.com/en/authentication).
		- With a GUI tool, that tool should guide you.
## Base repository
1. Log into `GitHub` and create a new repository.
	- Call it `wiki-obsidian`
1. Make the `hhs-wiki-working-copy` directory a local `git` repository
``` bash
git init
git add *
git add */*
git add .*
git add .*/.*
git commit -m "Initial commit of all from WikMD except the git repository"
git branch -M wikmd`
```
2. We now have a branch against which we can compare future changes between the captured `wikmd` version and the live version, by re-importing all files and `git diff`-ing.
3. At this point we can push it up to `GitHub`
``` bash
git remote add origin git@github.com:hobart-hackerspace/wiki-obsidian.git
git push -u origin wikmd
```

## Working repository

There are three major phases from here:
- Getting things working in `Obsidian`
- The "low-hanging fruit" - changes that all or most of the files and directories will need
- Detailed editing - small changes to individual files to make them work in `Obsidian` and then when published

But first, create two new branches to work in. These will be our primary working branches as thing evolve.
We'll call them `published` and `draft`. We also replicate them to `GitHub`

``` bash
git branch published
git checkout published
git push --set-upstream origin published
git branch draft
git checkout draft
git push --set-upstream origin draft
```

Note that we end up with the **`draft`** branch checked out. That's where we'll do most of the work.
### Obsidian setup
This assumes that you've got Obsidian installed on a suitable device. It runs on Linux, MacOS & Win. While it also runs on Android & iOS, I'm not sure if they will run it in conjunction with `git` -- someone please tell me...

### Terminology
Obsidian's background is as a "Personal knowledge base" so its terminology is different from that of a content management system for websites. In particular:

- "**Vault**"
	- A vault is a a single knowledge base. 
	- In practice that is a single base directory (with subdirectories) of markdown files, attachment files (usually media) and other files used by Obsidian for configuration etc.
	- The name of the vault is always the name of the directory
	- An Obsidian vault is treated as an entity when it comes to publishing and replication. Searches, indexing, etc. work within the entirety of a vault. You may have as many vaults as you like for your own purposes; for Hackerspace we have one "wiki" vault.
- "**Note**"
	- A single Markdown file, which will produce a single web page in our Wiki.
	- When displaying or linking to a note, Obsidian does not show the "`.md`" file suffix. But it's still there and must be used in links.
- "**Links**"
	- Obsidian understands two different syntactical forms for links to other documents - "Markdown links" and "Wikilinks".
		- Markdown links are in the standard Markdown form - eg "`[A page](A%20page.md)`" where the displayed text is enclosed in square brackets and the link target is URL-encoded text enclosed in round brackets.
		- Wikilinks are simpler (but more constrained ) in form: "`[[A page]]`". No URL encoding is required and the file suffix is not given, but the displayed text will match the file name.
	- In order to publish as HTML, we need to use the standard Markdown link form.

#### First the `attachments` directory
Before turning our working directory into an Obsidian vault, it's worth making one structural change - the location of attachments is different in `Obsidian` from `WikMD`.

- Remove the empty `attachments` directory
- Change the name of the `img` directory to `attachments`
- Add that change to `git`, `commit` it and `push` to `GitHub`
``` bash
git mv img attachments
git commit -m "changed `img` directory to `attachments` to fit with obsidian"
git push
```
- Please note that this process nearly doubles the (present) size of the repo (adds about 70MB), because `git` treats the `git rm` as a removal & recreation, without doing any deduplication. So we end up with two copies of each attached file in the `git` `objects` directory. We could have avoided this by changing directory names before the initial commit but then whenever we want to compare live to the saved replica we'd have to make the change again.

#### Now create the `Obsidian` vault
- Open `Obsidian`
- If you don't have any current vault open, it will present you with the "Vault management" page. If you have one open, click on the name of that vault in the bottom-left corner of the window and select `Manage vaults...`.
- Click on `Open folder as vault` and select your `hhs-wiki-working-copy` directory.

The HHS Wiki is now your current open vault.

#### Obsidian settings
Obsidian keeps the settings for each vault separate from other vaults, so you're able to have different settings for Wiki one from those you may have chosen for your own vault(s).

The settings that are useful for managing and publishing the HHS wiki that are different from the default settings are:
- Editor
	- Display
		- `Readable line length`: off 
		- `Strict line breaks`: on
	- Behaviour
		- `Indent using tabs`: on
- Files and attachments
	- `Default location for new notes`: "Vault folder"
	- `Default location for new attachments`: "In subfolder under current folder"

***I'm sure there's more yet. I need to do a full mapping from a default installation.***

#### Commit those settings
The above process will have created a subdirectory called `.obsidian` in your `hhs-wiki-working-copy` directory, containing various config-type files. If we commit that directory to our repository then it will share the settings across other folks' copies.

``` bash
git add .obsidian/*
git commit -m "created obsidian vault"
```


### Low-hanging fruit
#### Attachment links
- We changed the `img` directory to `attachments`. We have to adjust all attachment links to match.
- No doubt there's a simple `sed` script to do that, but I simply used a project-wide replace in my favourite IDE, replacing all occurrences of "`(/img/`" with "`(attachments/`". 
- The omission of the leading "`/`" is deliberate as the setting for new attachments is for local sub-folders.
- It reported 66 links in 19 files.
- So we commit that:
``` bash
git add *.md
git commit -m "Adjusted attachment links"
```
#### Line indentation character
- `WikMD` is agnostic as to whether lines are indented using tab characters or multiple spaces; `Obsidian` is less flexible. It either uses tabs or precisely 4 spaces.
- So our documents have ended up with a mix, wihich causes confused formatting when published.
- So a global change was made of:
	- all leading 4-space groups to tabs, then
	- tab followed by 4 spaces to 2 tabs
	- this was repeated until no more existed
- (Of course, I'm sure `sed` could do it, too:-)
- ... and then commit.
#### Logo & favicon
- We have logo files and a favicon set in the MS365 file store, so copy those to the `attachments` folder. (`logo-192x192.png` and `favicon.ico`)
- When publishing we identify the logo file to `Publish`; it will find the favicon automatically.
# Publication
## First try
Having done all the above, we tried publishing, using Brian's `Obsidian Publish` subscription.
This gives the opportunity to check that we got things basically right.

Yay!! It worked. 
### First `Publish` settings:
#### Site
- Publishing to: `hhs-wiki`
- Published URL: [`https://publish.obsidian.md/hhs-wiki/`](https://publish.obsidian.md/hhs-wiki/)
#### General
- `Site name`: Hobart Hackerspace Wki
- `Homepage file`: Hackerspace Wiki
- `Logo`: attachments/logo-192x192.png
- `Site collaboration`: None initially
- `Custom domain`: none initially
- `Search engines`: disallowed
#### Appearance
- `Theme`: light
- `toggle`: on
#### Reading
- `Hover`: on
- `Hide page title`: off
- `Readable line length`: off (but doesn't seem to work...)
- `Strict line breaks`: on
- `stack pages`: on
#### Components
- `Nav`: yes
- `Custom nav`: home page at top, others unchanged
	- `hidden`: attachments folder
- `search`, `graph`, `ToC` & `backlinks`: all on
#### Other settings- ``:
- `passwords`: one -- "spacehackers@#"

## Shared publication
The next step was to publish using another `Obsidian` account.

### Hackerspace account
- As the trial publication had been done using Brian's account, a shared publication was tried with a new Hackerspace account, with email address `obsidian@hobarthackerspace.org.au`.
- The account is set up as an email address list in MS365, currently forwarding only to Brian.
- The account was registered with Obsidian. 
	- But no subscription fees were paid nor payment details supplied.
	- Password is (as usual) in the vault.
- This account was recorded in the `Publish` settings as a collaborator.

### Publication process
Couldn't have been easier.

- Obsidian was installed on a linux machine (with desktop - Obsidian is a GUI application)
- The `GitHub` repository was cloned
	- `git clone git@github.com:hobart-hackerspace/wiki-obsidian.git`
- Obsidian was opened and the `wiki-obsidian` directory opened as a vault.
- a simple change was made to one file (this one) and publication initiated
	- the changed file was identified by Obsidian, selected and published
	- It duly shoed up on the website
- The change was then committed to `git` and pushed to the repository

## Shared publication takeaways
1. A paid `Publish` subscription is required for the site
1. Paid accounts are ***not*** required by individual contributors
1. Change management (`git commands`) discipline is essential
	- Pull from `GitHub` before making edits
		- merge with local changes or otherwise resolve conflicts if necessary
	- Edit and review in Obsidian
	- Publish and review on site
		- Not forgetting to force refresh
	- Commit and push to `GitHub` after publication

# Links
`WikMD` is as flexible with link formats as it is with indentation. In particular:
 - it allows both standard MarkDown links `[displayed words](hidden URL)` and Wiki links `[[words which are also the link]]`.
 - For internal links:
	 - It allows the "URL" bit to have embedded spaces
	 - It doesn't add the `.md` suffix to file titles
	 - It allows image references, of the form `![](url){scaling info}`. 
		 - Where `Obsidian` uses the form `![|width](url)`

So we have a random mix of all the above in our Wiki Markdown.

It may be necessary to manually convert these, but it'll be a bit of work; a quick `grep` shows over 300 links (although many may be external).

``` bash
❯ grep \]\( *.md | wc
     315    3841   41041
❯ grep \]\] *.md | wc
      20     311    2134
❯ grep http *.md | wc
     148    2036   22916
```

So I'll experiment with a bit of scripting first.

The bit that will need to be adjusted is the "URL" bit. Some are fine: external (`http:` or `https:`) links and links to anchors in the same document (`#` prefixes). The ones to treat are those to other files within the wiki. 

## Target forms:
### Markdown links
#### Form
The following form (usually without line breaks): 
``` bash
"["
<some text, possibly including pairs of "[]", backtick pairs and HTML links>
"]("
<more text, possibly including space characters but not starting with "#" or URL prefix strings ("html:", "mailto:", etc)>
")"
```
#### Regular expressions
I'm going for simplicity here - edge cases can be handled manually, as each page will need to be proof-read, anyway.

- Basic Markdown link, including the `<a>` body text:
	- This looks at the entire line
	- `\[.*\]\(.*\)`
-  Looking within the link portion only:
	- External link
		- Defined by  having a non-empty "scheme" component and non-empty "path"
		- `^.+\:.+`
	- Anchor within the document
		- A "`#`" at the start
		- `^\#.+`
#### Action
1. URL encode the link text
2. If the final `")"` is not preceded by "`.md`", then that suffix is to be inserted.
### WikiLinks
#### Form
They take the following form: 
`"[["<link text>"]]"`
#### Regular expressions
A simple target of `\[\[.*?\]\]` suffices.
#### Action
Expand by dropping to single "`]`"s and then repeating the link text,
in braces ("`()`"), URL encoded and with "`.md`" appended.

## Links script
I ended up with a Python script to do the above in one pass over each file.

The script is simply `main.py` in the directory `wiki-convert-markdown`. This  is in the GitHub repository [`hobart-hackerspace/wiki-convert-markdown`](https://github.com/hobart-hackerspace/wiki-convert-markdown). Run it in the relevant `uv` venv using the command `python -m main -f "filename.md"`


# Some notes for Brian

## Records of stuff-ups etc that won't be repeated if the sequence above is followed

### The `.gitignore` file
I omitted to create this at the beginning and had to do it later. The file has since been added into each of the branches, so `wikmd` and `published` branches each have a second commit at what should be the start of the repository history.
