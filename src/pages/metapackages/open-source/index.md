---
title: Install the Open Source metapackage
---

# Install the Open Source metapackage

You must install this metapackage in all projects using PWA Studio 12.1.0 and higher.

## Prerequisite

-  Magento Open Source or Adobe Commerce installation.

## Installation as a git-based composer package

To set up and develop your PWA extension modules locally, use the following instructions.

1. Clone the `magento2-pwa` repository into your vendor directory name:

    ```bash
    git clone git@github.com:magento-commerce/magento2-pwa.git ext/magento/magento2-pwa
    ```

1. Update the `magento2/composer.json` settings to create a better development workflow for your extension modules:

    -  Update the `minimum-stability` for packages to `dev`. This allows for the installation of development modules:

        ```bash
        composer config minimum-stability dev
        ```

    -  To work with `stable` packages, ensure that the `prefer-stable` property is `true`. This property should already be included in the `composer.json` file, right above the `minimum-stability` setting.

    -  Configure `composer` to find new extension modules. The following command configures `composer` to treat any extension code inside the `ext` directory as a package and creates a symlink to the `vendor` directory:

        ```bash
        composer config repositories.ext path "./ext/*/*/*"
        ```

1. Install the `pwa` metapackage:

    ```bash
    composer require magento/pwa
    ```

At this point, you should see symlinks for all the `pwa` modules inside the `vendor` directory. These symlinks allow you to:

-  Run a Magento installation with additional modules.
-  Develop locally using the standard git workflow.

You may need to ensure that there are no `Magento_PWA*` modules listed as `enabled` when you run `bin/magento module:status`. If there are, [follow the docs on how to enable modules](https://devdocs.magento.com/guides/v2.4/extension-dev-guide/build/enable-module.html).

## Setting up the Git workflow

To improve the developer experience even further, you can add these configurations as well:

1. Exclude all the `ext` directories in the project's `.git` configuration:

    ```bash
    echo ext >> ./.git/info/exclude
    ```

1. Skip your project's root directory `composer.\*` files to avoid committing them by mistake:

    ```bash
    git update-index --skip-worktree composer.json && git update-index --skip-worktree composer.lock
    ```

    **NOTE:** You can reverse this operation anytime as needed:

    ```bash
    git update-index --no-skip-worktree composer.json && git update-index --no-skip-worktree composer.lock
    ```

## Cloud deployment extension installation

1. Add https://repo.magento.com as a composer repository by adding the following to the `composer.json` file of your cloud instances.

    ```json
    "repositories": {
        "repo": {
            "type": "composer",
            "url": "https://repo.magento.com"
        }
    },
    ```

1. Require in `magento/magento2-pwa` extension by adding the following to the `composer.json` file of your cloud instances.

    ```json
    "require": {
        "magento/magento2-pwa": "0.0.1"
    },
    ```

1. Ensure your `auth.json` file has valid credentials for `repo.magento.com`.

1. Run `composer update` to update your `composer.lock` file.

1. Push the changes and deploy your cloud instance.
