# pure-pwsh-bible
A collection of pure pwsh alternatives to external processes.

# STRINGS

## Trim leading and trailing white-space from string
```powershell
PS> "    Hello,  World    ".Trim()
Hello, World

PS> $name = "   John Black  "
PS> $name.Trim()
John Black
```