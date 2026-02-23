# Pre-requisites. 
You’ll need to ensure all these are in place. But you’ll only have to do them once:-)

## 1. *Obsidian*
You need to have *Obsidian* installed on your machine/device. 
- See [obsidian.md](https://obsidian.md/download) or you device app store for details.
## 2. *Obsidian* ID
- You’ll also need to register [an Obsidian id](https://obsidian.md/auth) with your email address 
- And arrange to have this account authorised to publish to our Wiki:
    - Notify the [HHS Obsidian administrator](mailto:obsidian@hobarthackerspace.org.au) of your address
    - They will send you an invitation
    - Click on the link in the invitation to click on to accept it
## 3. *GitHub* account
If you don’t already have a *GitHub* account, that needs to be set up.
- Go to [github.com/signup](https://github.com/signup) and follow the prompts.
- Note that the messiest bit is setting up your authentication. 
	- *GitHub* has strong protections against fraudulent submissions to their repository. 
	- Fortunately it’s a one-off process. 
	- You’ll need an account and an “authentication token” (like a password but stronger), either an `ssh` key or a “Personal Access Token (classic)”. 
		- `ssh` keys
			- The author of *GitSync* provides [a good guide to setting up a GitHub `ssh` key](https://viscouspotenti.al/posts/gitsync-all-devices-tutorial#generate-and-add-ssh-key-for-github). Note that this  is only for machines with a command line interface.
		- Personal Access Tokens (PATs)
			- The GitHub guide to creating PATs is [https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens#creating-a-personal-access-token-classic](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens#creating-a-personal-access-token-classic)
		- Whichever you choose, store it somewhere secure but accessible, like a password vault such as [BitWarden](https://bitwarden.com/products/personal/). You may also need to link that to a time-based (TOTP) authenticator (BitWarden can do that too).
## 4. *Git*
You need to have *Git* installed on your machine/device.
### Laptops & desktops
- You can use `git` either on the command line or you can use a *Git* GUI tool such as:
    - [*GitHub desktop*](https://github.com/desktop/desktop?tab=readme-ov-file#where-can-i-get-it)
	    - *Brian note: If you are using Windows or MacOS,  I’d strongly recommend this, even if you’re an experienced `git` user. It saves you having to remember most of the commands and provides you with a visible reminder for things like update descriptions and pushing back to the repo. It’s free & open source, fast and simple to use.*
    - [*Sourcetree*](https://www.sourcetreeapp.com/), 
    - [*Visual Studio*](https://learn.microsoft.com/en-us/visualstudio/version-control/git-with-visual-studio?view=visualstudio)
    - or one of the many others.
- The [*Git* website](https://git-scm.com/) has links for installation, tools and the CLI reference.
### Pads & phones
- Go to your app store and install [*GitSync*](https://gitsync.viscouspotenti.al/)
	- **App Store**: [https://apps.apple.com/ca/app/gitsync/id6744980427](https://apps.apple.com/ca/app/gitsync/id6744980427)  
	- **Play Store**: [https://play.google.com/store/apps/details?id=com.viscouspot.gitsync](https://play.google.com/store/apps/details?id=com.viscouspot.gitsync)
- Follow the relevant set of instructions (*Android* or *iOS*) in the [GitSync guide](https://viscouspotenti.al/posts/gitsync-all-devices-tutorial#setting-up-gitsync). 
- Note, however, these differences:
	1. Our repository does not (at the moment) work with OAuth authentication. You’ll need to use an `ssh` key or  PAT as described above.
	2. You’ll have to paste in the URL of our repository (`https://github.com/hobart-hackerspace/wiki-obsidian`). GitSync only offers a dropdown list of your personal repositories.
## 5. Repository write access
This is necessary if you’d like to contribute directly to the Wiki. Simply send a note to github@hobarthackerspace.org.au asking to be added as a user to the Hackerspace repository members list. 
