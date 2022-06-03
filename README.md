# pure-pwsh-bible
Currently a rewrite of [pure bash bible](https://github.com/dylanaraps/pure-bash-bible) in PowerShell.
Will possibly add content unique to PowerShell in the future.

# STRINGS

## Trim leading and trailing white-space from string
**Example Function:**
```pwsh
function trim_string {
    # Usage: trim_string "   example   string    "
    # "   example   string    " | trim_string
    $input+$args | % {
        $_.Trim()
    }
}
```
**Example Usage:**
```powershell
PS>trim_string "    Hello,  World    "
Hello, World

PS>$name="   John Black  "
PS>$name | trim_string
John Black

PS>"   or call .Trim() directly  ".Trim()
or call .Trim() directly
```
## Trim all white-space from string and truncate spaces
**Example Function:**
```pwsh
function trim_all {
    # Usage: trim_all "   example   string    "
    # "   example   string    " | trim_all
    $input+$args | % {
        [regex]::Replace($_.Trim(),"\s+"," ")
    }
}
```
**Example Usage:**
```powershell
PS>trim_all "    Hello,    World    "
Hello, World

PS>$name="   John   Black  is     my    name.    "
PS>$name | trim_all
John Black is my name.
```
## Use regex on a string
**Example Function:**
```pwsh
function regex($Pattern){
    # Usage: regex "regex" "string"
    # "string" | regex "regex"
    $input+$args | % {
        if ($_ -match $Pattern) {
            $matches[1]
        }
    }
}
```
**Example Usage:**
```powershell
PS># Trim leading white-space.
PS>regex "   hello" -Pattern "^\s*(.*)"
hello

PS># Validate a hex color.
PS>regex "#FFFFFF" -Pattern '^(#?([a-fA-F0-9]{6}|[a-fA-F0-9]{3}))$'
#FFFFFF

PS># Validate a hex color (invalid).
PS>regex "red" -Pattern '^(#?([a-fA-F0-9]{6}|[a-fA-F0-9]{3}))$'
# no output (invalid)
```
**Example Usage in script:**
```powershell
function is_hex_color {
    $input+$args | % {
        if ($_ -match '^(#?([a-fA-F0-9]{6}|[a-fA-F0-9]{3}))$') {
            "$_"
        } else {
            Write-Error "$_ is an invalid color."
        }
    }
}

$color = Read-Host
is_hex_color $color || $color="#FFFFFF"

# Do stuff
```
## Split a string on a delimiter
**Example Function:**
```pwsh
function split($Delimiter){
    # Usage: split "delimiter" "string"
    # split "string" -Delimiter "delimiter"
    # "string" | split "delimiter"
    ($input+$args) -split $Delimiter
}
```
**Example Usage:**
```powershell
PS>split "," "apples,oranges,pears,grapes"
apples
oranges
pears
grapes

PS>split "1, 2, 3, 4, 5" -Delimiter ", "
1
2
3
4
5

# Multi char delimiters work too!
PS>"hello---world---my---name---is---john" | split "---"
hello
world
my
name
is
john

# Or just call -split directly
PS>"a;b;c;d" -split ";"
a
b
c
d
```
## Change a string to lowercase
**Example Function:**
```pwsh
function lower {
    # Usage: lower "string"
    # "string" | lower
    $input+$args | % {
        $_.ToLower()
    }
}
```
**Example Usage:**
```powershell
PS>lower "HELLO"
hello

PS>lower "HeLlO"
hello

PS>"hello".ToLower()
hello
```
## Change a string to uppercase
**Example Function:**
```pwsh
function upper {
    # Usage: upper "string"
    # "string" | upper
    $input+$args | % {
        $_.ToUpper()
    }
}
```
**Example Usage:**
```powershell
PS>upper "hello"
HELLO

PS>upper "HeLlO"
HELLO

PS>"HELLO".ToUpper()
HELLO
```
## Reverse a string case
**Example Function:**
```pwsh
function reverse_case {
    # Usage: reverse_case "string"
    # "string" | reverse_case
    $input+$args | % {
        $sb=[Text.StringBuilder]::new()
        $_.ToCharArray() | % {
            if($_ -ge 97){
                $null = $sb.Append("$_".ToUpper())
            } else {
                $null = $sb.Append("$_".ToLower())
            }
        }
        $sb.ToString()
    }
}
```
**Example Usage:**
```powershell
PS>reverse_case "hello"
HELLO

PS>"HeLlO" | reverse_case
hElLo

PS>reverse_case "HELLO"
hello
```
## Trim quotes from a string
**Example Function:**
```pwsh
function trim_quotes {
    # Usage: trim_quotes "string"
    # "string" | trim_quotes
    $input+$args | % {
        $_.Replace("'","").Replace("`"","")
    }
}
```
**Example Usage:**
```powershell
PS>$var="'Hello', `"World`""
PS>trim_quotes $var
Hello, World
```
## Strip all instances of pattern from string
**Example Function:**
```pwsh
function strip_all($Pattern) {
    # Usage: strip_all "pattern" "string"
    # "string" | strip_all "pattern"
    $input+$args | % {
        [regex]::Replace($_,$Pattern,"")
    }
}
```
**Example Usage:**
```powershell
PS>strip_all "[aeiou]" "The Quick Brown Fox"
Th Qck Brwn Fx

PS>strip_all "The Quick Brown Fox" -Pattern " "
TheQuickBrownFox

PS>"The Quick Brown Fox" | strip_all "Quick "
The Brown Fox
```
## Strip first occurrence of pattern from string
**Example Function:**
```pwsh
function strip {
    # Usage: strip "pattern" "string"
    # "string" | strip "pattern"
    $input+$args | % {
        [regex]::new($Pattern).Replace($_,"",1)
    }
}
```
**Example Usage:**
```powershell
PS>strip "[aeiou]" "The Quick Brown Fox"
Th Quick Brown Fox

PS>"The Quick Brown Fox" | strip " "
TheQuick Brown Fox
```
## Strip pattern from start of string
**Example Function:**
```pwsh
function lstrip($Pattern) {
    # Usage: lstrip "pattern" "string"
    # "string" | lstrip "pattern"
    $input+$args | % {
        if($_.StartsWith($Pattern)){
            $_.Substring($Pattern.Length)
        } else {
            $_
        }
    }
}
```
**Example Usage:**
```powershell
PS>lstrip "The " "The Quick Brown Fox"
Quick Brown Fox
```
## Strip pattern from end of string
**Example Function:**
```pwsh
function rstrip($Pattern) {
    # Usage: rstrip "pattern" "string"
    # "string" | rstrip "pattern"
    $input+$args | % {
        if($_.EndsWith($Pattern)){
            $_.Substring(0,$_.Length - $Pattern.Length)
        } else {
            $_
        }
    }
}
```
**Example Usage:**
```powershell
PS>rstrip " Fox" "The Quick Brown Fox"
The Quick Brown
```
## Percent-encode a string
**Example Function:**
```pwsh
function urlencode {
    # Usage: urlencode "string"
    # "string" | urlencode
    $input+$args | % {
        [System.Web.HTTPUtility]::UrlEncode($_)
    }
}
```
**Example Usage:**
```powershell
PS>urlencode "https://github.com/n4t-dog/pure-pwsh-bible"
https%3a%2f%2fgithub.com%2fn4t-dog%2fpure-pwsh-bible
```
## Decode a percent-encoded string
**Example Function:**
```pwsh
function urldecode {
    # Usage: urldecode "string"
    # "string" | urldecode
    $input+$args | % {
        [System.Web.HTTPUtility]::UrlDecode($_)
    }
}
```
**Example Usage:**
```powershell
PS>urlencode "https%3a%2f%2fgithub.com%2fn4t-dog%2fpure-pwsh-bible"
https://github.com/n4t-dog/pure-pwsh-bible
```
## Check if string contains a sub-string
**Using a test:**
```powershell
if ($var.Contains("sub_string")) {
    "sub_string is in var."
}

# Inverse (substring not in string).
if(!$var.Contains("sub_string")) {
    "sub_string is not in var."
}

# This works for arrays too!
if($arr -like "*sub_string*") {
    "sub_string is in array."
}
```
**Using a case statement:**
```powershell
switch ($var) {
    {$_.Contains("sub_string")}{
        # Do stuff
    }
    {$_ -like "*sub_string2*"}{
        # Do more stuff
    }
    default{
        # Else
    }
}
```
## Check if string starts with sub-string
```powershell
if ($var.StartsWith("sub_string*")) {
    "var starts with sub_string."
}

# Inverse (var does not start with sub_string).
if ($var -notlike "sub_string*") {
    "var does not start with sub_string."
}
```
## Check if string ends with sub-string
```powershell
if ($var.EndsWith("sub_string")) {
    "var ends with sub_string."
}

  # Inverse (var does not end with sub_string).
if ($var -notlike "*sub_string") {
    "var does not end with sub_string."
}
```
# ARRAYS
## Reverse an array
**Example Function:**
```pwsh
function reverse_array {
    # Usage: reverse_array "array"
    # "array" | reverse_array
    $array = foreach($i in $input+$args){$i}
    $array[$array.Count..0]
}
```
**Example Usage:**
```powershell
PS>reverse_array 1 2 3 4 5
5
4
3
2
1

PS>$arr="red","blue","green"
PS>$arr | reverse_array
green
blue
red

PS>("a","b","c")[0..2]
c
b
a
```
## Remove duplicate array elements
**Example Function:**
```pwsh
function remove_array_dups {
  # Usage: remove_array_dups "array"
  $array = foreach($i in $input+$args){$i}
  $array | Select-Object -Unique
}
```
**Example Usage:**
```powershell
PS>remove_array_dups 1 1 2 2 3 3 3 3 3 4 4 4 4 4 5 5 5 5 5 5
1
2
3
4
5

PS>$arr="red","red","green","blue","blue"
PS>$arr | remove_array_dups
red
green
blue
```
## Random array element
**Example Function:**
```pwsh
function random_array_element {
    # Usage: random_array_element "array"
    $input+$args | Get-Random
}
```
**Example Usage:**
```powershell
PS>$array="red","green","blue","yellow","brown"
PS>random_array_element $array
yellow

# Multiple arguments can also be passed.
PS>random_array_element 1 2 3 4 5 6 7
3

# Call Get-Random directly
PS>Get-Random -InputObject $array
red
```

