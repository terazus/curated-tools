# PYTHON
A list of curated tools and libraries used for development in python

## Pycharm plugins:
- .env file support (https://github.com/adelf/idea-php-dotenv-plugin/): provides support for .env files.
- EduTools (https://www.jetbrains.com/academy/): provides a feature to create classes integrated in jetbrains IDEs.
- ExcelReader (https://github.com/obiscr/ExcelReader): provides support for reading excel files.
- Copilot (https://plugins.jetbrains.com/plugin/17718-github-copilot): the github copilot module for code autocompletion and suggestion. Requires a github pro account (free for students).
- GraphQL (https://plugins.jetbrains.com/plugin/8097-graphql): provides a UI to interact and query graphQL endpoints.
- LNKD (https://plugins.jetbrains.com/plugin/12802-lnkd-tech-editor): provides supports for RDF data (specifically turtle).
- outSPARQL (https://plugins.jetbrains.com/plugin/16503-outsparql): provides supports for SPARQL quering.
- Python: Styled Dataframe Viewer (https://plugins.jetbrains.com/plugin/16050-python-styled-dataframe-viewer): provides the ability to preview styled pandas dataframes during debug.
- R Language for IntelliJ (https://plugins.jetbrains.com/plugin/6632-r-language-for-intellij): provides supports for R (R need to be installed)
- Requirements (https://plugins.jetbrains.com/plugin/10837-requirements): provides supports for python requirements files
- ReST Console (https://plugins.jetbrains.com/plugin/8114-rest-console): provides a psotman style ReST interface to interact with web APIs.
- Snyk (https://plugins.jetbrains.com/plugin/10972-snyk-security--code-open-source-container-iac-configurations): provide security insights on your code (requires an account on https://snyk.io/).


## Testing:
- unittest (https://docs.python.org/3/library/unittest.html): builtin libreary for unittesting
- nosetests (https://nose.readthedocs.io/en/latest/): an extension built on top of unittest
- coverage (https://coverage.readthedocs.io/en/7.1.0/): a library to report unit test coverage
- coverall.io (https://coveralls.io/): a service to host coverage report. Can be integrated in your CI pipeline.
- github actions (https://coveralls.io/): run your CI builds directly in github

## Documentation:
- Sphinx (https://www.sphinx-doc.org/en/master/): a framework to generate library documentations based on docstrings
- Jupyter Book (https://jupyterbook.org/en/stable/intro.html): a sphinx distribution that also supports jupyter notebooks.
- Sphinx themes (https://sphinx-themes.org/): a list of curated themes for Sphinx.
- Sphinx contrib (https://github.com/sphinx-contrib/): a directory of sphinx modules that extend the framework.
- flasgger (https://github.com/flasgger/flasgger): a Flask extension to generate SWAGGER documentations of ReST APIs.
- docstr-coverage (https://github.com/HunterMcGushion/docstr_coverage): a library to report docstring coverage.
- readthedocs (): a service to host python code documentation, in particular when generated with sphinx. Synchronises documentation with the corresponding code version.

## Linting / Type Hints:
- flake8 (https://flake8.pycqa.org/en/latest/): a library to check PEP8 compliance of your code.
- mypy (https://mypy.readthedocs.io/en/stable/): a library to verify type hints cohesion of your code. May require some specific stub imports:
  - data-science-types (https://github.com/wearepal/data-science-types): for numpy and other scientific libraries such as matplotlib and pandas.
  - pandas-stubs (https://pypi.org/project/pandas-stubs/): for pandas
  - sqlalchemy-stubs (https://pypi.org/project/sqlalchemy-stubs/): for SQLAlchemy
- codacy (https://www.codacy.com/): Provides very usefull and intehrated devops features for software lifecycle (code coverage, linting, pattern analysis, ...).
