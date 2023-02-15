# Contributing

Contributions are welcome, and they are greatly appreciated! Every little bit
helps, and credit will always be given.

You can contribute by reporting or fixing bugs, requesting or implementing
features, or writing documentation.

## Report bugs

Report bugs at https://github.com/Enodis/python-flow/issues.

If you are reporting a bug, please include:

* Your operating system name and version.
* Any details about your local setup that might be helpful in troubleshooting.
* Detailed steps to reproduce the bug.

## Fix bugs

Look through the GitHub issues for bugs. Anything tagged with "bug" and "help
wanted" is open to whoever wants to implement it.

## Request features

The best way to send feedback is to file an issue at https://github.com/Enodis/python-flow/issues.

If you are proposing a feature:

* Explain why the feature is worth having. What are we loosing out on without it?
* Explain in detail the expected behavior.
* Keep the scope as narrow as possible, to make it easier to implement.
* Remember that this is a volunteer-driven project, and that contributions
  are welcome :)

## Implement features

Look through the GitHub issues for features. Anything tagged with "enhancement"
and "help wanted" is open to whoever wants to implement it.

## Write documentation

Flow could always use more documentation, whether as part of the user guides,
examples, reference manual, or even on the web in blog posts, articles, and
such. If you do create material on Flow, please drop us a line and we'll add a
link.

## Get started

Ready to contribute? Here's how to set up `python-flow` for local development.
Make sure you have *Python 3.8* or newer and *pipenv* installed.

1. Fork the `python-flow` repo on GitHub.
2. Clone your fork locally::

    ```sh
    $ git clone git@github.com:your_name_here/python-flow.git
    ```
3. Set-up your local virtual environment:

    ```sh
    $ cd python-flow/
    $ pipenv sync -d
    ```

4. Create a branch for local development:

    ```sh
    $ git checkout -b name-of-your-bugfix-or-feature
    ```

   Now you can make your changes locally.

5. When you're done making changes, check that your changes pass *flake8*,
   *mypy*, and *pytest*:

    ```sh
    $ flake8 flow
    $ mypy flow
    $ pytest
    ```

6. Commit your changes and push your branch to GitHub::

    ```sh
    $ git add .
    $ git commit -m "Your detailed description of your changes."
    $ git push origin name-of-your-bugfix-or-feature
    ```

7. Submit a pull request through the GitHub website.

## Pull request guidelines

Before you submit a pull request, check that it meets these guidelines:

1. The pull request should include tests.
2. If the pull request adds functionality, the docs should be updated. This
   means that reference documentation should be updated and user guides should
   be updated or created.
3. Make sure that the tests pass for all supported Python versions.
