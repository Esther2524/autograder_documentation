# autograder_documentation

## Usage of Git
1. `git fetch origin`
2. `git pull origin main`
3. `git checkout -b esther-hw6`
This command is used to create a new branch named esther-hw6 and immediately switch to it. The `-b` flag tells Git to create a new branch. If the branch `esther-hw6` does not exist, Git will create it based on the current branch you are on.

4. `git push origin esther-hw6`
This command pushes the commits from your local branch esther-hw6 to the remote repository. If the branch doesn't exist on the remote, it will be created. However, without -u, your local branch will not set up a tracking relationship with the remote branch. This means that in the future, you'll have to specify the remote and branch names explicitly when pushing or pulling.
5. 
## Test Environment
`source venv/bin/activate`

## Test functions
1. AST 
check if certain function contains something?
