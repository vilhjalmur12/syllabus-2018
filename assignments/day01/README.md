# Getting to know Linux

Getting to know the Linux operating system

- Create a git repository on GitHub for the course.

## Questions

- [ ] What is Linux?
- [ ] Why use Linux? What are the pros and cons?

## Objectives

- [ ] Set up Linux Ubuntu 18.04 on your machine (options include, mono/dual
      boot, Virtual Machine options (VMWare, VirtualBox, Parallels), Boot from
      USB) (Not necessary on Mac)
- [ ] Install an editor (options include VS Code, Atom, WebStorm, Sublime).
- [ ] Install git, NodeJS.

## Assignment

Create a bash script `verify_environment.sh` that checks required programs/dependencies. 

- [ ] Make sure all bash commands are commented.
- [ ] The script should prompt the user with:
  - [ ] A welcome message that includes the current user’s username (the
        username should not be hard coded).
  - [ ] Information on what it does
  - [ ] What type of Linux system it is running on
- [ ] Display the date and time when the script starts and ends.
- [ ] The script should complete checking versions of all tools.
- [ ] The script should generate a log file and output to terminal.

Check for presence and/or output version of the following
- [ ] Operating system. Supported are Ubuntu and OsX (darwin).
- [ ] Git. 
- [ ] Npm. 
- [ ] NodeJS (node). 

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
├── verify_environment.sh
└── assignments
    └── day01
        └── answers.md
```

They must be placed at this location to get full marks.\
YourGitRepository/assignments/day01/answers.md\
YourGitRepository/verify_environment.sh

You should make a copy of your setup script and maintain it through out the 
course for the final handin.

## Tips and tricks
Bash supports functions. Functions in bash behave like commands, not like functions in regular programming
languages. That means they have stdout,stderr and return codes.

The command "tee" redirects output to a file and stdout.

Google can help you find a solution to almost every problem in bash. You just have to know how to phrase your search.
