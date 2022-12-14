# PHP CodeSniffer GitHub Action

This action will help you to run phpcs (PHP_CodeSniffer) with [GitHub Actions](https://github.com/features/actions) platform. It also supports [annotations](https://help.github.com/en/github/collaborating-with-issues-and-pull-requests/about-status-checks#checks) out of the box. 

<img src="https://leonardo.osnova.io/491e4ce9-72d9-9417-29f7-9934ce7ec8ad/" alt="How Annotations Works" title="How Annotations Works" width="560" height="432" />

## Usage

Add the following code to `.github/workflows/phpcs.yml` file.

```yaml
name: PHPCS check

on: pull_request

jobs:
  phpcs:
      name: PHPCS
      runs-on: ubuntu-latest
      steps:
        - uses: actions/checkout@v2
        - name: Setup PHP
          uses: shivammathur/setup-php@v2
          with:
            php-version: 8.1
            tools: composer:v2
        - name: Install Dependencies
          run: composer install
        - name: PHPCS check
          uses: navarr/phpcs-action@v3
```

If you want to check only files changed in the PR (**NOTE**: This requires checkout with depth 0)

```yaml
        - name: Checkout
          uses: actions/checkout@v2
          with:
            fetch-depth: 0
        ...
        - name: PHPCS check
          uses: navarr/phpcs-action@v3
          with:
            only_changed_files: true
```

Or, if you want to limit it even further, you can report on only changed lines:

```yaml
        - name: Checkout
          uses: actions/checkout@v2
          with:
            fetch-depth: 0
        ...
        - name: PHPCS check
          uses: navarr/phpcs-action@v3
          with:
            only_changed_lines: true
```

To enable phpcs warnings:

```yaml
        ...
        - name: PHPCS check
          uses: navarr/phpcs-action@v3
          with:
            enable_warnings: true
```

You probably would like to have [configuration file](https://github.com/squizlabs/PHP_CodeSniffer/wiki/Advanced-Usage#using-a-default-configuration-file) for PHP_CodeSniffer in order to make it work as you like.

#### `installed_paths` and `Dealerdirect/phpcodesniffer-composer-installer`
If you are using custom standards, you have two options on how to customize this action.

1. Just use [special library](https://github.com/Dealerdirect/phpcodesniffer-composer-installer) which will find and activate all the standards you have in your `composer.json`.

It will also change phpcs `installed_paths` setting, but will prefer local phpcs install. That means we should use local phpcs binary in the action. To do so run action with certain parameter.

```yaml
        ...
        - name: Install dependencies
          run: composer install --dev --prefer-dist --no-progress --no-suggest
        - name: PHPCS check
          uses: navarr/phpcs-action@v3
          with:
            phpcs_bin_path: './vendor/bin/phpcs'
```

2. Change the `installed_paths` directly by using another option.

```yaml
        ...
        - name: PHPCS check
          uses: navarr/phpcs-action@v3
          with:
            installed_paths: '${{ github.workspace }}/vendor/phpcompatibility/php-compatibility'
```
