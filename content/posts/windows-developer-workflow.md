---
title: "Windows Developer Workflow"
date: 2023-03-08T04:59:48Z
draft: false
---

PowerShell optimizations for faster project switching.
Hardcoded path to work folder, but then just press Ctrl+f to navigate to that folder.

```
function GoToWork {
    GoToPath -Path C:\work\
}

Set-Alias work -value GoToWork

Set-PSReadlineKeyHandler -Chord Ctrl+f -ScriptBlock {
    [Microsoft.PowerShell.PSConsoleReadLine]::RevertLine()
    [Microsoft.PowerShell.PSConsoleReadLine]::Insert('work')
    [Microsoft.PowerShell.PSConsoleReadLine]::AcceptLine()
}
```

Open .sln files in Visual Studio.
```
function OpenInVS() {
    $file = Get-ChildItem *.sln | Select-Object -ExpandProperty Name
    Invoke-expression ".\$file"
}

Set-Alias vs -value OpenInVS
```

Now if I could only get [winsessionizer](https://github.com/jawee/winsessionizer) to work...
