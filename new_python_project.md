# Setting up your python project like a pro

## Introduction:
Welcome to this tutorial where we will show you how to set up a new python project like a professional. Although this tutorial relieson very simple code you need to have basic understanding ofpython and g it commands.

### Requirements:
- Basic python and command line understanding.
- Make sure you have a GitHub account.
- Have a git client installed (we will use the CLI in this tutorial).
- Have python 3.x installed and an updated pip.
- We recommend using an IDE to follow this tutorial such as pycharm.

### Objectives:
- learn how to manage a GitHub repo 
- learn how to implement unit testing, continuous integration and test driven development.
- learn how to document code and share the documentation with the end users.

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
mkdir lcpsolver tests
touch lcpsolver/__init__.py lcpsolver/two_sum.py tests/test_two_sum.py
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