## Cycle through an array
```pwsh
$arr = "a","b","c","d"

function cycle {
     $arr[$(if($i){$i}else{0})]
     if($i -ge $arr.Count-1){$script:i=0}else{++$script:i}
}
```
```powershell
# Using null coalescing/ternary (PowerShell 7+)
$arr = 1..10

function cycle {
    $arr[$i ?? 0]
    $script:i=$i -ge $arr.Count-1?0:++$i
}
```
## Toggle between two values
```pwsh
# The previous code could also be reused
$arr = "one","two"

function toggle {
    $arr[$toggle ?? $false]
    $script:toggle = -not $toggle
}
```
# LOOPS
## Loop over a range of numbers
```powershell
# Loop from 0-100 with for
for($i=0;$i -le 100;$i++){$i}

# With foreach
foreach($i in 0..100){$i}

# With ForEach-Object
0..100 | ForEach-Object{$_}
```
## Loop over a variable range of numbers
```powershell
# Loop from 0-VAR.
$VAR=50

# With foreach
foreach($i in 0..$VAR){$i}

# With for
for($i=0;$i -le $VAR;$i++){$i}

# With ForEach-Object
0..$VAR | ForEach-Object {$_}
```
## Loop over an array
```powershell
$arr = "apples","oranges","tomatoes"

# With for
for($i=0;$i -lt $arr.Count;$i++){$arr[$i]}

# With foreach
foreach($element in $arr){$element}

# With ForEach-Object
$arr | ForEach-Object {$_}
```
## Loop over an array with an index
```powershell
$arr = "apples","oranges","tomatoes"

# With for
for($i=0;$i -lt $arr.Count;$i++){$arr[$i]}

# With foreach
foreach($i in 0..($arr.Count-1)){$arr[$i]}

# With ForEach-Object
0..($arr.Count-1) | ForEach-Object{$arr[$_]}
```
## Loop over the contents of a file
```powershell
# With for
$file = Get-Content "file"
for($i=0;$i -lt $file.Count;$i++){$file[$i]}

# With foreach
foreach($l in (Get-Content "file")){$l}

# With ForEach-Object
Get-Content "file" | ForEach-Object{$_}
```
## Loop over files and directories
```powershell
# All files
foreach($file in (Get-ChildItem)){
    $file
}

# PNG files in dir
foreach($file in (Get-ChildItem ~/Pictures/*.png)){
    $file
}

# Iterate over directories
foreach($dir in (Get-ChildItem ~/Downloads -Directory)){
    $dir
}

# "Brace Expansion"
foreach($file in ("file1","file2","subdir/file3"|%{"/path/to/parentdir/"+$_})){
    $file
}

# Iterate recursively
foreach($file in (Get-ChildItem ~/Pictures -Recurse -File){
    $file
}
```
# FILE HANDLING
## Read a file to a string
```powershell
(Get-Content "file") -join "`n"
```
## Read a file to an array (*by line*)
```powershell
# All lines
Get-Content "file"

