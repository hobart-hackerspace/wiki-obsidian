Not yet available.

Brian is working on this — watch this space or [ask him directly](mailto:brian.marriott@hobarthackerspace.org.au).

All offers of assistance (even just to test, especially on Android pads or phones) gratefully received.

*BWM 2026-02-14*

## Some brief notes on what and where so far.
- It works, and cleanly, with the [GitSync](https://gitsync.viscouspotenti.al/) plugin on my iPhone.
- But:
	- it’s not intuitive to set up, and 
	- I’ve not tested it on an iPad or any earlier iOS than the current one (26.2)
- So we need to test on a wider range of devices…
- Basic setup is simple
	- Before you start, set up within the GitHub a “[Personal Access Token (classic)](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens#creating-a-personal-access-token-classic)”
		- Make sure you stash it somewhere safe like a password vault.
	- Install *GitSync* app from the App Store.
	- Walk through the setup process.
- Process is:
	1. On device, 
		1. Run Obsidian and create a temporary vault.
			- Click on the “sidebar” icon in the top LH corner
			- Down at the bottom there should be a “Manage vaults”  option
				- It may be under an existing vault name.
			- Select “Create new vault” — give it a temp name
			- This process ensures that the Obsidian folder is in place and accessible
		2. Open the “Files” app and select the “On My iPhone” location (or equiv)
		3. Open the “Obsidian” folder”
		4. Within that, click on the triple dots in top RH corner and choose “New Folder”
		5. Give the new folder a suitable name, eg “Hackerspace Wiki””
			- This will be the location of the Obsidian vault
			- This one is empty, whereas the one created in Obsidian will have some settings in it.
	2. Within the GitSync app:
		1. Click on the “Auth”  button
			- Choose the “HTTPS”  Git protocol
			- Give it your GitHub userID (not the email address but the ID name)
			- Paste in the Personal Access Token
		2. Click on the “Remote” dropdown and paste in the the URL field the URL of our repository: 
			- [https://github.com/hobart-hackerspace/wiki-obsidian](https://github.com/hobart-hackerspace/wiki-obsidian)
		3. It should then download the repository.
	3. Go back to The Obsidian app
		1. “Manage vaults” should automatically show you the new “Hackerspace Wiki” vault.
		2. Choose that one and remove the temporary one.
- For the record, I struck a snag on the authentication bit
	- OAuth works fine and I could see and sync to any of my personal repos
	- But I couldn’t see the Hackerspace ones that I have access to but don’t own.
	- I ended up using my relevant “Personal access token”
- 