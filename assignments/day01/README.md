# Updates

- Verify environment git (> 2.17).
- export and use environment variable.
- use parameters / and set local variables.
- If all are verified, checkout repostiroy that is sent in as parameter.
- install nodejs 10 LTS, npm.
- Check if everything at X version.
- Run all checks, do not exit on first error.
- Continuously update this script when we add dependencies.

# Getting to know Linux

Getting to know the Linux operating system

- Create a git repository on GitHub for the course.
- Solve the following problems inside folder assignments/day1 in your repository:

## Questions

- [ ] What is Linux?
- [ ] Why should you use Linux? What are the pros and cons?

## Objectives

- [ ] Set up Linux Ubuntu 18.04 on your machine (options include, mono/dual
      boot, Virtual Machine options (VMWare, VirtualBox, Parallels), Boot from
      USB) (Not necessary on Mac)
- [ ] Install an editor (options include, VS Code, Atom, WebStorm, Sublime).
- [ ] Install git, NodeJS, yarn.

## Assignment

Create a bash script that installs all your programs/dependencies (text editor
and git.

- [ ] Make sure all bash commands are commented.
- [ ] The script should prompt the user with:
  - [ ] A welcome message that includes the current user’s username (the
        username should not be hard coded).
  - [ ] Information on what it does
  - [ ] What type of Linux system it is running on
  - [ ] Ask “are you sure you want to continue y/n” and abort if the user types
        in anything other than ‘y’ or ‘Y’.
- [ ] Display the date and time when the script starts and ends.
- [ ] The script should stop if it encounters an error and give descriptive
      error messages
- [ ] The script should generate an error log (script-error.log) with the error
      message, should not be generated on success (or at least deleted)
- [ ] The script should generate a success log (report.log) with
      information on what was just accomplished (only if there are no errors).

## Adding an SSH key to GitHub

The following information can be found
[here for creating new key](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/)
and
[here for adding to github](https://help.github.com/articles/adding-a-new-ssh-key-to-your-github-account/)

Now commit your changes and clone the repository with SSH instead of HTTPS. SSH 
is more convenient when automating build systems because it allows for
password-less authentication. It is also more secure e.g. keyloggers.

## How do I know I'm done?

- [ ] I have answered the questions about Linux
- [ ] I have created an executable script that completes all requirements
- [ ] I have commented all the lines in the script (what is the purpose of each
      line of code)
      
## Handin

You should store the answers and setup scripts inside your repository:

```text
.
└── assignments
    └── day1
        ├── answers.md
        └── setup.sh
```

They must be placed at this location to get full marks.\
YourGitRepository/assignments/day01/answers.md\
YourGitRepository/assignments/day01/setup.sh

You should make a copy of your setup script and maintain it through out the 
course for the final handin.