# octoprint-venv-tool

A tool to help with various tasks surrounding OctoPrint venvs.

[![Demo of octoprint-venv-tool recreate-venv](https://asciinema.org/a/5NpMS3PfEsGphqO2PJ4ZP0LiE.svg)](https://asciinema.org/a/5NpMS3PfEsGphqO2PJ4ZP0LiE)

## Installation

### Linux & other POSIX systems

```
curl -LO https://get.octoprint.org/octoprint-venv-tool && chmod +x octoprint-venv-tool
```

It should then be callable via `./octoprint-venv-tool`.

### Windows

```
curl -LO https://get.octoprint.org/octoprint-venv-tool
```
Then use with `python octoprint-venv-tool`.

## Usage

<!--INSERT:help-->
```
usage: octoprint-venv-tool [-h] [--verbose]
                           {export-plugins,install-plugins,create-venv,recreate-venv} ...

Various tools for OctoPrint's venvs

positional arguments:
  {export-plugins,install-plugins,create-venv,recreate-venv}
    export-plugins      export a list of all OctoPrint plugins installed into
                        the venv that are available on the repo.
    install-plugins     install plugins from an export into a provided venv.
    create-venv         create an OctoPrint venv, installing an optional
                        plugin export
    recreate-venv       recreate an OctoPrint venv, attempting to migrate all
                        plugins installed therein

options:
  -h, --help            show this help message and exit
  --verbose             verbose output
```
<!--/INSERT:help-->

Also see the list of [common workflows](#common-workflows) below.

### `export-plugins`

Export a list of all OctoPrint plugins installed into the venv that are available on the repo.

The venv does not have to be functional for this anymore.

<!--INSERT:export-plugins-->
```
usage: octoprint-venv-tool export-plugins [-h] [--output OUTPUT] venv

positional arguments:
  venv                 path of the venv

options:
  -h, --help           show this help message and exit
  --output, -o OUTPUT  optional path for the export, if unset stdout will be
                       used
```
<!--/INSERT:export-plugins-->

#### Example

```
$ ./octoprint-venv-tool export-plugins --output plugins.json ~/oprint
```

### `install-plugins`

Install plugins from an export into a provided venv.

<!--INSERT:install-plugins-->
```
usage: octoprint-venv-tool install-plugins [-h] [--ignore-plugin-errors]
                                           export venv

positional arguments:
  export                path of the export
  venv                  path of the venv

options:
  -h, --help            show this help message and exit
  --ignore-plugin-errors
                        Continue if trying to install a plugin raises an error
```
<!--/INSERT:install-plugins-->

#### Example

```
$ ./octoprint-venv-tool install-plugins plugins.json ~/oprint
```

### `create-venv`

Create an OctoPrint venv, installing an optional plugin export.

<!--INSERT:create-venv-->
```
usage: octoprint-venv-tool create-venv [-h] [--export EXPORT]
                                       [--python PYTHON]
                                       [--ignore-plugin-errors]
                                       venv

positional arguments:
  venv                  path of the venv

options:
  -h, --help            show this help message and exit
  --export EXPORT       path of the export, optional
  --python PYTHON       python binary to use for creating the venv, optional,
                        if not provided the version used to run the script
                        will be used
  --ignore-plugin-errors
                        Continue if trying to install a plugin raises an error
```
<!--/INSERT:create-venv-->

#### Example

```
$ ./octoprint-venv-tool create-venv ~/new-venv --python=/usr/bin/python3.12 --export plugins.json
```

### `recreate-venv`

Recreate an OctoPrint venv, attempt to migrate all plugins.

The venv does not have to be functional for this anymore.

<!--INSERT:recreate-venv-->
```
usage: octoprint-venv-tool recreate-venv [-h] [--python PYTHON]
                                         [--ignore-plugin-errors]
                                         venv

positional arguments:
  venv                  path of the venv

options:
  -h, --help            show this help message and exit
  --python PYTHON       python binary to use for creating the venv, optional,
                        if not provided the version used to run the script
                        will be used
  --ignore-plugin-errors
                        Continue if trying to install a plugin raises an error
```
<!--/INSERT:recreate-venv-->

#### Example

```
$ ./octoprint-venv-tool recreate-venv ~/oprint --python=/usr/bin/python3.12
```

## Common workflows

### Migrating a venv to a newer Python version

If migrating to a newer Python version, make sure you already have that installed in your system. Be aware that the tool will only
support Python versions >= 3.7.

Then run 

    octoprint-venv-tool recreate-venv /path/to/your/venv --python /path/to/python

substituting `/path/to/your/venv` with the path to your OctoPrint venv and `/path/to/python` with the path to your python 
*executable*, e.g. `/usr/bin/python3.12`.

> [!NOTE]
> If any of your currently installed plugins will cause an error while attempting to install them into the new virtual environment,
> recreation will be aborted and the initial backup of your original venv rolled back.
>
> You can ignore errors caused by plugins and make the recreation continue without installing problematic plugins by running
> the recreation command with the `--ignore-plugin-errors` parameters, e.g.
>
>     octoprint-venv-tool recreate-venv /path/to/your/venv --python /path/to/python --ignore-plugin-errors
>
> The same parameter is also available on the `create-venv` and `install-plugins` commands.

### Recreating a corrupted venv

If you need to recreate a corrupted venv, it might be a good time to also update to a newer Python version. But you can also use
the one you already are using. In most cases, that should be the default Python 3 version installed on your system, so something
like `/usr/bin/python3` on Linux and other POSIX compatible systems. You can run `/usr/bin/python3 --version` to verify that
has at least version 3.7.

Then follow the migration steps outlined above.

### Fetching an export of all of the plugins installed into the venv

Run 

    octoprint-venv-tool export-plugins --output plugin-export.json /path/to/venv
    
substituting `/path/to/venv` with the path to your OctoPrint venv. 

That will create a `plugin-export.json` in your current folder that can be installed through
OctoPrint's plugin manager, or via `octoprint-venv-tool install-plugins`.

### Creating a fresh OctoPrint venv

You can also use `octoprint-venv-tool` to create a fresh venv with OctoPrint and optionally some plugins from a valid export
already preinstalled.

For that, figure out the Python binary you want to use, then run 

    octoprint-venv-tool create-venv --python /path/to/python --export plugin-export.json /path/to/venv

substituting the paths accordingly.
