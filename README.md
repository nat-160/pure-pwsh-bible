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
            if($extract -and $_ -ne $Closing){
                $_
            }
        if($_ -eq $Opening){$extract=$True}
        if($_ -eq $Closing){$extract=$False}
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
