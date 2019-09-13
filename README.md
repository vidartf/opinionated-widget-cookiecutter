# opinionated-widget-cookiecutter

A [cookiecutter](https://github.com/audreyr/cookiecutter) template for a custom
Jupyter widget project.

## What is opinionated-widget-cookiecutter?

With **opinionated-widget-cookiecutter** you can create a custom Jupyter
interactive widget project with sensible defaults. opinionated-widget-cookiecutter helps custom widget authors get started
with best practices for the packaging and distribution of a custom
Jupyter interactive widget library.

## Usage

Install [cookiecutter](https://github.com/audreyr/cookiecutter):

    $ pip install cookiecutter

After installing cookiecutter, use opinionated-widget-cookiecutter:

    $ cookiecutter https://github.com/vidartf/opinionated-widget-cookiecutter.git

As the cookiecutter runs, you will be asked for basic information about
your custom Jupyter widget project. You will be prompted for the following
information:

- `author_name`: your name or the name of your organization,
- `author_email`: your project's contact email,
- `github_project_name`: name of your custom Jupyter widget's GitHub repository,
- `github_organization_name`: name of your custom Jupyter widget's GitHub user or organization,
- `python_package_name`: name of the Python "back-end" package used in your custom widget.
- `npm_package_name`: name for the npm "front-end" package holding the JavaScript
  implementation used in your custom widget.
- `npm_package_version`: initial version of the npm package.
- `project_short_description` : a short description for your project that will
  be used for both the "back-end" and "front-end" packages.

After this, you will have a directory containing files used for creating a
custom Jupyter widget. To check that everything is set up as it should be,
you should run the tests:

```bash
# First install the python package. This will also build the JS packages.
pip install -e ".[test, examples]"

# Run the python tests. This should not give you a few sucessful example tests
py.test

# Run the JS tests. This should again, only give TODO errors (Expected 'Value' to equal 'Expected value'):
npm test
```

When developing your extensions, you need to manually enable your extensions with the
notebook / lab frontend. For lab, this is done by the command:

```
jupyter labextension install .
```

For classic notebook, you can run:

```
jupyter nbextension install --sys-prefix --symlink --overwrite --py <your python package name>
jupyter nbextension enable --sys-prefix --py <your python package name>
```

Note that the `--symlink` flag doesn't work on Windows, so you will here have to run
the `install` command every time that you rebuild your extension. For certain installations
you might also need another flag instead of `--sys-prefix`, but we won't cover the meaning
of those flags here.


## Releasing your initial packages

- Do a develop install (see above)
- Add tests
- Ensure tests pass locally and on CI. Check that the coverage is reasonable.
- Make a release commit, where you prepare for release by running:
  ```bash
  pip install bumpversion
  bumpversion release
  ```
- Update the version in `package.json`
- Release the npm packages:
  ```bash
  npm login
  npm publish
  ```
- Bundle the python package: `python setup.py sdist bdist_wheel`
- Publish the package to PyPI:
  ```bash
  pip install twine
  twine upload dist/<python package name>*
  ```
- Tag the release commit (`git tag <python package version identifier>`)
- Go back to dev version (`bumpversion patch`).
- Commit the changes.
- `git push` and `git push --tags`.
