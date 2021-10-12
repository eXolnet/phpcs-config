# eXolnet - PHPCS Ruleset

[![Software License](https://img.shields.io/badge/license-MIT-8469ad.svg?style=flat-square)](LICENSE.md)
[![Build Status](https://img.shields.io/travis/eXolnet/code-quality-tools/master.svg?style=flat-square)](https://travis-ci.org/eXolnet/code-quality-tools)

A collection of shareable configurations for various coding-style tools to make the configurations consistent across our projects.

## Setup

1. `composer require --dev exolnet/phpcs-config`
2. Run the following command to run the standards checks:

```
vendor/bin/phpcs --standard=vendor/exolnet/phpcs-config .
```

You can add this to your Travis YAML file as a test:

```yaml
script:
  - phpunit
  - vendor/bin/phpcs --standard=vendor/exolnet/phpcs-config .
```

### Excluding Files

This standard includes special support for a `.phpcsignore` file (in the future, this should be [built into phpcs itself](https://github.com/squizlabs/PHP_CodeSniffer/issues/1884)). 
Simply place a `.phpcsignore` file in your root directory (wherever you're going to run `phpcs` from).

The format of this file is similar to `.gitignore` and similar files: one pattern per line, comment lines should start with a `#`, and whitespace-only lines are ignored:

```
# Exclude our tests directory.
tests/

# Exclude any file ending with ".inc"
*\.inc
```

Note that the patterns should match [the PHP_CodeSniffer style](https://github.com/squizlabs/PHP_CodeSniffer/wiki/Advanced-Usage#ignoring-files-and-folders): `*` is translated to `.*` for convenience, but all other characters work like a regular expression.

Patterns are relative to the directory that the `.phpcsignore` file lives in. On load, they are translated to absolute patterns: e.g. `*/tests/*` in `/your/dir/.phpcsignore` will become `/your/dir/.*/tests/.*` as a regular expression. **This differs from the regular PHP_CodeSniffer practice.**


### Advanced/Extending

If you want to add further rules (such as laravel-specific rules), you can create your own custom standard file (e.g. `ruleset.xml`):

```xml
<?xml version="1.0"?>
<ruleset>
    <file>app</file>
    <file>config</file>
    <file>database</file>
    <file>routes</file>
    <file>tests</file>
    
    <!-- Use eXolnet Coding Standards -->
    <rule ref="vendor/exolnet/phpcs-config" />
    
    <!-- Add Laravel-specific rules -->
    <rule ref="PSR1.Classes.ClassDeclaration.MissingNamespace">
        <exclude-pattern>database/*</exclude-pattern>
    </rule>
</ruleset>
```

You can then reference this file when running phpcs:

```
vendor/bin/phpcs --standard=ruleset.xml .
```


#### Excluding/Disabling Checks

You can also customise the rule to exclude elements if they aren't applicable to the project:

```xml
<rule ref="vendor/exolnet/phpcs-config">
	<!-- Disable short array syntax -->
	<exclude name="PSR1.Classes.ClassDeclaration.MissingNamespace" />
</rule>
```

Rules can also be disabled inline. [phpcs rules can be disabled](https://github.com/squizlabs/PHP_CodeSniffer/wiki/Advanced-Usage#ignoring-parts-of-a-file) with a `// @codingStandardsIgnoreLine` comment, and [ESLint rules can be disabled](http://eslint.org/docs/user-guide/configuring#disabling-rules-with-inline-comments) with a `/* eslint disable ... */` comment.

To find out what these codes are, specify `-s` when running `phpcs`, and the code will be output as well. You can specify a full code, or a partial one to disable groups of errors.

## Security

If you discover any security related issues, please email security@exolnet.com instead of using the issue tracker.

## Credits

- [Alexandre D'Eschambeault](https://github.com/xel1045)
- [Simon Gaudreau](https://github.com/Gandhi11)
- [All Contributors](../../contributors)

## License

This code is licensed under the [MIT license](http://choosealicense.com/licenses/mit/). 
Please see the [license file](LICENSE) for more information.