# Remove blank lines
Get-Content "file" | Where-Object {$_}
```
## Get the first N lines of a file
```powershell
Get-Content "file" -TotalCount $N
```
## Get the last N lines of a file
```powershell
Get-Content "file" -Tail $N
```
## Get the number of lines in a file
```powershell
(Get-Content "file").Count

# Memory friendly
$count = 0
$reader = [System.IO.File]::OpenText("file")
while($reader.ReadLine() -ne $null){
    $count++
}
$count

# Memory friendly (.NET 4+)
[System.IO.File]::ReadLines("file") | Measure-Object -Line
```
## Count files or directories in directory
**Example Function:**
```pwsh
function count($Directory){
    $count = 0
    $input+$args | % {
        $count+=(Get-ChildItem $_ -Directory:$Directory -Force).Count
    }
    $count
}
```
**Example Usage:**
```powershell
# Count all files in a dir
PS>count ~/Downloads
232

# Count all dirs in a dir.
PS>"~/Downloads" | count -Directory
45

# Count all jpg files in dir.
PS>(Get-ChildItem ~/Pictures/*.jpg).Count
64
```
## Create an empty file
```powershell
# Shortest
"">file

# Longer alternatives
$null>file
New-Item "file"
"" | Out-File "file"
```
## Extract lines between two markers
**Example Function:**
```pwsh
function extract($Opening,$Closing){
    # Usage: extract "opening marker" "closing marker" "file"
    $extract=0
    $input+$args | % {
        Get-Content $_ | % {
            if($extract -and $_ -ne $Closing){$_}
            if($_ -eq $Opening){$extract=1}
            if($_ -eq $Closing){$extract=0}
        }
    }
}
```
**Example Usage:**
```powershell
# Extract code blocks from MarkDown file.
PS>extract ./README.md -Opening '```pwsh' -Closing '```'
# Output here...
```
# FILE PATHS
## Get the directory name of a file path
**Example Function:**
```pwsh
function dirname([switch]$Resolve){
    # Usage: dirname "path" [-Resolve]
    $input+$args | % {
        Split-Path $_ -Resolve:$Resolve
    }
}
```
**Example Usage:**
```powershell
PS>dirname ~/Pictures/Wallpapers/1.jpg
~/Pictures/Wallpapers

PS>"~/Pictures/Downloads" | dirname -Resolve
/home/user/Pictures
```
## Get the base-name of a file path
**Example Function:**
```pwsh
function basename([switch]$Base){
    # Usage: basename "path" [-Base]
    $input+$args | % {
        if ($Base) {
            Split-Path $_ -LeafBase
        } else {
            Split-Path $_ -Leaf
        }
    }
}
```
**Example Usage:**
```shell
PS>basename ~/Pictures/Wallpapers/1.jpg
1.jpg

PS>basename ~/Pictures/Wallpapers/1.jpg -Base
1

PS>"~/Pictures/Downloads/" | basename
Downloads
```
# VARIABLES
## Assign and access a variable using a variable
```powershell
PS>$hello_world = "value"

# Create the variable name.
PS>$var = "world"
PS>$ref = "hello_$var"

# Print the value of the variable name stored in 'hello_$var'.
PS>Get-Variable -ValueOnly $ref
value
```
## Name a variable based on another variable
```powershell
PS>$var = "world"
PS>Set-Variable "hello_$var" -Value "value"
PS>$hello_world
value
```
# ESCAPE SEQUENCES
## Text Colors
| Sequence                  | What does it do?                            | Value       |
|---------------------------|---------------------------------------------|-------------|
| `` `e[38;5;<NUM>m``       | Set text foreground color.                  | `0-255`     |
| `` `e[48;5;<NUM>m``       | Set text background color.                  | `0-255`     |
| `` `e[38;2;<R>;<G>;<B>m`` | Set the text foreground color to RGB color. | `R`,`G`,`B` |
| `` `e[38;2;<R>;<G>;<B>m`` | Set the text background color to RGB color. | `R`,`G`,`B` |
Using `$PSStyle` (PowerShell 7.2+)
```powershell
# Using the 16 Console Color names
$PSStyle.Foreground.<ConsoleColor>
$PSStyle.Background.<ConsoleColor>
# Using RGB values
$PSStyle.Foreground.FromRgb(<R>,<G>,<B>)
$PSStyle.Background.FromRgb(<R>,<G>,<B>)
# Using HEX values
$PSStyle.Foreground.FromRgb(0x<hexcolor>)
$PSStyle.Background.FromRgb(0x<hexcolor>)
```
## Text Attributes
Reset a single format by adding 20 to escape sequence, `` `e[22m`` resets both Bold and Faint.
Using `$PSStyle` (PowerShell 7.2+): `$PSStyle.Bold` will create bold text, `$PSStyle.BoldOff` will turn off bold text.
| Sequence   | What does it do?                  | $PSStyle Var  |
|------------|-----------------------------------|---------------|
| `` `e[m``  | Reset text formatting and colors. | Reset         |
| `` `e[1m`` | Bold text.                        | Bold          |
| `` `e[2m`` | Faint text.                       | N/A           |
| `` `e[3m`` | Italic text.                      | Italic        |
| `` `e[4m`` | Underline text.                   | Underline     |
| `` `e[5m`` | Blinking text.                    | Blink         |
| `` `e[6m`` | Flashing text.                    | N/A           |
| `` `e[7m`` | Highlighted text.                 | Reverse       |
| `` `e[8m`` | Hidden text.                      | Hidden        |
| `` `e[9m`` | Strike-through text.              | Strikethrough |
## Cursor Movement
| Sequence                 | What does it do?                      | Value            |
|--------------------------|---------------------------------------|------------------|
| `` `e[<LINE>;<COLUMN>H`` | Move cursor to absolute position.     | `line`, `column` |
| `` `e[H``                | Move cursor to home position (`0,0`). |                  |
| `` `e[<NUM>A``           | Move cursor up N lines.               | `num`            |
| `` `e[<NUM>B``           | Move cursor down N lines.             | `num`            |
| `` `e[<NUM>C``           | Move cursor right N columns.          | `num`            |
| `` `e[<NUM>D``           | Move cursor left N columns.           | `num`            |
| `` `e[s``                | Save cursor position.                 |                  |
| `` `e[u``                | Restore cursor position.              |                  |
## Erasing Text
| Sequence        | What does it do?                                         |
|-----------------|----------------------------------------------------------|
| `` `\e[K``      | Erase from cursor position to end of line.               |
| `` `\e[1K``     | Erase from cursor position to start of line.             |
| `` `\e[2K``     | Erase the entire current line.                           |
| `` `\e[J``      | Erase from the current line to the bottom of the screen. |
| `` `\e[1J``     | Erase from the current line to the top of the screen.    |
| `` `\e[2J``     | Clear the screen.                                        |
| `` `\e[2J\e[H`` | Clear the screen and move cursor to `0,0`.               |


# "PARAMETER EXPANSION"
## Indirection
```powershell
# Access a variable based on the value of VAR.
Get-Variable $VAR

# Expand list of variables starting with VAR.
Get-Variable "VAR*"
```
## Replacement
See STRINGS
## Length
| "Parameter"   | What does it do?                                            |
|---------------|-------------------------------------------------------------|
| `$VAR.Length` | Length of array in elements, or length of var in characters |
| `$ARR.Count`  | Length of array in elements.                                |
## Expansion
| "Parameter"                                   | What does it do?                         |
|-----------------------------------------------|------------------------------------------|
| `$VAR.Substring($OFFSET)`                     | Remove first `N` chars from variable     |
| `$VAR.Substring($OFFSET,$LENGTH)`             | Get substring from `N` to `N` character. |
| `$VAR.Substring(0,$OFFSET)`                   | Get first `N` chars from variable.       |
| `$VAR.Substring(0,$VAR.Length-$OFFSET)`       | Remove last `N` chars from variable.     |
| `$VAR.Substring($VAR.Length-$OFFSET)`         | Get last `N` chars from variable.        |
| `$VAR.Substring($OFFSET,$VAR.Length-$OFFSET)` | Cut first `N` chars and last `N` chars   |
## Case Modification
PowerShell has no built in methods for reverse case.
| "Parameter"                                       | What does it do?           |
|---------------------------------------------------|----------------------------|
| `$VAR.Substring(0,1).ToUpper()+$VAR.Substring(1)` | Uppercase first character. |
| `$VAR.ToUpper()`                                  | Uppercase all characters.  |
| `(Get-Culture).TextInfo.ToTitleCase($VAR)`        | Uppercase all words.       |
| `$VAR.Substring(0,1).ToLower()+$VAR.Substring(1)` | Lowercase first character. |
| `$VAR.ToLower()`                                  | Lowercase all characters.  |
## Default Value
**Variables**
| "Parameter"                   | What does it do?                             | CAVEAT  |
|-------------------------------|----------------------------------------------|---------|
| `$VAR ??= "STRING"`           | If `VAR` is unset, use `STRING` as its value | `PS 7+` |
| `$VAR ?? "STRING"`            | Return `STRING` if `VAR` is unset            | `PS 7+` |
| `$VAR = ($VAR)?$VAR:"STRING"` | If `VAR` is empty, use `STRING` as its value | `PS 7+` |
| `($VAR)?$VAR:"STRING"`        | Return `STRING` if `VAR` is empty            | `PS 7+` |
| `if(!$VAR){$VAR="STRING"}`    | If `VAR` is empty, use `STRING` as its value |         |
**Function Parameters**
```powershell
# Set VAR to STRING if nothing is passed
function f($VAR="STRING"){
    $VAR
}

# Proper formatting
function f{
    param($VAR="STRING")
    $VAR
}
```
# "BRACE EXPANSION"
## Ranges
```powershell
# Syntax: <START>..<END>

# Print numbers 1-100
1..100

# Print range of floats
11..19 | ForEach-Object {$_/10}
# As strings
1..9 | ForEach-Object {"1.$_"}

# Print chars a-z. (PowerShell 6+)
'a'..'z'
'A'..'Z'

# Nesting (A0,A1,...,Z9)
foreach($l in 'A'..'Z'){foreach($i in 0..9){"$l$i"}}
# NOTE: situation where foreach and ForEach-Object are NOT interchangeable

# Print zero-padded numbers.
1..100 | ForEach-Object {"$_".PadLeft(2,'0')}

# Change increment amount
1..5 | ForEach-Object {$_*2-1}

# Increment backwards
5..-5

# Variable range
$VAR=50
1..$VAR
```
## String Lists
```powershell
"apples","oranges","pears","grapes"

# Example Usage:
# Remove dirs Movies, Music and ISOS from ~/Downloads/.
"Movies","Music","ISOS" | ForEach-Object {
    Remove-Item -Force "~/Downloads/"+$_
}
```

# CONDITIONAL EXPRESSIONS
## File Conditionals
```powershell
# If file exists
Test-Path file

# If file exists and is a directory.
Test-Path file -PathType Container

# If file exists and is a file.
Test-Path file -PathType Leaf

# If file exists and is a symbolic link.
(Get-Item file -ErrorAction Ignore).LinkType -eq "SymbolicLink"

# If file exists and its size is greater than zero.
(Get-Item file -ErrorAction Ignore).Size

# If file has been modified since last read.
#Note: most programs do not update last access time anymore, so this type of conditional will output true regardless of actual status in both sh and pwsh.
((Get-Item file).LastWriteTime - (Get-Item file).LastAccessTime) -gt 0

# Windows Permissions
Get-Acl file

# Unix Permissions
#Note: does not show sticky bit
(Get-Item file).UnixMode
```
## File Comparisons
**If `file` is newer than `file2` (*uses modification time*)**
```powershell
((Get-Item file).LastWriteTime - (Get-Item file2).LastWriteTime) -gt 0
```
## Variable Conditionals
| Expression                     | What does it do?                 |
|--------------------------------|----------------------------------|
| `$PSSessionOption`             | Get shell options                |
| `[bool]$VAR`                   | If variable has value assigned.  |
| `$VAR. -is ([ref]0).GetType()` | If variable is name reference.   |
| `$VAR.Length -eq 0`            | If length of string is zero.     |
| `$VAR.Length -ne 0`            | If length of string is non-zero. |
## Variable Comparisons
Add `i` or `c` between `-` and operator for case insensitive or case sensitive, respectively.
| Expression     | What does it do?          |
|----------------|---------------------------|
| `var -eq var2` | Equal to.                 |
| `var -ne var2` | Not equal to.             |
| `var -lt var2` | Less than.                |
| `var -le var2` | Less than or equal to.    |
| `var -gt var2` | Greater than.             |
| `var -ge var2` | Greater than or equal to. |
# ARITHMETIC OPERATORS
## Assignment
| Operators | What does it do?                              |
|-----------|-----------------------------------------------|
| `=`       | Initialize or change the value of a variable. |
## Arithmetic
| Operators          | What does it do?                                |
|--------------------|-------------------------------------------------|
| `+`                | Addition                                        |
| `-`                | Subtraction                                     |
| `*`                | Multiplication                                  |
| `/`                | Division                                        |
| `[Math]::Pow(x,y)` | Exponentiation                                  |
| `%`                | Modulo                                          |
| `+=`               | Plus-Equal (*Increment a variable.*)            |
| `-=`               | Minus-Equal (*Decrement a variable.*)           |
| `*=`               | Times-Equal (*Multiply a variable.*)            |
| `/=`               | Slash-Equal (*Divide a variable.*)              |
| `%=`               | Mod-Equal (*Remainder of dividing a variable.*) |
## Bitwise
| Operators | What does it do?     |
|-----------|----------------------|
| `-shl`    | Bitwise Left Shift   |
| `-shr`    | Bitwise Right Shift  |
| `-band`   | Bitwise AND          |
| `-bor`    | Bitwise OR           |
| `-bnot`   | Bitwise NOT (32 bit) |
| `-bxor`   | Bitwise XOR          |
## Logical
| Operators  | What does it do? |
|------------|------------------|
| `-not`,`!` | NOT              |
| `-and`     | AND              |
| `-or`      | OR               |
| `-xor`     | XOR              |
