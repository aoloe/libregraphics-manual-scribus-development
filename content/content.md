# Contributing to Scribus

## Contributing code

## Improving the user interface

## Testing and writing bug reports

## Writing manuals, articles, documentation


# Installing the development version of Scribus

The current development version of Scribus is 1.5svn.

If you want to play with it and, eventually, submit propositions for improvements or bug reports you can get a pre-compiled package, if one is available for your platform.



# Working with Github

- If you don't have an account on Github, create one


# Your first patch using GitHub

- Fork the Scribus repository:
    - Go to https://github.com/scribusproject/scribus
    - Click on the button in the top right corner that says "Fork"
    - You now have your own repository called Scribus (https://github.com/yourname/scribus, where yourname is the name you have chosen for Github)
- Each time you want to do a patch, create a new branch of your repository, with a name that matches your change.
- In Scribus find a string that refers to what you want to change and that is somehow not too common.
- On Github, search for that string in the repository by using the "Search" field in the top part of the page.
- Click on the file that matches your result (you might have to click on a few files before finding the right one; if you get too many results, try to use another search criterium and put quotes around sentences)
- Click on the edit icon make the change and commit it.
- If you want to test the change, clone the repository on your computer (or pull the new version if you already have it locally), switch to the new branch and compile as usual.
- If you're happy with your change, make a pull request by going to the main page of your repository and clicking on the green button left of the branch name, in the header of the page.
- Don't use the same branch for other changes, since those will be added to the pull request you already sent.
Once you're comfortable with this way of doing, you should try to do patches on the source code that you have pulled on your computer.

# Using Qt Creator
...

# Working with SVN

The official Scribus repository is provided through SVN at <svn://scribus.net/Scribus>

# Proposing a patch

If you want to directly propose a patch to the Scribus, you cannot yet use the process described in the Github specific section.

## Creating a patch file

- when doing the diff, you have to be in the main scribus directory (the one below the "scribus/" directory that contains the whole code).
- use the version system to create the patch:
  - `svn diff scribus/the_file_you_have_modified > name_of_my_patch.diff`
  - `git diff scribus/the_file_you_have_modified > name_of_my_patch.diff`
  that way you don't have to keep a copy of the original file, but can "automatically" compare the file you modified with the version you got when you last updated the repository.

Of course, you don't have to use the command line tool, but you can use your svn or git client to create the patch.

## Uploading a patch to the bug tracker

Once you have crated an account for the bug tracker you can attach your patch to an existing ticket (or create first a new ticket for it, if none matching exists).

- upload each diff file individually: that way they can be viewed in mantis itself

