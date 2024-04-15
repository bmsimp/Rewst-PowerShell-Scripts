### Create data obj to store results ###

$PS_Results = [System.Collections.Generic.List[object]]::new()

#Check if NuGet installed and force install if not
Get-PackageProvider -Name "NuGet" -ForceBootstrap
$PS_Results.Add('NuGet either existed as a Package Provider or was added')

#Set PSGallery as trusted repository to bypass prompts before installing modules from source
Set-PSRepository PSGallery -InstallationPolicy Trusted
$PS_Results.Add('PSGallery set as trusted repository')

# Check if PSWindowsUpdate module is installed
$moduleInstalled = Get-Module -Name PSWindowsUpdate -ListAvailable

if ($moduleInstalled) {
    $PS_Results.Add('PSWindowsUpdate module is installed.')
} else {
    Install-Module -Name PSWindowsUpdate -Force
    $PS_Results.Add('PSWindowsUpdate module has been successfully installed.')
}

#Ensure PSWindowsUpdate is imported to session
Import-Module -Name PSWindowsUpdate
$PS_Results.Add('PSWindowsUpdate module has been successfully imported.')

$RunWindowsUpdates = Install-WindowsUpdate -AcceptAll
$PS_Results.Add($RunWindowsUpdates)

### Post results back to Rewst ###
$postData = $PS_Results | ConvertTo-Json
Invoke-RestMethod -Method 'Post' -Uri $post_url -Body $postData -ContentType 'application/json'
