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
delete a local branch

7. `git checkout esther-hw6`
we can use this to check uncommitted or unupdated changes

## Test Environment
`source venv/bin/activate`

## Test Function File
1. AST 
check if certain function contains something?

2. use `call_verify` to test each function (mainly the output of the function)
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
## Test the Driver File
1. checks the existence of display_neighbourhood_with_most_parks but also displays a descriptive message if the function does not exist
```
# Test display_neighbourhood_with_most_parks exists
@weight(0)
@visibility("visible")
def test_display_neighbourhood_with_most_parks_exist(self):
    """parks_driver: Test that display_neighbourhood_with_most_parks exists"""
    # Attempt to import the expected module
    exec(f"import {self.expected_module}")
    # Check if the function exists in the imported module
    function_exists = hasattr(eval(f"{self.expected_module}"), 'display_neighbourhood_with_most_parks')
    # Assert that the function exists, with a custom message if it does not
    self.assertTrue(function_exists, f"The function 'display_neighbourhood_with_most_parks' does not exist in the {self.expected_module} file. \
Please ensure you have implemented this function as required.")
```
2. check if a certain function is called/not called in `main`
```
# check if the display_neighbourhood_with_most_parks function is called at least once anywhere in the entire driver file
@weight(0)
@visibility("visible")
def test_parks_driver_calls_display_function(self):
    """parks_driver: Check that main calls display_neighbourhood_with_most_parks"""
    # read file into a string
    with open(self.expected_filename[0], 'r') as f:
        code = f.read()
    pedal.contextualize_report(code)
    pedal.ensure_function_call('display_neighbourhood_with_most_parks', 1)  # 1 indicates the minimum number of expected calls
    res = pedal.resolvers.simple.resolve()
    self.assertTrue(res.success, res.message)   
```

4. test the output of the entire program (not just one function)
5. test if they use `if __name__ == "__main__"`
