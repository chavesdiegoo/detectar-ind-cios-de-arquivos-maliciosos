# detectar-indicios-de-arquivos-maliciosos
Script que monitora pastas sensíveis
✔ Identifica arquivos recém-criados ou modificados
✔ Procura comportamentos típicos de malware, como: extensões suspeitas nomes usados por worms/trojans  arquivos executáveis em locais errados persistência em Startup 
✔ Roda em segundo plano 
🧠 SCRIPT – Monitoramento em Segundo Plano

👉 Salve como:
monitor-malware.ps1
# ===============================
# Monitoramento de Arquivos Suspeitos
# Autor: Diego Chaves
# ===============================

$WatchPaths = @(
    "$env:APPDATA",
    "$env:LOCALAPPDATA",
    "$env:TEMP",
    "$env:USERPROFILE\Downloads",
    "$env:APPDATA\Microsoft\Windows\Start Menu\Programs\Startup"
)

$SuspiciousExtensions = @(
    ".exe", ".js", ".vbs", ".ps1", ".bat", ".cmd", ".scr", ".dll"
)

$SuspiciousNames = @(
    "svchost", "update", "installer", "setup", "chrome_update",
    "winupdate", "system32", "runtime", "taskhost"
)

$LogFile = "$env:LOCALAPPDATA\security_monitor.log"

function Log-Suspicious {
    param ($Message)
    $timestamp = Get-Date -Format "yyyy-MM-dd HH:mm:ss"
    "$timestamp - $Message" | Out-File -Append -FilePath $LogFile
}

foreach ($Path in $WatchPaths) {
    if (Test-Path $Path) {
        $Watcher = New-Object System.IO.FileSystemWatcher
        $Watcher.Path = $Path
        $Watcher.IncludeSubdirectories = $true
        $Watcher.EnableRaisingEvents = $true

        Register-ObjectEvent $Watcher Created -Action {
            $file = $Event.SourceEventArgs.FullPath
            $ext = [System.IO.Path]::GetExtension($file).ToLower()
            $name = [System.IO.Path]::GetFileNameWithoutExtension($file).ToLower()

            if ($SuspiciousExtensions -contains $ext) {
                foreach ($badname in $SuspiciousNames) {
                    if ($name -like "*$badname*") {
                        Log-Suspicious "ARQUIVO SUSPEITO CRIADO: $file"
                    }
                }
            }
        }
    }
}

Log-Suspicious "Monitoramento iniciado com sucesso."
while ($true) { Start-Sleep -Seconds 30 }

▶️ Como executar em segundo plano

Abra PowerShell como Administrador e rode:

Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy Bypass
Start-Process powershell -ArgumentList "-WindowStyle Hidden -File `"$PWD\monitor-malware.ps1`""


Ele vai rodar oculto, em background.
