# Overview
This project demonstrates that serverless removes all whitespace characters from the names of [custom variable types](https://serverless.com/framework/docs/providers/aws/guide/plugins#custom-variable-types) before they are sent to a plugin to be resolved.

To show this we use the [EchoTestVarPlugin](https://serverless.com/framework/docs/providers/aws/guide/plugins#custom-variable-types) from the serverless documentation. It resolves a variable like `${echo:foo}` to the string `foo`.

However we show that a variable like `${echo:foo bar}` resolves to `foobar` instead of `foo bar` as expected.

Also if double quotes or single quotes are used (e.g. `${echo:"foo bar"}` or `${echo:'foo bar'}` the plugin will not be used to resolve those variables.

# Steps to reproduce:
- `yarn install`
- `yarn sls print`
- Observe a log message "EchoTestVarPlugin resolving echo:foobar" which shows the plugin was invoked to resolve a variable but the name has whitespace removed
- Observe the value of `var1` is `foobar` instead of `foo bar` as expected
- Observe no further logs messages from EchoTestVarPlugin which shows it isn't invoked for `var2` and `var3`
- Observe the values of `var2` and `var3` are incorrect

