# Fluid Documentation Generator

This package generates an automatic [Fluid]() ViewHelper reference documentation in
[RST format](https://en.wikipedia.org/wiki/ReStructuredText). The included CLI command
is configured with json files. Based on this configuration, the following assets are generated:

* a directory structure with RST files to navigate between namespaces, groups of ViewHelper
as well as the ViewHelpers themselves
* a json file which contains all relevant information about the Fluid namespace and its
ViewHelpers

The result can be rendered with [render-guides](https://github.com/TYPO3-Documentation/render-guides),
which contains a special RST directive to interpret the generated JSON file.

## Installation

This package needs to be installed **inside** a composer project that contains all ViewHelpers
that should be documented.

```sh
composer req --dev t3docs/fluid-documentation-generator
```

## Configuration and Usage

The documentation assets will be generated by using the following CLI command:

```sh
vendor/bin/fluidDocumentation generate viewhelpers1_config.json viewhelpers2_config.json ...
```

You can specify 1-n config files, separated by a space character. If you specify multiple files,
their order is important because it will determine the order on the documentation's index page.
You can use wildcards as well, see examples below.

Each configuration file must respect [the provided JSON schema](./src/Config.schema.json). A
minimal configuration file could look like this:

```json
{
    "name": "MyPackage",
    "namespaceAlias": "my",
    "targetNamespace": "http://typo3.org/ns/Vendor/MyPackage/ViewHelpers"
}
```

The resulting JSON file will contain all ViewHelpers in the PHP namespace `Vendor\MyPackage\ViewHelpers`.
The result will be placed in a folder called `fluidDocumentationOutput`. In this example, the folder
structure will look like this:

* fluidDocumentationOutput/
    * MyPackage/
        * Index.rst
        * MyViewHelper.rst
        * ...
    * Index.rst
    * MyPackage.json

## Examples

Generate ViewHelper reference for `dev-main` of [TYPO3 Core](https://github.com/typo3/typo3) with [config files](./config/typo3/):

```sh
git clone git@github.com:TYPO3/typo3.git
composer install
composer require --dev t3docs/fluid-documentation-generator
vendor/bin/fluidDocumentation generate vendor/t3docs/fluid-documentation-generator/config/typo3/*
```

Generate ViewHelper reference for `dev-main` of [Fluid Standalone](https://github.com/TYPO3/Fluid) with [config files](./config/fluidStandalone/):

```sh
git clone git@github.com:TYPO3/Fluid.git
composer install
composer require --dev t3docs/fluid-documentation-generator
vendor/bin/fluidDocumentation generate vendor/t3docs/fluid-documentation-generator/config/fluidStandalone/viewhelpers_fluid.json
```
