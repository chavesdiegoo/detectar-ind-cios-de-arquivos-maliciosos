# detectar-indicios-de-arquivos-maliciosos
Script que monitora pastas sensíveis
✔ Identifica arquivos recém-criados ou modificados
✔ Procura comportamentos típicos de malware, como: extensões suspeitas nomes usados por worms/trojans  arquivos executáveis em locais errados persistência em Startup 
✔ Roda em segundo plano 
🧠 SCRIPT – Monitoramento em Segundo Plano

🔐 O que esse script faz

✔ Monitora pastas sensíveis
✔ Identifica arquivos recém-criados ou modificados
✔ Procura comportamentos típicos de malware, como:
  • extensões suspeitas
  • nomes usados por worms/trojans
  • arquivos executáveis em locais errados
  • persistência em Startup

✔ Roda em segundo plano
✔ Gera log para auditoria

⚠️ Ele não remove nada, apenas detecta e alerta (boa prática).


📁 Pastas monitoradas

AppData\Roaming
AppData\Local
Temp
Startup
Downloads
🧠 SCRIPT – Monitoramento em Segundo Plano

👉 Baixe o arquivo: 
monitor-malware.ps1
Salve em uma pasta de sua preferencia. 

▶️ Como executar em segundo plano
Abra PowerShell como Administrador e rode:
Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy Bypass
Start-Process powershell -ArgumentList "-WindowStyle Hidden -File `"$PWD\monitor-malware.ps1`""
