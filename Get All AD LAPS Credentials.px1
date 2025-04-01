# Convert credentials to PSCredentials object
$securePassword = ConvertTo-SecureString -String {{ CTX.domain_admin_password }} -AsPlainText -Force
$credential = New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList {{ CTX.domain_admin_username }}, $securePassword

# Retrieve all computer objects
$computers = Get-ADComputer -Filter *

# Create an ArrayList to store the computer information
$computerInfo = New-Object System.Collections.ArrayList

# Iterate through each computer object
foreach ($computer in $computers) {
    $computerDN = $computer.DistinguishedName

    # Retrieve the SID for the computer
    $computerSID = $computer.SID.Value

    # Create a custom object for computer information
    $computerObject = [PSCustomObject]@{
        Name = $computer.Name
        SID = $computerSID
    }

    # Add the computer object to the ArrayList
    [void]$computerInfo.Add($computerObject)
}

# Create an ArrayList to store the LAPS information
$lapsInfo = New-Object System.Collections.ArrayList

# Iterate through each computer object
foreach ($computer in $computers) {
    $computerDN = $computer.DistinguishedName

    # Perform the Get-LapsADPassword command using the DistinguishedName as -Identity
    $result = Get-LapsADPassword -Identity $computerDN -AsPlainText -DecryptionCredential $credential

    # Add the LAPS information to the ArrayList
    [void]$lapsInfo.Add($result)
}

# Convert the computer information and credential information to separate JSON objects
$computerJson = $computerInfo | ConvertTo-Json
$credentialJson = $lapsInfo | ConvertTo-Json

# Create a hash table to store the final JSON objects
$jsonObject = @{
    Computers = $computerJson
    Credentials = $credentialJson
}

# Convert the LAPS information to a JSON object and POST back to Rewst
$postData = $jsonObject | ConvertTo-Json
Invoke-RestMethod -Method 'Post' -Uri $post_url -Body $postData -ContentType 'application/json; charset=utf-8'
