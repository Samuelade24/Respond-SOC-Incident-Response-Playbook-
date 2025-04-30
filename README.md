Project Description

XSS Detector is a PowerShell-based security tool that identifies and demonstrates Reflected Cross-Site Scripting (XSS) vulnerabilities in web applications. This utility tests target endpoints for XSS susceptibility by injecting various payloads and analyzing responses.

Languages and Utilities Used:

PowerShell (Primary scripting language)
cURL/Invoke-WebRequest (HTTP request handling)
HTML/JavaScript (XSS payload generation)

Environments Used:

Windows 10
Linux (With PowerShell Core installed)

Features:

Automated XSS payload injection
Multiple payload variations testing
Risk assessment and reporting
Safe demonstration mode (non-malicious alerts)
Support for GET/POST parameter testing


Program Walk-through:


Installation:

# Clone the repository
git clone https://github.com/yourusername/xss-detector.git
cd xss-detector

# Run the tool
.\XSSDetector.ps1

Usage Examples

Basic scan:
.\XSSDetector.ps1 -Url "http://testphp.vulnweb.com/search"

Comprehensive test with all payloads:
.\XSSDetector.ps1 -Url "http://testphp.vulnweb.com/search" -TestAllPayloads

Test specific parameter:
.\XSSDetector.ps1 -Url "http://testphp.vulnweb.com/search" -Parameter "query"

Sample Code Structure:

# XSSDetector.ps1

param(
    [string]$Url,
    [string]$Parameter = "query",
    [switch]$TestAllPayloads,
    [switch]$GenerateReport
)

# Import modules
. .\modules\xss-payloads.ps1
. .\modules\http-request.ps1
. .\modules\report-generator.ps1

function Main {
    Write-Host "=== XSS Detector - Reflected XSS Vulnerability Scanner ===" -ForegroundColor Cyan
    
    # Validate URL
    if (-not $Url) {
        $Url = Read-Host "Enter target URL (e.g., http://example.com/search)"
    }
    
    # Generate payloads
    $payloads = Get-XSSPayloads -All:$TestAllPayloads
    
    # Test each payload
    $results = @()
    foreach ($payload in $payloads) {
        $response = Invoke-TestRequest -Url $Url -Parameter $Parameter -Payload $payload
        $results += [PSCustomObject]@{
            Payload = $payload
            IsVulnerable = $response.Contains($payload)
            ResponseCode = $response.StatusCode
        }
    }
    
    # Generate report
    if ($GenerateReport) {
        New-Report -Results $results -Url $Url
    }
    
    # Show summary
    Show-ResultsSummary -Results $results
}

Main

Security Considerations:

Only test against systems you own or have permission to scan
Default payloads are non-destructive (alert-based)
Includes rate limiting to avoid overwhelming servers
Clearly marks vulnerable endpoints in reports

Impact Assessment:
Vulnerability	Risk Level	Potential Impact
Reflected XSS	High	Session hijacking, phishing, malware delivery
Unfiltered Input	Medium	Defacement, limited script execution
Partial Encoding	Low	Possible exploitation under specific conditions
