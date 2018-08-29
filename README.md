# exec-cmd

### Description 

A less verbose system for defining reusable parameterized commands than shell functions, and more powerful than shell aliases. 

### Usage

Parameters are preceded by the characters !!, configurable in the main script. The command processing automatically executes a check for all included parameters, rendering an error message with the required parameters.

Define your desired commands in *commands*, referenced by the *$commands_cfg* variable in the main executable.  See the default provided file for examples of usage.

Each command is defined as follows:

```
<name>[,<alias1>,<alias2>,...] <command>
```

Make sure not to leave any space between the command names and the optional comma-separated aliases.  

To reference an existing command, include the tag %self% (also configurable in the variable *$SELF_REF*) followed by the command/alias name defined in the same command file.

Define your own optional command help text in *commands_help*. See the default file for examples.

To execute a command, simply provide a command to the executable along with the required parameters:

```bash
$ ./exec-cmd test 1 2 3
param1: 1, param2: 2, param3: 3
```

Execute with -h to view the other available options, or -h \<command\> to reference the defined help text.

### Command line completion

To ease the command line entry, you may configure the simple one-word shell autocompletion for your defined commands.  First, simplify execution, create a symlink to the executable in ~/bin.  Then, place the following in your ~/.bashrc:

```bash
if [ -f ~/bin/exec-cmd ]; then
    ec_opts=$(exec-cmd -c)
    complete -W "$ec_opts" exec-cmd ec
fi
```
