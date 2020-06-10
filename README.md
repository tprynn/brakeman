
[Brakeman](https://github.com/presidentbeef/brakeman) changed their license to a non-free license in 2018/2019. This is a clone of the last MIT-licensed commit of the upstream Brakeman repo.

I have not read any Brakeman source code/commits in the upstream repo and any contributors to this repo will need to assert the same.

# Brakeman

Brakeman is an open source static analysis tool which checks Ruby on Rails applications for security vulnerabilities.


# Installation

Do not install via rubygems, as the installed version is not freely licensed.

# Usage

From a Rails application's root directory:

    brakeman

Outside of Rails root:

    brakeman /path/to/rails/application

# Compatibility

Brakeman should work with any version of Rails from 2.3.x to 5.x.

Brakeman can analyze code written with Ruby 1.8 syntax and newer, but requires at least Ruby 1.9.3 to run.

# Basic Options

For a full list of options, use `brakeman --help` or see the [OPTIONS.md](OPTIONS.md) file.

To specify an output file for the results:

    brakeman -o output_file

The output format is determined by the file extension or by using the `-f` option. Current options are: `text`, `html`, `tabs`, `json`, `markdown`, `csv`, and `codeclimate`.

Multiple output files can be specified:

    brakeman -o output.html -o output.json

To suppress informational warnings and just output the report:

    brakeman -q

Note all Brakeman output except reports are sent to stderr, making it simple to redirect stdout to a file and just get the report.

To see all kinds of debugging information:

    brakeman -d

Specific checks can be skipped, if desired. The name needs to be the correct case. For example, to skip looking for default routes (`DefaultRoutes`):

    brakeman -x DefaultRoutes

Multiple checks should be separated by a comma:

    brakeman -x DefaultRoutes,Redirect

To do the opposite and only run a certain set of tests:

    brakeman -t SQL,ValidationRegex

If Brakeman is running a bit slow, try

    brakeman --faster

This will disable some features, but will probably be much faster (currently it is the same as `--skip-libs --no-branching`). *WARNING*: This may cause Brakeman to miss some vulnerabilities.

By default, Brakeman will return a non-zero exit code if any security warnings are found or scanning errors are encountered. To disable this:

    brakeman --no-exit-on-warn --no-exit-on-error

To skip certain files or directories that Brakeman may have trouble parsing, use:

    brakeman --skip-files file1,/path1/,path2/

To compare results of a scan with a previous scan, use the JSON output option and then:

    brakeman --compare old_report.json

This will output JSON with two lists: one of fixed warnings and one of new warnings.

Brakeman will ignore warnings if configured to do so. By default, it looks for a configuration file in `config/brakeman.ignore`.
To create and manage this file, use:

    brakeman -I

# Warning information

See [warning\_types](docs/warning_types) for more information on the warnings reported by this tool.

# Warning context

The HTML output format provides an excerpt from the original application source where a warning was triggered. Due to the processing done while looking for vulnerabilities, the source may not resemble the reported warning and reported line numbers may be slightly off. However, the context still provides a quick look into the code which raised the warning.

# Confidence levels

Brakeman assigns a confidence level to each warning. This provides a rough estimate of how certain the tool is that a given warning is actually a problem. Naturally, these ratings should not be taken as absolute truth.

There are three levels of confidence:

 + High - Either this is a simple warning (boolean value) or user input is very likely being used in unsafe ways.
 + Medium - This generally indicates an unsafe use of a variable, but the variable may or may not be user input.
 + Weak - Typically means user input was indirectly used in a potentially unsafe manner.

To only get warnings above a given confidence level:

    brakeman -w3

The `-w` switch takes a number from 1 to 3, with 1 being low (all warnings) and 3 being high (only highest confidence warnings).

# Configuration files

Brakeman options can stored and read from YAML files. To simplify the process of writing a configuration file, the `-C` option will output the currently set options.

Options passed in on the commandline have priority over configuration files.

The default config locations are `./config/brakeman.yml`, `~/.brakeman/config.yml`, and `/etc/brakeman/config.yml`

The `-c` option can be used to specify a configuration file to use.

# License

see [MIT-LICENSE](MIT-LICENSE)
