# Adicionar diretórios ao PATH pelo PowerShell

O `PATH` informa ao Windows onde procurar comandos como `pip`, `python` e `git`.

## Adicionar o `pip` ao PATH

Execute no PowerShell (sem administrador):

```powershell
# Localiza a pasta Scripts do Python 3.
$pipPath = py -3 -c "import sysconfig; print(sysconfig.get_path('scripts'))"

# Lê o PATH permanente do usuário.
$userPath = [Environment]::GetEnvironmentVariable('Path', 'User')

# Salva a pasta no PATH apenas se não existir.
if (($userPath -split ';') -notcontains $pipPath) {
    [Environment]::SetEnvironmentVariable('Path', (($userPath, $pipPath) | Where-Object { $_ }) -join ';', 'User')
}

# Atualiza a sessão atual do terminal.
if (($env:Path -split ';') -notcontains $pipPath) {
    $env:Path += ";$pipPath"
}
```

**Entendendo o código:** <br>
`$pipPath` encontra a pasta;<br>
`$userPath` lê o PATH;<br>
`-split ';'` separa os diretórios;<br>
`-notcontains` evita duplicatas;<br> 
`SetEnvironmentVariable` salva a alteração;<br> 
`$env:Path` atualiza o terminal aberto.

## Adicionar qualquer outra pasta

Substitua o exemplo por uma pasta **existente** que contenha o executável:

```powershell
$novoDiretorio = 'C:\Ferramentas\MinhaCLI\bin'
if (-not (Test-Path -LiteralPath $novoDiretorio -PathType Container)) { throw 'Pasta não encontrada.' }

$userPath = [Environment]::GetEnvironmentVariable('Path', 'User')
if (($userPath -split ';') -notcontains $novoDiretorio) {
    [Environment]::SetEnvironmentVariable('Path', (($userPath, $novoDiretorio) | Where-Object { $_ }) -join ';', 'User')
}
if (($env:Path -split ';') -notcontains $novoDiretorio) { $env:Path += ";$novoDiretorio" }
```

## Verificar

```powershell
pip --version
py -3 -m pip --version   # Alternativa sem depender do PATH
Get-Command pip          # Mostra o executável encontrado
```

> **Segurança:** altere o PATH do **usuário** (`'User'`), não o do sistema; evite diretórios desconhecidos. Adicionar uma pasta ao PATH não instala programas nem cria `pip.exe` ou `pip3.exe`.
