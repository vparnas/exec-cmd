# exec-cmd

## Description 

A less verbose system for defining reusable parameterized commands than shell functions, yet more powerful than shell aliases. 

## Advantages

1. Commands that support parameterization unlike traditional shell aliases
1. Automatic help text to echo the expected or missing parameter names. One must manually design this behavior into traditional shell functions. 

## Disadvantages

1. Theoretically slower than leveraging the native shell aliases/functions; a separate process is spawned for each stackable call. 

    The difference, however, is not to be perceived: the solution is built in shell, which leverages fast C routines and system calls.

## Setup

1. Configuration file (optional)

    The only parameters relevant to the executable are:

    ```
    DEBUG=false
    PARAM_CHAR='!!'
    SELF_REF='%self%'
    ```

    *PARAM_CHAR* is the parameter prefix. 

    *SELF_REF* is the prefix that precedes any internally defined command invocation.

    To change these values, paste the above lines in a config file, either `$HOME/.exec-cmd.rc` or one defined by the `$EXEC_CMD_CFG` environment variable.

1. The commands

    `exec-cmd` reads all definitions from either `$HOME/commands` or `$EXEC_CMD_DIR/commands` if you've defined the `$EXEC_CMD_DIR` environment variable. 

    The same applies to the `commands_help` optional help text.

1. Place `exec-cmd` within your environment path.

## Usage

See the example `commands` file for sample usage. This is the most straightforward way to get accustomed to the syntax.

Each command is defined as follows:

```
<name>[,<alias1>,<alias2>,...] <command>
```

Make sure not to leave any space between the command names and the optional comma-separated aliases.

A command may include parameters, these prefixed with '!!' by default. 

As you invoke a command, `exec-cmd` checks for all included parameters and outputs a message with any missing ones and their names.

To call an existing command/alias, include the tag %self% (reconfigurable in $SELF_REF) followed by the existing command/alias name.

Define your own optional command help text in `commands_help`. See the default file for examples.

To execute a command, simply provide a command to the executable along with the required parameters:

```
$ exec-cmd test 1 2 3
param1: 1, param2: 2, param3: 3
```

Execute with -h to view the other available options, or -h \<command\> to reference the defined help text.

## Command line completion

To ease the command line entry, you may configure the simple one-word shell autocompletion for your defined commands.  

Assuming you've placed the executable within your path, include the following in your `~/.bashrc:` or similar:

```
if [ -f ~/bin/exec-cmd ]; then
    ec_opts=$(exec-cmd -c)
    complete -W "$ec_opts" exec-cmd ec
fi
```
