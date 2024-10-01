# octoprint-venv-tool

A tool to help with various tasks surrounding OctoPrint venvs.

## Installation

```
curl -LO https://get.octoprint.org/octoprint-venv-tool && chmod +x octoprint-venv-tool
```

## Usage

<!--INSERT:help-->
```
usage: octoprint-venv-tool [-h] [--verbose]
                           {export-plugins,install-plugins,create-venv,recreate-venv}
                           ...

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

### `export-plugins`

Export a list of all OctoPrint plugins installed into the venv that are available on the repo.

The venv does not have to be functional for this anymore.

<!--INSERT:export-plugins-->
```
usage: octoprint-venv-tool export-plugins [-h] [--output OUTPUT] venv

positional arguments:
  venv                  path of the venv

options:
  -h, --help            show this help message and exit
  --output OUTPUT, -o OUTPUT
                        optional path for the export, if unset stdout will be
                        used
```
<!--/INSERT:export-plugins-->

#### Example

```
$ ./octoprint-venv-tool export-plugins ~/oprint plugins.json
```

### `install-plugins`

Install plugins from an export into a provided venv.

<!--INSERT:install-plugins-->
```
usage: octoprint-venv-tool install-plugins [-h] export venv

positional arguments:
  export      path of the export
  venv        path of the venv

options:
  -h, --help  show this help message and exit
```
<!--/INSERT:install-plugins-->

#### Example

```
$ ./octoprint-venv-tool install-plugins ~/oprint plugins.json
```

### `create-venv`

Create an OctoPrint venv, installing an optional plugin export.

<!--INSERT:create-venv-->
```
usage: octoprint-venv-tool create-venv [-h] [--export EXPORT]
                                       [--python PYTHON]
                                       venv

positional arguments:
  venv             path of the venv

options:
  -h, --help       show this help message and exit
  --export EXPORT  path of the export, optional
  --python PYTHON  python binary to use for creating the venv, optional, if
                   not provided the version used to run the script will be
                   used
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
usage: octoprint-venv-tool recreate-venv [-h] [--python PYTHON] venv

positional arguments:
  venv             path of the venv

options:
  -h, --help       show this help message and exit
  --python PYTHON  python binary to use for creating the venv, optional, if
                   not provided the version used to run the script will be
                   used
```
<!--/INSERT:recreate-venv-->

#### Example

```
$ ./octoprint-venv-tool recreate-venv ~/oprint --python=/usr/bin/python3.12
```
