# Setting up your python project like a pro

## Menu
- [Introduction](#introduction)
  - [Requirements](#requirements)
  - [Objectives](#objectives)
- [Create a new repo on GitHub](#create-a-new-repo-on-github)
- [Unit testing, Continuous Integration and Test Driven Development](#unit-testing-continuous-integration-and-test-driven-development)
  - [Writing your first unit test](#writing-your-first-unit-test)
  - [Coverage report](#coverage-report)
  - [Continuous Integration](#continuous-integration)
- [Code Documentation](#code-documentation)

## Introduction
Welcome to this tutorial where we will show you how to set up a new python project like a professional. Although this tutorial relieson very simple code you need to have basic understanding ofpython and g it commands.

### Requirements
- Basic python and command line understanding.
- Make sure you have a GitHub account.
- Have a git client installed (we will use the CLI in this tutorial).
- Have python 3.x installed and an updated pip.
- We recommend using an IDE to follow this tutorial such as pycharm.

### Objectives
- learn how to manage a GitHub repo 
- learn how to implement unit testing, continuous integration and test driven development.
- learn how to document code and share the documentation with users and contributors.

# Let's get started! 

## Create a new repo on GitHub
First you want to head to GitHub. com and create a new project. Enter a project name, check the "Add READ ME file" option, 
and select the python template for the gitignore file. Finally select licence. We suggest:
- MIT for libraries (https://choosealicense.com/licenses/mit/)
- AGPL for applications and services (https://choosealicense.com/licenses/agpl-3.0/)
- CCBY for content and digital assets (https://creativecommons.org/licenses/by/2.0/)

Once done, your repository should contain:
- a README.md file
- a .gitignore file
- a LICENSE file

Before we start working on the code, we first want to protect our main branch from miss manipulation. Head to the 
setting tab of your repo and click on "branch". Click on "Add Rule" next toBranch protection rules and under 
"Branch Name Pattern" enter the name of your main branch. Then check the option "Requires a pull request before merging". 
We suggest getting familiar with this panel as we will come back to it later.


Now that our main branch is protected, we can clone the repository on our local machine and open the project in our IDE. 
Create a new branch and name it "Initial setup":
```shell
git checkout -b InitialSetup
```
Now create and enable a python virtual environment. We use venv:
```shell
python3 -m venv venv
source venv/bin/activate  # for linux
venv\Scripts\activate  # for windows
```

We now want to create a few important files:
- requirements.txt: this file will contain all the production dependencies of our project.
- requirements-dev.txt: this file will contain all the development dependencies of our project. This includes testing,
documentation, linting, etc.
- .flake8: this file will contain the configuration for the flake8 linter.
- .coveragerc: this file will contain the configuration for generating the unit test coverage report.

```shell
touch requirements.txt requirements-dev.txt .flake8 .coveragerc
```


We will not use any specific library to implement our code so the requirements.txt file will be empty. However, we will 
need some development dependencies. Open the requirements-dev.txt file and add the following lines:
```requirements.txt
coverage~=7.1.0
flake8~=6.0.0
docstr-coverage~=2.2.0
sphinx~=6.1.3
sphinx-rtd-theme~=1.2.0
sphinx-rtd-dark-mode~=1.2.4
```
Remember to always pin your dependencies to a specific version. This will ensure that your code will not break if a new
version of a dependency is released. The versions provided here are the latest versions at the time of writing this tutorial
for python 3.11 (22 February 2023).
These dependencies are being used for:
- coverage: generating the unit test coverage report
- flake8: linting
- docstr-coverage: checking the documentation coverage
- sphinx: generating the documentation
- sphinx-rtd-theme: a sphinx theme
- sphinx-rtd-dark-mode: a dark variant of the selected theme

You can now install the dependencies:
```shell
pip install -r requirements-dev.txt
```
Open the .gitignore file and add the following lines .venv/ to it. Add the files to your git index and push to your 
remote repository:
```shell
git add .
git push --set-upstream origin InitialSetup
```

## Unit testing, Continuous Integration and Test Driven Development

Before writing any code, we need to think about how we are going to test it. We could manually test it after each change, but
it is very inefficient and unreliable. It also doesn't work if you are working on a team or if you have external contributors
to your open source project. <br>
We will use unit testing to automate the testing process. Unit testing in the process of making sure classes, functions 
and methods do what they are supposed to. It tests the business logic of the smallest unit of code possible. The idea is 
to import a function, class or method and call it with a set of parameters. Because we know what it does and its inputs,
we can infer what the output should be. We then compare the actual output to the expected output and if they match, the 
test passes. <br>
This process is key to keeping your app maintainable as it grows and provides awareness on breaking modifications, 
preventing them from reaching production. <br>
Test Driven Development (TDD) is a way of programming where tests are written before the code. This may look overwhelming, 
but it forces developers to think about their architecture before they code and not as they are coding (or worse, afterward).

### Writing your first unit test
For the sake of this tutorial we want to use a very simple example and will implement the "two sum" LeetCode function
(https://leetcode.com/problems/two-sum). <br>
The function takes an array of integers and a target integer. It needs to return the indices of the couples that add up to the
target. For example, if the array is [1, 2, 3, 4] and the target is 5, the function should return [0, 3] as 1 + 4 = 5. <br>
We want to create a test that calls our two_sum function and assert the validity of its output. <br>
Let's create a few files and folders:
- `lcpsolver` directory : this folder will contain the code of our project and have the name of our library. Inside this folder, create:
  - `__init__.py`: the root file of our library.
  - `two_sum.py`: the file that will contain the two_sum function.
- `tests` directory: this folder will contain all the unit tests. For the purposes of readability, we want our `tests`
directory to have the same architecture as our applications such as each source file as a corresponding test file.
Inside this folder, create:
  - `test_two_sum.py`: the file that will contain the unit tests for the two_sum function. By convention, the name of the file
needs to start with `test_` so that unittest can `discover` it.

```shell
mkdir lcpsolver tests && touch lcpsolver/__init__.py lcpsolver/two_sum.py tests/test_two_sum.py
```

At that point, this is what yourproject should look like:
```
lcpsolver
├── __init__.py
└── two_sum.py
tests
└── test_two_sum.py
.coveragerc
.flake8
.gitignore
LICENSE
README.md
requirements.txt
requirements-dev.txt
```

You can now run the following commands to execute our suite of unit tests:
```shell
python-m unit test discover -s tests/
```

This will raise an error telling unittest could not find any test suite. Open `tests/test_two_sum.py` and let's get started. <br>
First, import the Testcase class from the unittest library and your two_sum function.
```python
from unittest import TestCase

from lcpsolver.two_sum import two_sum
```

A general tip to handle imports is to separate them into three blocks separated with a white line and order them alphabetically:
- The very first one should import all **built-ins** you will use like os, date time, etc.
- The second one should import all **third party** libraries you will use like numpy, pandas, etc.
- The last one should import all **your own code**. Import **code and logic** first, **data and templates** last.

Always apply this pattern, and you'll never get lost with imports again. Don't forget to separate your imports and the code
with two white lines. <br>

To write a unit test in python you need to create a class that inherits from the TestCase class and, by convention, its name
should start with `Test`. Inside this class, you can create methods that will contain your tests.

```python
from unittest import TestCase

from lcpsolver.two_sum import two_sum


class TestTwoSum(TestCase):
    def test_two_sum(self):
        pass
```

Running this code with the previous command will raise an errorindicating there is nothing named two_sum in our empty 
two_sum.py file, so it can't import it. <br>
Open the file and create an empty two_sum function:
```python
def two_sum():
    pass
```

Rerun the unit test command: everything now passes but doesn't actually do anything. Before we deal with this, 
let's make our function easier to import. Open `lcpsolver/__init__.py` and add the following line:
```python
from .two_sum import two_sum
```

We can now rewrite the unit test to import the two_sum function from the lcpsolver library:
```python
from unittest import TestCase

from lcpsolver import two_sum


class TestTwoSum(TestCase):
    def test_two_sum(self):
        pass
```

Let's now get to the test itself. We want to call the two_sum function with different parameter sand assert the output.
We will use the `assertEqual` and `assertNotEqual` method from the TestCase class to compare the actual output to the expected output. We could
alternatively use the `assertTrue` method but it doesn't provide as much information if the test fails. <br>
```python
from unittest import TestCase

from lcpsolver import two_sum


class TestTwoSum(TestCase):
    def test_two_sum(self):
        self.assertEqual(two_sum([1, 2, 3, 4], 5), [0, 3])
        self.assertEqual(two_sum([2, 7, 11, 15], 9), [0, 3])
        self.assertNotEqual(two_sum([1, 2, 3, 4], 5), [0, 2])
```

Running the test command would result in an error because we are passing two parameters to the two-sum function but the
function doesn't accept any. <br>
Let's go back to the `two_sum.py` file and do a proper implementation:

```python
def two_sum(nums, target):
    hashmap = {}
    for i in range(len(nums)):
        complement = target - nums[i]
        if complement in hashmap:
            return [i, hashmap[complement]]
        hashmap[nums[i]] = i
```

That's the official leet  code one-pass with a hash table. Rerun the test command and
enjoy. <br>

A few notes about the Test `TestCase` class. It provides useful class methods that will let us execute code before and 
after each or all tests. wbr>
The unittest library also provides patch functions and decorators to help you mock library calls (useful when your 
code relies on an external resources, like ReST calls).

Let's now deal with edge cases and error handling. Imagine the inputs of the function comes from a user and could be anything, 
like a string. We want to make sure our function will raise a meaningful error to our end users. <br>
There are two things we can do in our `two_sum` function:
- implement type hints but this is not mandatory and won't be enforced by the interpreter.
- validate the inputs and raise an exception if they are not valid.

Let's do both. Open `lcpsolver/two_sum.py` and add the following lines:
```python
from __future__ import annotations


def two_sum(nums: list[int], target: int) -> list[int] | None:
    if not isinstance(target, (int, float)):
        raise TypeError("two_sum() 'target' must be an integer but got %s" % type(target))
    if not isinstance(nums, list):
        raise TypeError("two_sum() 'nums' must be a list but got %s" % type(nums))
    
    hashmap: dict[int, int] = {}
    for i in range(len(nums)):
        if not isinstance(nums[i], (int, float)):
            raise TypeError("two_sum() 'nums' must be a list of integers but got %s" % nums[i])
        complement = target - nums[i]
        if complement in hashmap:
            return [i, hashmap[complement]]
        hashmap[nums[i]] = i
```

We can now go back to our unit test and make sure that:
- it works with floats
- it raises the correct error when the inputs are not valid

```python
from unittest import TestCase

from lcpsolver import two_sum


class TestTwoSum(TestCase):
    def test_two_sum(self):
        self.assertEqual(two_sum([1, 2, 3, 4], 5), [0, 3])
        self.assertEqual(two_sum([2, 7, 11, 15], 9), [0, 3])
        self.assertNotEqual(two_sum([1, 2, 3, 4], 5), [0, 2])

    def test_two_sum_floats(self):
        self.assertEqual(two_sum([1.00, 2, 3, 4], 5.00), [0, 3])

    def test_two_sum_invalid_inputs(self):
        with self.assertRaises(TypeError) as context:
            two_sum([1, 2, 3, 4], "A")
        self.assertEqual(str(context.exception), "two_sum() 'target' must be an integer but got 'str'")
        with self.assertRaises(TypeError) as context:
            two_sum(["A"], 5)
        self.assertEqual(str(context.exception), "two_sum() 'nums' must be a list of integers but got 'A'")
        with self.assertRaises(TypeError) as context:
            two_sum("A", 5)
        self.assertEqual(str(context.exception), "two_sum() 'nums' must be a list but got 'str'")
```

We highly suggest you to read the [unittest documentation](https://docs.python.org/3/library/unittest.html) 
to learn more about the unittest library and its features. It's important to think of edges cases and unexpected inputs when 
writing your tests. <br>

### Coverage report
In this fairly simple example it isn't hard to figure out which part of the code has been tested and which
hasn't. However, as the code will grow and more people contribute we need a way to track and monitor which functions 
are tested and which aren't. To do this we will use the coverage library that we installed earlier. <br>
To configure the coverage library, open the `.coveragerc` file located at the root of the project (or create it if 
you haven't yet).
```.coveragerc
[run]
source = lcpsolver
relative_files = True

omit = 
  tests/*
```

You can now replace the unit test command with the coverage command:
```shell
coverage run -m unittest discover -s tests  # run the unit tests
coverage report -m  # generate a report
```

These command will generate a report that indicates the name of each source file, the number of total statements,
the number of missed statements, the percent of statements covered and which specific lines have not been tested yet. 
It will also create a file named `.coverage`, that needs to be added to the `.gitignore` file, so it won't be committed 
to the repository. <br>
If you use pycharm pro, you can also use the integrated coverage tool to integrate reports in the IDE. <br>

### Continuous Integration
The idea of what we have been doing is to prevent the introduction of breaking changes into stable branches. Whenever a 
contributor commits code to the repo, we want to run the unit tests, generate a coverage report and ensure nothing fails. <br>
On larger project, it's impossible to do this manually after every pull request. Thus, we are going to automate the 
process through continuous integration, aka continuous and automated testing of source code.
In the case of open source projects we can rely on free services to help use run our builds:
- GitHub actions to run the actual tests build.
- Coveralls.io to version and explore our coverage reports and ensure that the coverage doesn't decrease on each build.

First create a new directory at the root of the project named `.github` and inside it create a new directory named `workflows`.
Then create a YAML file named `ci-build.yml`:
```shell
mkdir .github && mkdir .github/workflows && touch .github/workflows/ci-build.yml
```
This file describes the steps the build needs to go through before success or failure. <br>
We first enter a name for our build and the conditions under which it should be triggered: here on PR and push operations. <br>
We then describe the jobs to be executed. We first enter the python versions to use. Each version in the matrix will trigger an
independent build. Note that the newest versions of python need to be string. <br>
We can now define the steps of our build:
- We first need to pull our code in the machine. Conveniently enough, actions can use other actions which is what we 
are doing here by using `actions/checkout@v2`.
- Let's now install python with yet another integrated action: `actions/setup-python@v2`. This time we need an extra parameter to
indicate the python version is to be pulled from the version matrix.
- We can now install the project dev dependencies. Don't forget to install and update PIP.
- Finally, we can run our tests and generate a coverage report. 

```yaml
name: CI Build

on:
  pull_request:
  push:

jobs:
  build:

    runs-on: ubuntu-latest
    strategy:
      matrix:
        python-version: [3.9, '3.10', '3.11']

    steps:
      - uses: actions/checkout@v2
      - name: Set up Python ${{ matrix.python-version }}
        uses: actions/setup-python@v2
        with:
          python-version: ${{ matrix.python-version }}
      - name: Install dependencies
        run: |
          python -m pip install --upgrade pip
          pip install requirements.txt
          pip install -r requirements-dev.txt
      - name: Test with python unittest
        run: |
          coverage run -m unittest discover -s tests/
          coverage report -m
```

Before we commit and push our code, we need a way to host and version our reports. There are several free services 
available for open source projects and the one I generally use are coverall.io and codacy.com. Head to
coverall.io and login using GitHub OAuth. In the left menu, click `Add Repo`. If nothing appears under the list of available repos,
click `Sync Repos` at the top right. Finally, enable coveralls on the desired projects. <br>
Now let's go back to our build file to add the final missing piece. Remember that `.coverage` file we added to `.gitignore`? Well,
we want to commit that file to coveralls. To do this, we will use yet another action called Andre `Mitras/coveralls-python-
action@develop`. The workflow works in a two steps process as indicated in the documentation. Here is the final
yaml file:
```yaml
name: CI Build

on:
  pull_request:
  push:

jobs:
  build:

    runs-on: ubuntu-latest
    strategy:
      matrix:
        python-version: [3.9, '3.10', '3.11']

    steps:
      - uses: actions/checkout@v2
      - name: Set up Python ${{ matrix.python-version }}
        uses: actions/setup-python@v2
        with:
          python-version: ${{ matrix.python-version }}
      - name: Install dependencies
        run: |
          python -m pip install --upgrade pip
          pip install requirements.txt
          pip install -r requirements-dev.txt
      - name: Test with python unittest
        run: |
          coverage run -m unittest discover -s tests/
          coverage report -m
      - name: Coveralls
        uses: AndreMiras/coveralls-python-action@develop
        with:
          parallel: true
          flag-name: Unit Test

  coveralls_finish:
    needs: build
    runs-on: ubuntu-latest
    steps:
      - name: Coveralls Finished
        uses: AndreMiras/coveralls-python-action@develop
        with:
          parallel-finished: true
```

We are now ready to commit the changes and push them to the remote branch. Go to your GitHub project, select your branch 
and look at the little yellow pill on the left of the commit history link. Click and look at the new entry in the popup. <br>
You can follow the link to your build which leads to the action tab, and inspect the logs. A failing build will be 
indicated by a red cross and a successful build by a green tick. If the build is successful, a new incoming check should 
now tell you your report as been correctly updated on coverall and the coverage percentage of the current branch. <br>

Before we create a PR, we want to report the status of our build and unit tests coverage in the README file. To do this. <br>
Go back to the branch protection rule we created at the beginning of the's tutorial and click the `Requires status
checks to pass before merging` and `Require branches to be up to date before merging` options. Then, in the search box 
enter `build` and select the build we created. Clear, enter `coveralls` and select that one as well. Then, save your 
changes at the bottom of the page. <br>
Now go to the `Actions` tab of the repo and select the latest workflow. At the top right, click the 3 dots and `Create a
status badge`. Select the main branch as the branch and copy the generated markdown at the top of your README file, below
your title and logo. <br>
Go to coveralls and select your repository. Next to the status badge click "Embed" and copy the makdown code in
your README file next to the build one. <br>
Commit your code, push your local branch to the remote one and open a PR. Now wait a few minutes. <br>

That's it. Merge the PR: we are done with our CI infrastructure. <br>


## Code Documentation

As a developer, when using a library, you expect a documentation. This doc can serve a dual purpose as it also informs
contributors about what each piece of code is doing. If you are a python dev you are probably already familiar 
with `readthedocs`. It's okay if you are not, it just a web service to host code documentation. <br>
It relies on a documentation framework called `Sphinx`. I personally do not like the default bright theme, so I have
installed `sphinx-rtd-theme` and `sphinx-rtd-dark-mode`. (RTD stands for Read The Docs). <br>

Before we can start writing documentation, we need to initiate `Sphinx` with the following command:
```bash
sphinx-quickstart docs/
```

This will prompt you with a few questions:
- `Separate source and build`: yes
- `Project name`: My awesome project name.
- `Author`: Your name
- `Release`: empty
- `Language`: usually en

You now have a new directory named `docs/` at the root of your project. Inside that directory you will find a `source` 
directory (this is where we will write import our code documentation and write examples).
It contains a `conf.py` file used as the sphinx configuration. Open it and complete the fields as:
```python
extensions = [
    'sphinx.ext.autodoc',
    'sphinx.ext.doctest',
    'sphinx_rtd_dark_mode'
]
html_theme = 'sphinx_rtd_theme'
default_dark_mode = True
html_css_files = ['custom.css']
```

The `source` dir contains a few other files we will soon go back to. <br>
Our local documentation is now properly configured, but we didn't write doc yet. To do this we are going to follow the `PEP
257` specification and its implement called `docstring`. This serves a dual purpose:
- Generate an online HTML documentation
- Generate inputs parsable by IDEs and `man`/`help` commands to generate visual help.

A docstring is a comment wrapped into three double quotes. You will generally find them at the top of a file
(describes the module) and right after each function/method declaration or before a class constructor.
They give information on the function and methods signature (what are the inputs and outputs and what are their types) 
and describe what they do. <br>

Let's go back to the code, so we can see this in practice. Open the source file and the unit test file in an IDE. 
In the unit test, over the `TestCase` class with your cursor: a description should appear in the tool tip, 
describing the class and its purpose. <br>
If you hover over the `two_sum()` function, you should only see the function signature and no description. The signature
is available because we have added type hints to the function. <br>
Now, let's add a docstring to the function. The docstring should be right after the function declaration:
```python
from __future__ import annotations


def two_sum(nums: list[int], target: int) -> list[int] | None:
    """ The two sum function returns the indices of the two numbers first numbers such that they add up to target
    
    :param nums: a list of integers
    :param target: the target sum
    
    :return: a list of two integers representing the indices of the two numbers
    """
    if not isinstance(target, (int, float)):
        raise TypeError("two_sum() 'target' must be an integer but got %s" % type(target))
    if not isinstance(nums, list):
        raise TypeError("two_sum() 'nums' must be a list but got %s" % type(nums))
    
    hashmap: dict[int, int] = {}
    for i in range(len(nums)):
        if not isinstance(nums[i], (int, float)):
            raise TypeError("two_sum() 'nums' must be a list of integers but got %s" % nums[i])
        complement = target - nums[i]
        if complement in hashmap:
            return [i, hashmap[complement]]
        hashmap[nums[i]] = i
```

There is a wide range of attributes docstring supports; we highly suggest getting familiar with its documentation and
specification. <br> 
Go back to your test file, over the `two_sum()` function to now see a full detail of the documentation. You can also run 
the `help(two_sum)` in a python console to display your documentation. <br>

Inside the `docs/source` directory create a new directory with the name of your module (`lcpsolver`) and, inside, 
create a `two_sum.rst` file (RST stands for ReStructured which is an extended Markdown format).
```bash
mkdir docs/source/lcpsolver && touch docs/source/lcpsolver/two_sum.rst
```
Open the file, add a title and call the `automodule` directive with the full name of our module:
```rst
# Two_sum documentation

.. automodule:: lcpsolver.two_sum
    :members:
```
Now, in `docs/source/index.rst` add the new file to the tree of content (TOC):
```rst
.. toctree::
   :maxdepth: 2
   :caption: Contents:
   :titlesonly:
   :numbered:

   lcpsolver/two_sum
```

Before we generate a documentation, we want to remove the `docs/build` directory from versioning by adding it
to the `.gitignore` file. The `build` directory will contain the HTML version of our documentation and will be managed 
by `readthedocs`. Open a terminal and run the command (for windows):
```bash
.\docs\make.bat html
```

Congratulations! Under `/docs/build` you now have a fully working HTML documentation. Open the `index.html` in your 
browser and check it out. <br>

We can now use read the-docs to synchronise our GitHub repository with a documentation website. Head to the website
and login with your GitHub account.