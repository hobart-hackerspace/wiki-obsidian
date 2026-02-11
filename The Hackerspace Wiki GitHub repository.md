# The GitHub repository
Our wiki operates under source control, using `git` as the source code manager and  [`GitHub`](https://github.com/hobart-hackerspace/wiki-obsidian) as our shared repository. This is a private repository, with access given to Hackerspace members only. If you'd like access, simply ask a committee member or send an email to committee@hobarthackerspace.org.au.

# The `git` branches
There are currently three primary branches in our repository: `wikmd`, `published` and `draft`. 
At any point in time there may be others, associated with different projects or individuals, but those are the “workhorse” branches.

`draft` and `published` represent  temporary and long-term states of the wiki, respectively. `wikmd` is there to support migration from the currently-live [wiki](http://wiki.local), which is based on WikMD software.

## The migration base branch (`wikmd`)
This is the base branch for migrations from our older (but still running) `wikmd` wiki. This branch will no longer be necessary once we’ve fully migrated to Obsidian. It can then be archived.

It is intended that a migration is started with an `rsync` copy into a directory built from this branch from the chosen backup directory of the live `wikmd` system. The specific command, executed within the target directory would be:
```bash
rsync -aC <sourcedir> . 
# the "C" above stops it from messing with the git files
```

The branch is structured as close as possible to the source `wikmd` directories, except that the `wikmd` `git` information has been replaced by that required to keep the `github` repository working cleanly. It contains:

- Raw (unchanged) copies of all the markdown files current at the time of the migration.
- An unchanged copy of the current `wikmd` `/img/` directory
- No `attachments/` directories
- No `.obsidian/` folder or references.
- 

Files which were part of previous migrations that have not been changed in the live `wikmd` are checked into the `github` repository with check-in dates corresponding to their migration dates.

## The published Obsidian branch (`published`)

This is the branch which reflects the Obsidian-generated website, from which `Obsidian` can be run and from which `Obsidian Publish` is invoked. All files and directories are in Obsidian markdown and the Obsidian environment is in place.

It contains:

- Obsidian markdown formatted files derived from the most recent migration, possibly with content changes made since then on the Obsidian side.
- Attachments in relevant `attchments/` directories. Some will be migrated from the `wikmd` `/img/` directory, some may be more recently loaded into Obsidian.
- No `/img/` directory
- A current `.obsidian/` folder reflecting the in-use plug-ins, theme and other settings.

## The volatile Obsidian branch (`draft`)

This is the branch to trial new content.

Use `draft` as somewhere where you can add or update content, or share it for review by others. Publish briefly from this branch to verify the format of published HTML, without the concern that you've messed up the live site.

Before you make any changes to your `git` copy of the site, check that it's up-to-date with the [`github`](https://github.com/hobart-hackerspace/wiki-obsidian) primary repository by checking `git status` or simply doing `git pull`.

When you're happy with your changes:

- make sure that your copy of `published` is up to date (use `git pull`)
- merge the `draft` branch into the `published` one,
- publish it again from that branch and
- push _both_ branches back to [`github`](https://github.com/hobart-hackerspace/wiki-obsidian).
