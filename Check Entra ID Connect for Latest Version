### Create data obj to store results ###
$PS_Results = [System.Collections.Generic.List[object]]::new()

### Get Current Version of Microsoft Entra Connect Sync
$CurrentVersion = Get-WmiObject -Class Win32_Product | Where-Object { $_.name -eq "Microsoft Entra Connect Sync" } | Select-Object Name, Version
$PS_Results.add($CurrentVersion)

### Post results back to Rewst ###
$postData = $PS_Results | ConvertTo-Json
Invoke-RestMethod -Method 'Post' -Uri $post_url -Body $postData -ContentType 'application/json'