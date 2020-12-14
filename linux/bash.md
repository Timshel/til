# Bash tips

## Basic if

```bash
if [ "$depth" -eq "0" ]; then
   echo "false";
   exit;
fi
```

## Import a script file

The source command run the given file :
```
source utils.sh
```

## Array expansion modifiers for var args handling:

`"${@:3}"` Will return a string with all args starting with the third one.
It can handle range such as : `"{@:2:5}"`

## Parameter expension

Most useful to assign with a default :

```bash
TOTO=${1:-default}
```
More expensions: http://www.gnu.org/software/bash/manual/html_node/Shell-Parameter-Expansion.html

## Append multiline to a file :

```bash
tee --append greetings.txt > /dev/null <<EOT
line 1
line 2
EOT
```
