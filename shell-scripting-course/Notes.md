# Note
This is a note for the course.
All examples are under the `lessons` subfolder of the folder which has the same name as the section. e.g. under section `2-shell-scripting-in-a-nutshell`, a use case is `script-07.sh`, meaning the full path of the example is `//shell-scripting-course/2-shell-scripting-in-a-nutshell/lessons/script-07.sh`.

# 2-shell-scripting-in-a-nutshell

## shebang (`#!` sign)
1. The `#!/bin/bash` at the first line of a script is called [shebang](https://en.wikipedia.org/wiki/Shebang_(Unix)). It specify what is the interpreter to run the script.
2. If there is no shebang defined, the commands are executed by your current shell.
3. Different shells have slightly different syntax.

## Variables
1. Syntax: VARIABLE_NAME="Value".
2. No spaces before or after the `=` sign.
3. By convention variable names are uppercase.
4. To use a variable, you can use `$` before the name: `$VARIABLE_NAME`.
5. To use a variable, you can use `${}` aroung the name: `${VARIABLE_NAME}`. Use case: `script-07.sh`. Compare to the wrong usage in `script-08.sh`
6. You can assign the output of a command to a variable with `$`. See `script-09.sh`. You may also see &#96;&#96;, which can appear in old scripts, see `script-10.sh`

## If statement
1. Syntax: `[ condition ]`, e.g. `[ -e /etc/pass ]`
2. There are tests for files, strings, math....
3. Use it with `if` . See `script-11.sh`.
4. Use it with `if else` . See `script-12.sh`. 
5. Use it with `if elif else` . See `script-13.sh`.

## For loop
1. For syntax, see `script-14.sh`.
2. Also you can store items in a variable, see `script-15.sh`.

## Positional parameters
1. `$ script.sh parameter1 parameter2 parameter3` will have these positional parameters: 
   1. $0: `cript.sh`
   2. $1: `parameter1`
   3. $2: `parameter2`
   4. $3: `parameter3`
2. Examples: `archive_user.sh`, `archive_user-01.sh`, `archive_user-02.sh`.
3. A special syntax is `$@`, meaning all parameters from `$1` to the last parameter. See `archive_user-02.sh`.

## Accept standard input (STDIN)
1. Use the `read` command: `read -p "PROMPT" VARIABLE`.
2. Example: `archive_user-03.sh`.

# 3-exit-status-and-return-code

## Exit Status
1. Every command returns a exit status.
2. Range: [0, 255].
3. 0 = success
4. !0 = error condition
5. Use `man` or `info` to find meaning of exit status
6. `$?` contains the return code of the previously executed command. e.g.: 
   * `ls /not/here`
   * `echo $?`
7. Check `exit-status-01.sh`, `exit-status-02.sh`, `exit-status-03.sh`.

## && and ||
1. && = AND, will execute the latter commadns only if the prev command succeeds. Check `exit-status-04.sh`.
2. || = OR, will execute the latter commadns only if the prev command fails. Check `exit-status-05.sh`.

## The Semicolon
1. To ensure multiple commands get executed in one line, use `;`. e.g.:
   * `cp a.txt /tmp ; cp a.txt /tmp/backup`
   * it does not check if the prev command succed or not

## Exit Command
1. Explicitly define the return code:
   1. `exit 0`
   1. `exit 1`
   1. `exit 0`
   1. `exit 0`
   1. etc
2. If not defined, the default value will be the return code of the last executed command.
3. Check `exit-status-06.sh`.

# 4-functions

## Create and Call a function
1. Options:
   1. function func-name() {//code}
   2. func-name() {//code}
2. Define the function before it is called (shell script is not pre-compiled). Bad praction: check `function-3`
3. To call a function: `func-name`
4. Check: `function-1`, `function-2`

## Positional Parameters
1. The first parameter is stored in `$1`, second is `$2`, etc.
2. `$@` contains all parameters. Check `function-4`.
3. `$0` is the script itself, not the function name.
4. To pass parameters, check `function-4`.

## Variable scope - Global Variables
1. By default, variables are global. 
2. Variables have to be defined before used. Check `function-6`, `function-7` and `function-8`.

## Variable scope - Local Variables
1. Can be only accessed within the function.
2. Create using the `local` keyword.
3. Only functions can have local variables.
4. It is best practice to keep variables local in functions.
5. Check `function-09`.

## Exit Status (Return Code)
1. Functions have an exist status
2. Explicitly: `return <code>`
3. Implicitly: the return code of the last executed command

# 6-wildcards

## Wildcards
1. Wildcards can be used with many commands.
2. `*`: matches 0 or more chars
   1. `*.txt`
   2. `a*`
   3. `a*.txt`
3. `?`: matches exactly 1 char
   1. `?.txt`
   2. `a?`
   3. `a?.txt`

## Character classes
1. `[]`: a character class. Matches any of the chars included between `[]`. Matches exactly 1 char.
   1. `[aeiou]`
   2. `ca[nt]*` can match:
      1. can
      2. cat
      3. candy
      4. catch
2. `[!]`: Matches any of the chars not included between `[!]`. Matches exactly 1 char.
   1. `[!aeiou]*` matches:
      1. basketball
      2. cricket
3. Range: use 2 chars separated by a `-` to create a range in a character class.
   1. `[a-g]*`: matches all files start with a, b, c, d, e, f or g.
   2. `[3-6]*`: matches all files start with 3, 4, 5 or 6.

## Named Character Classes
1. `[[:alpha:]]`: matches alphabetic letters, both lower & upper cases.
2. `[[:alnum:]]`: matches alphabetic letters and digits - lower & upper case letters and decimal digits.
3. `[[:digit:]]`: matches 0 - 9.
4. `[[:lower:]]`: matches lower case letters.
5. `[[:space:]]`: matches white spaces, e.g. space, tab, new line characters.
6. `[[:upper:]]`: matches upper case letters.

## Matching Wildcard Patterns
1. `\`: escape character. Use if you want to match a wildcard character.
   1. `*\?`: matches all file ends with `?`:
      1. done?

# 7-case-statement

## Case Statements
1. Check `case-01.sh` to know what is the basic layout and grammer.
2. You can use wildcard in case statement.

# 8-logging

## Syslog
1. The syslog standard uses facitlities and severities to categorize messages. 
   1. Facilities: indicates where the log is from
      1. `kern` - kernal
      2. `user` - use it when you don't know which to use
      3. `mail` - an app which handles mail
      4. `daemon`
      5. `auth`
      6. `local0`, `local1`, ....`local7` - custom logs
   1. Severities: how sever the log is
      1. `emerg`
      2. `alert`
      3. `crit`
      4. `err`
      5. `warning`
      6. `notice`
      7. `info`
      8. `debug`
2. Log file locations are configurable. Some common locations:
   1. `/var/log/messages`
   2. `/var/log/syslog`

## Logging with logger
1. The logger utility
2. By default severity is `notice`
   1. `logger "message"`
   2. `logger -p local0.info "message"`
   3. `logger -s -p local0.info "message"`
   4. `logger -t myscript -p local0.info "Message"`
   5. `logger -i -t myscript "Message"`

# 9-while-loops

## While Loops
1. Control loop:
   1. Explicit number of times. See `while-loops-01.sh`.
   2. User input. See `while-loops-02.sh`.
   3. Command exit state. See `while-loops-03.sh`.
2. Reading files, line by line. See `while-loops-04.sh`, `while-loops-05.sh`, `while-loops-06.sh`.
3. Break. See `while-loops-07.sh`.
4. Continue. See `while-loops-08.sh`.

# 10-debugging

## Built-in Debugging Help
1. `-x`: print commands as they execute
   1. After substitutions and expansions
   2. Called an x-trace, tracing, or print debugging
   3. e.g.:
      1. `#!/bin/bash`
      2. `set -x` # start debugging
      3. {code}
      4. `set +x` # stop debugging
   4. Check `debugging-01.sh`, `debugging-02.sh`
2. `-e`: exit on error
   1. Can be combined with other options
      1. `#!/bin/bash -ex`
      2. `#!/bin/bash -xe`
      3. `#!/bin/bash -x -e`
      4. `#!/bin/bash -e -x`
   2. Check `debugging-03.sh`, `debugging-04.sh`
3. `-v`: print shell input lines as they are read
   1. Can be combined with other options
   2. Check `debugging-05.sh`, `debugging-06.sh`
4. For more info: `help set | less`

## Manual Debugging
1. Create your own debugging code
2. Use a special var like `DEBUG`
   1. `DEBUG=true`
   2. `DEBUG=false`
   3. Check [`debugging-07.sh`, `debugging-13.sh`]
3. Manual copy and paste
   1. Open a second terminal
   2. Copy & paste the commands into terminal line by line
   3. Can be useful to work with `set -x`

## Syntax highlighting
1. Use Editors with syntax highlighting.

## Use PS4
1. Controls what is displayed before a line using the `-x` option
2. The default value is `+`
3. Can use with bash variables: BASH_SOURCE, LINENO, etc
4. Check `debugging-14.sh`, `debugging-15.sh`

## DOS vs Linux(Unix) file types
1. Linux system has line feed to represent end of line (`LF`). Windows is different, it has 2 more characters for end of line. It is `CRLF` - Carriage Return, Line Feed.
2. To print un-printable characters, use `cat -v script.sh`. It will print `^M`
3. Run `cat -v script.sh` for `debugging-16.sh`, `debugging-17.sh` to check the difference.
4. Run `debugging-16.sh`, `debugging-17.sh` to check the difference. See the error when you run `debugging-16.sh`.
5. If you find some strange errors, check if there are carriage returns in the script.
6. In addition to `cat`, you can use `file` command to check. e.g.
   * e.g. 1
   * `file debugging-16.sh` 
   * `debugging-16.sh: Bourne-Again shell script text executable, ASCII text, with CRLF line terminators`
   * e.g.2
   * `file debugging-17.sh`
   * `debugging-17.sh: Bourne-Again shell script text executable, ASCII text`
7. A util `dos2unix` command, converts a file with carriage returns to a file without them.
8. A util that do the convert against `dos2unix` is `unix2dos`
9. How does this problem happens?
   1.  Using a Windows editor and uploading Unix.
       1.  Some editors can configure to just use LF.
   2.  Pasting from a Windows to a Linux terminal.
   3.  Pasting from a browser to a Linux terminal.

# 11 Data Manipulation and Text transformations with Sed

## Sed
1. `sed`: stream editor
2. stream are typically textual data
3. sed performs text transformations on streams
   1. substitute text with some other text
   2. remove lines
   3. append lines after given lines
   4. Insert lines before certain lines
   5. ...
4. `sed` is used programmatically, not interactively