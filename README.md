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
@weight(0.08)
@visibility("visible")
def test_get_average_invalid_drop_lowest2(self):
    '''gradebook: Test get_average with invalid drop_lowest'''
    numbers = [1, 2, 3, 4, 5]
    drop_lowest = -1
    expected = 3.0
    self.call_verify(expected, 'get_average', numbers, drop_lowest)
```
just call call_verify directly.
if we want to display customized message to students, then we need to write test code in a different way.
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
we don't have to use `call_verify`. we can write some customized test code ourselves.
especially for this part:
```
imported_module = __import__(self.expected_module)
get_neighbourhood_with_most_parks = getattr(imported_module, 'get_neighbourhood_with_most_parks', None)
```
```
########################################
# Test get_neighbourhood_with_most_parks functionality
########################################

# Check that get_neighbourhood_with_most_parks returns a list and the content of the list matches the expected outcome
@weight(0)
@visibility("visible")
def test_get_neighbourhood_with_most_parks_returns_list(self):
    """parks_functions: Check that get_neighbourhood_with_most_parks returns a list"""
    # Sample data to pass to the function
    sample_neighbourhood_park_dictionary = {
        'Neighbourhood1': ['Park1', 'Park2'],
        'Neighbourhood2': ['Park3'],
        'Neighbourhood3': ['Park4', 'Park5', 'Park6'],
    }

    # Expected result for the given sample data
    # For example, if multiple neighbourhoods have the same max number of parks, include them all
    expected_result = ['Neighbourhood3']

    # Import the module and function to test
    imported_module = __import__(self.expected_module)
    get_neighbourhood_with_most_parks = getattr(imported_module, 'get_neighbourhood_with_most_parks', None)

    # Ensure the function exists
    self.assertIsNotNone(get_neighbourhood_with_most_parks, msg="Function get_neighbourhood_with_most_parks is not defined")

    # Call the function with the sample data
    result = get_neighbourhood_with_most_parks(sample_neighbourhood_park_dictionary)

    # Verify the result is a list
    self.assertIsInstance(result, list, "The return type of function get_neighbourhood_with_most_parks() should be a list.")
    
    # Verify the result matches the expected output
    # Verify the result matches the expected output
    self.assertEqual(result, expected_result, f"The return type of function get_neighbourhood_with_most_parks() is correct (list), but the elements inside the list are not as expected. Expected {expected_result} but got {result}. \
Ensure your function correctly identifies the neighbourhood(s) with the most parks.")
```
3. 
