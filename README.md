# Collect-diagnostic-information-for-remote-Windows-System
Collect diagnostic information for remote Windows System

# Define the folder path on the local machine
$folderPath = "C:\temp\information"
# Create the folder if it doesn't exist
if (-not (Test-Path -Path $folderPath -PathType Container)) {
 New-Item -Path $folderPath -ItemType Directory
}
# Define the list of server names or IP addresses
$servers = Get-Content -Path "C:\Path\To\ServerList.txt"
# Define the remote server's credentials (replace with actual credentials)
$credential = Get-Credential
# Loop through the list of servers and run the script on each server
foreach ($server in $servers) {
 # Use Invoke-Command to run commands on the remote server
 Invoke-Command -ComputerName $server -Credential $credential -ScriptBlock {
 param (
 $folderPath
 )
 # Change the current location to the folder on the remote server
 Set-Location -Path $folderPath
 # Export computer information to computerinfo.txt on the remote server
 Get-ComputerInfo -Property WindowsBuildLabEx,WindowsEditionID | Out-File -FilePath "computerinfo.txt"
 # Export system information to systeminfo.txt on the remote server
 systeminfo.exe | Out-File -FilePath "systeminfo.txt"
 # Export network information to ipconfig.txt on the remote server
 ipconfig /all | Out-File -FilePath "ipconfig.txt"
 } -ArgumentList $folderPath
}
# Return to the original location on the local machine
Set-Location -Path $PSScriptRoot
