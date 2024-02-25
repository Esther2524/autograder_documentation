# autograder_documentation

## Usage of Git
1. `git fetch origin`
2. `git pull origin main`
3. `git checkout -b esther-hw6`
This command is used to create a new branch named esther-hw6 and immediately switch to it. The `-b` flag tells Git to create a new branch. If the branch `esther-hw6` does not exist, Git will create it based on the current branch you are on.

4. `git push origin esther-hw6`
This command pushes the commits from your local branch esther-hw6 to the remote repository. If the branch doesn't exist on the remote, it will be created. However, without -u, your local branch will not set up a tracking relationship with the remote branch. This means that in the future, you'll have to specify the remote and branch names explicitly when pushing or pulling.
5. `git branch`
This command will display a list of all local branches. The branch you're currently on will be highlighted and marked with an asterisk (*).

6. `git branch -D branch_name`

## Test Environment
`source venv/bin/activate`

## Test functions
1. AST 
check if certain function contains something?

2. use `call_verify` to test each function in the function file
```
def call_verify(self, expected, function_name, *args, **kwargs):
    """
    This function is a helper function for the test cases. 
    It calls the verify function and then compares the actual and expected values.
    """
    passed, actual, msg = verify(
        self.expected_module, function_name, expected, *args, **kwargs)

    # if verify returns False, then the test failed
    if not passed:
        self.fail(msg)

    # otherwise, compare the actual and expected values
    self.assertEqual(actual, expected, msg)
```
3. 
