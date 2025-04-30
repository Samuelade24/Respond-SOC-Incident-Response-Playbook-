# Respond-SOC-Incident-Response-Playbook-
Task: SOC playbook for credential theft (aligned with Splunk in your report).
# PowerShell script to isolate a compromised host
Stop-Service -Name "WinRM" -Force  # Disable remote access
Invoke-Command -ComputerName TARGET -ScriptBlock {
    Get-NetTCPConnection -State Established | Where-Object { $_.RemotePort -eq 80 } | Stop-Process -Force
}
