# pure-pwsh-bible
A rewrite of [https://github.com/dylanaraps/pure-bash-bible] in PowerShell.

# STRINGS

## Trim leading and trailing white-space from string
**Example Function:**
```powershell
function trim_string {
    # Usage: trim_string "   example   string    "
    # "   example   string    " | trim_string
    $input+$args | % {
        $_.Trim()
    }
}
```
**Example Usage:**
```shell
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
```powershell
function trim_all {
    # Usage: trim_all "   example   string    "
    # "   example   string    " | trim_all
    $input+$args | % {
        [regex]::Replace($_.Trim(),"\s+"," ")
    }
}
```
**Example Usage:**
```shell
PS>trim_all "    Hello,    World    "
Hello, World

PS>$name="   John   Black  is     my    name.    "
PS>$name | trim_all
John Black is my name.
```
## Use regex on a string
**Example Function:**
```powershell
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
```shell
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
```shell
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
```powershell
function split($Delimiter){
    # Usage: split "delimiter" "string"
    # split "string" -Delimiter "delimiter"
    # "string" | split "delimiter"
    ($input+$args) -split $Delimiter
}
```
**Example Usage:**
```shell
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
```powershell
function lower {
    # Usage: lower "string"
    # "string" | lower
    $input+$args | % {
        $_.ToLower()
    }
}
```
**Example Usage:**
```shell
PS>lower "HELLO"
hello

PS>lower "HeLlO"
hello

PS>"hello".ToLower()
hello
```
## Change a string to uppercase
**Example Function:**
```powershell
function upper {
    # Usage: upper "string"
    # "string" | upper
    $input+$args | % {
        $_.ToUpper()
    }
}
```
**Example Usage:**
```shell
PS>upper "hello"
HELLO

PS>upper "HeLlO"
HELLO

PS>"HELLO".ToUpper()
HELLO
```
## Reverse a string case
**Example Function:**
```powershell
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
```shell
PS>reverse_case "hello"
HELLO

PS>"HeLlO" | reverse_case
hElLo

PS>reverse_case "HELLO"
hello
```
## Trim quotes from a string
**Example Function:**
```powershell
function trim_quotes {
    # Usage: trim_quotes "string"
    # "string" | trim_quotes
    $input+$args | % {
        $_.Replace("'","").Replace("`"","")
    }
}
```
**Example Usage:**
```shell
PS>$var="'Hello', `"World`""
PS>trim_quotes $var
Hello, World
```
## Strip all instances of pattern from string
**Example Function:**
```powershell
function strip_all($Pattern) {
    # Usage: strip_all "pattern" "string"
    # "string" | strip_all "pattern"
    $input+$args | % {
        [regex]::Replace($_,$Pattern,"")
    }
}
```
**Example Usage:**
```shell
PS>strip_all "[aeiou]" "The Quick Brown Fox"
Th Qck Brwn Fx

PS>strip_all "The Quick Brown Fox" -Pattern " "
TheQuickBrownFox

PS>"The Quick Brown Fox" | strip_all "Quick "
The Brown Fox
```
## Strip first occurrence of pattern from string
**Example Function:**
```powershell
function strip {
    # Usage: strip "pattern" "string"
    # "string" | strip "pattern"
    $input+$args | % {
        [regex]::new($Pattern).Replace($_,"",1)
    }
}
```
**Example Usage:**
```shell
PS>strip "[aeiou]" "The Quick Brown Fox"
Th Quick Brown Fox

PS>"The Quick Brown Fox" | strip " "
TheQuick Brown Fox
```
## Strip pattern from start of string
**Example Function:**
```powershell
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
```shell
PS>lstrip "The " "The Quick Brown Fox"
Quick Brown Fox
```
## Strip pattern from end of string
**Example Function:**
```powershell
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
```shell
PS>rstrip " Fox" "The Quick Brown Fox"
The Quick Brown
```
## Percent-encode a string
**Example Function:**
```powershell
function urlencode {
    # Usage: urlencode "string"
    # "string" | urlencode
    $input+$args | % {
        [System.Web.HTTPUtility]::UrlEncode($_)
    }
}
```
**Example Usage:**
```shell
PS>urlencode "https://github.com/n4t-dog/pure-pwsh-bible"
https%3a%2f%2fgithub.com%2fn4t-dog%2fpure-pwsh-bible
```
## Decode a percent-encoded string
**Example Function:**
```powershell
function urldecode {
    # Usage: urldecode "string"
    # "string" | urldecode
    $input+$args | % {
        [System.Web.HTTPUtility]::UrlDecode($_)
    }
}
```
**Example Usage:**
```shell
PS>urlencode "https%3a%2f%2fgithub.com%2fn4t-dog%2fpure-pwsh-bible"
https://github.com/n4t-dog/pure-pwsh-bible
```
## Check if string contains a sub-string
**Using a test:**
```shell
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
```shell
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
```shell
if ($var.StartsWith("sub_string*")) {
    "var starts with sub_string."
}

# Inverse (var does not start with sub_string).
if ($var -notlike "sub_string*") {
    "var does not start with sub_string."
}
```
## Check if string ends with sub-string
```shell
if ($var.EndsWith("sub_string")) {
    "var ends with sub_string."
}

  # Inverse (var does not end with sub_string).
if ($var -notlike "*sub_string") {
    "var does not end with sub_string."
}
```
