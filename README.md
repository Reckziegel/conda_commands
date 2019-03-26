Conda Commands
================

<!-- README.md is generated from README.Rmd. Please edit that file -->
Installing Packages
-------------------

-   `conda --help`
-   `conda --version`

These commands can be combined to generate something like:

`conda install --help`

To install a specific version of a package type `conda install pkg=version`. Example:

`conda install pandas=0.24.2`

Sometimes may be necessary to remove a package. This can be done with `conda remove pkg`. As with other commands, it's also also possible to specify multiple packages separated by spaces. Conda always tries to use the most recent version of a package that is compatible with others. For this reason, removing one package allows another packages to be upgraded implicitly because only the removed package was requiring the older versions of the dependencies.

Sometimes may be interesting to verify which package versions are available in the current environment. By default `conda search pkg` looks for those matching your platform (although switches allow tweaking this behavior). Example:

-   `conda search pandas`
-   `conda search pandas=0.24.2` \# search for a specific version

Additional information about packages can be accessed with the `conda search pkg --info` command.

-   `conda searh pandas=0.24.2 --info`

Utilizing Channels
------------------

Packages can be installed from specific channels with the following command line:

-   `conda install --channel some-organization pkg`

Example:

-   `conda install --channel conda-forge youtube-dl`

It's possible to check the package installation with `conda list pkg`.

Working with Environments
-------------------------

Conda environments allow for flexible version management of packages.

Conda environments allow multiple incompatible versions of the same (software) package to coexist on your system. An environment is simply a file-path containing a collection of mutually compatible packages. By isolating distinct versions of a given package (and their dependencies) in distinct environments, those versions are all available to work on particular projects or tasks.

For example, you may develop a project comprising scripts, notebooks, libraries, or other resources that depend on a particular collection of package versions. You later want to be able to switch flexibly to newer versions of those packages and to ensure the project continues to function properly before switching wholly. Or likewise, you may want to share code with colleagues who are required to use certain package versions. In this context, an environment is a way of documenting a known set of packages that correctly support your project.

The sub-command `conda env list` displays a list of all environments on the current system; the one that is currently activated is marked with an asterisk.

To verify which package versions are installed in a specific environment type:

-   `conda list`

or for a more specific search type the package names follow by the *regex* `|`:

-   `conda list 'numpy|pandas|scikit-learn'`

The `conda list` command shows the packages in the current environment. To check for packages installed in a different env type

-   `conda list --name env-name pkg-name`.
-   `conda list --name r-reticulate 'numpy|pandas'`

To *activate* an environment, you simply use `conda activate env-name`. To *deactivate* an environment, you use `conda deactivate`, which returns you to the root/base environment. It's also possible to switch between environments. Example:

-   `conda activate env-name1` \# activates env1
-   `conda activate env-name2` \# deactivates env1 and activates env2

From time to time, it is worth cleaning up the environments you have accumulated just to make management easier. The command to remove an environment is:

-   `conda env remove --name env-name`

To create a new environment the command `conda create env` can be used, or a more specific pattern that adds several packages while the environment it's been created. Example:

-   `conda create --name r-reticulate python=3.6 pandas=0.23.4 statsmodels=0.9.0`

Note that the python and the packages versions should be separated by a space.

Using `conda list` provides useful information about the packages that are installed in a specific environment. However, the format it describes packages in is not immediately usable to let a colleague or yourself to recreate exactly the same environment on a different machine. For that you want the `conda env export` command. If you specify `-n` or `--name` you can export an environment other than the active one. Without that switch it chooses the active environment. If you specify `-f` or `--file` you can output the environment specification to a file rather than just to the terminal output. If you are familiar with piping, you might prefer to pipe the output to a file rather than use the --file switch. By convention, the name `environment.yml` is used for environment, but any name can be used (but the extension `.yml` is strongly encouraged).

-   `conda env export --name r-reticulate` \# exports the environment without a file name
-   `conda env export --name r-recitucale --file file_name.yml` \# exports the env with a file name

#### Create an environment from a shared specification

You may recreate an environment from one of the YAML (Yet Another Markup Language) format files produced by `conda env export`. However, it is also easy to hand write an environment specification with less detail. For example, you might describe an environment you need and want to share with colleagues as follows:

    $ cat our-project.yml
    name: our-project
    channels:
      - defaults
    dependencies:
      - python=3.7.1
      - pandas>=0.24.2
      - scikit-learn>=0.20.1
      - statsmodels>=0.9.0

Clearly this version is much less specific than what `conda env export` produces. But it indicates the general tools, with some version specification, that will be required to work on a shared project. Actually creating an environment from this sketched out specification will fill in all the dependencies of those large projects whose packages are named, and this will install dozens of packages not explicitly listed. Often you are happy to have other dependencies in the manner `conda` decides is best.

Of course, a fully fleshed out specification like we saw in the previous exercise are equally usable. Non-relevant details like the path to the environment on the local system are ignored when installing to a different machine—or indeed to a different platform altogether, which will work equally well.

To create an environment from `file-name.yml`, you can use the following command:

-   `conda env create --file file-name.yml`

As a special convention, if you use the plain command `conda env create` without specifying a YAML file, it will assume you mean the file `environment.yml` that lives in the local directory.
