# Overview
This project demonstrates serverless [issue 7223](https://github.com/serverless/serverless/issues/7223). Serverless removes all whitespace characters from the names of [custom variable types](https://serverless.com/framework/docs/providers/aws/guide/plugins#custom-variable-types) before they are sent to a plugin to be resolved.

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


# Output:
```
> yarn sls print                                                     
yarn run v1.19.1
$ /Users/mpv/dev/hack/sls-variable-bug/node_modules/.bin/sls print
EchoTestVarPlugin resolving echo:foobar
service: sls-variable-bug
provider:
  name: aws
plugins:
  - ./echo-test-var-plugin
custom:
  var1: foobar
  var2: 'echo:"foo bar'
  var3: 'echo:''foo bar'

Done in 2.83s.
```



