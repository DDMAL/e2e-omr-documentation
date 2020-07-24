# End-to-End OMR Workflow Documentation

This project is meant to create a base of documentation for users intending to create and use an optical music recognition workflow in Rodan.
It should link to more specific documentation when applicable but include all of the information necessary to get started.

A tutorial should also be created going through the entire workflow process with an example manuscript. Ideally it should be possible for a user to get the resources they need to replicate the workflow if they want.

## AUTHORS.txt file

*Do not manually edit the AUTHORS.txt file!*

Ensure your git configuration is up to date and then fill it using the following command:
```bash
git log --format='%aN <%aE>' | sort -f | uniq > AUTHORS.txt
```
This will use the information in the git history to make a list of contributors.
