# DevOpsDays 2020 (Live from Bogota '30_Jul_2020')

# Visual Studio Code y Azure

En este repositorio exploraremos Visual studio Code, desde una perspectiva dinamica y divertida, es como todas las historias de aventuras de D&D, o del señor de los anillos, estamos todos reunidos en la mayor comodidad, tranquilos disfrutando de la tarde, y el destino nos va a llegar nuevo un "Quest", [una busqueda o reto](https://code.visualstudio.com/docs), en este caso vamos a acompañar a este valiente explorador, de abultado vientre, y corta vista, en un terreno poco explorado, como lo es el uso de las diferentes plataformas de administracion de la nube como las lineas de comando y particularmente el vscode para gestionar plantillas ARM.

## Instalacion de prerequisitos

Si usted no tiene una suscripcion de Azure haga clic [aqui](https://azure.microsoft.com/es-mx/free/), necesitara un equipo con Windows 10 preferiblemente, y lo invito a que preparemos los pre-requisitos:

- Primero que todo una enorme taza del café de su preferencia

- El modulo de Azure para powershell, aunque se puede administrar Azure desde el [portal web](https://portal.azure.com) cual seria la gracia, este modulo AZ se puede instalar iniciando el PowerShell en modo administrador (es decir dando clic derecho sobre el icono de PowerShell), y usando los siguientes comandos:

```PowerShell
$PSVersionTable.PSVersion #para verificar que version de Powershell se tiene con version 5.0 o superior podemos hacer uso de

Install-Module -Name Az #Esto tomara un rato, porque son varios modulos, tambien es posible que te solicite confirmacion de que confias en el repositorio de PSGallery antes de continuar.

```
Para mas informacion consulte [aqui](https://docs.microsoft.com/en-us/powershell/azure/install-az-ps?view=azps-4.4.0)

- El Azure CLI tambien vale la pena tenerlo, vamos a instalarlo desde [aqui](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli-windows?view=azure-cli-latest&tabs=azure-cli) esta es otra manera de interactuar con Azure desde PowerShell como administrador ejecutaremos

```PowerShell
Invoke-WebRequest -Uri https://aka.ms/installazurecliwindows -OutFile .\AzureCLI.msi; Start-Process msiexec.exe -Wait -ArgumentList '/I AzureCLI.msi /quiet'; rm .\AzureCLI.msi
```

- Es posible ingresar a la tienda de Microsoft Store, y descargar el [Windows Terminal](https://www.microsoft.com/en-us/p/windows-terminal/9n0dx20hk701?activetab=pivot:overviewtab) 

- Tambien puedes querer descargar el WSL (Windows Subsystem for Linux) que permite la ejecucion de un linux Ubuntu dentro de tu sistema operativo, para tener acceso por BASH y poder ejecutar herramientas propias de linux, para instalar el [WSL](https://docs.microsoft.com/en-us/windows/wsl/install-win10#update-to-wsl-2)

- Descargue el Visual Studio Code [desde aqui](https://code.visualstudio.com/download) el instalador es muy liviano, pesa menos de 100MB. la instalacion es super simple, y vamos a ir complementandola con las extensiones para Azure a lo largo de este workshop
```Powershell
$VSCode = "https://code.visualstudio.com/docs/?dv=win"
New-FileDownload -SourceFile $VSCode
```

## Guia de laboratorio

### Visual Studio Code
Empecemos reconociendo el terreno, 

"VS Code viene con un diseño simple e intuitivo que maximiza el espacio provisto para el editor mientras deja un amplio espacio para navegar y acceder al contexto completo de su carpeta o proyecto. La IU se divide en cinco áreas:

- Editor: el área principal para editar sus archivos. Puede abrir tantos editores como desee, uno al lado del otro, vertical y horizontalmente.
- Barra lateral: contiene diferentes vistas como el Explorador para ayudarlo mientras trabaja en su proyecto.
- Barra de estado: información sobre el proyecto abierto y los archivos que edita.
- Barra de actividad: ubicada en el extremo izquierdo, le permite cambiar de vista y le brinda indicadores adicionales específicos del contexto, como la cantidad de cambios salientes cuando Git está habilitado.
- Paneles: puede mostrar diferentes paneles debajo de la región del editor para obtener información de salida o depuración, errores y advertencias, o un terminal integrado. El panel también se puede mover hacia la derecha para obtener más espacio vertical." tomado de:(https://code.visualstudio.com/docs/getstarted/userinterface#_basic-layout)

### ahora el Tiki-tiki
Para empezar a probar que todo nos este funcionando, usaremos la barra lateral, para crear un nuevo archivo. Quiero que lo llamemos *test.ps1*, esta extension hara que el Visual Studio Code identifique que estas intentando crear un Script de PowerShell, por lo que en el modulo de Extensiones te solicitara la instalacion de la extension de PowerShell, en mi escenario utilice la de "PowerShell preview"-

vamos a abrir la consola de PowerShell dentro del vscode, en la seccion inferior aparecen cuatro pestañas, "Problemas", "Salidas", "Consola de Depuracion", y "Terminal" esta ultima es la que me interesa, y vamos a validar que en la parte de la derecha aparece indicando que esta en modo PowerShell, porque tambien puede que esta consola nos funcione para bash.-

```Powershell
Connect-AzAccount #Aqui te pedira iniciar sesion sobre el portal de https://microsoft.com/devicelogin 
get-azresource |ft
```
## PowerShell en 5 minutos
```Powershell
Write-Host #"Powershell tiene una estructura sencilla, es una composicion de un verbo en infinitivo, un guion al medio y un sustantivo singular" -ForegroundColor Yellow
Write-Host #"Por ejemplo podemos encontrar un Get-Command, un Get-Help, o un Get-Service en mas de 800 cmd-lets" -ForegroundColor Yellow
Get-Command
Write-host #"Si queremos saber que esta haciendo nuestro equipo, hacemos un get-process" -ForegroundColor Yellow
Get-process
Write-Host #"Si la informacion es muy extensa, como en este caso, podemos ordenarla" -ForegroundColor Yellow
Update-Help
get-help Get-Process
get-help Get-Process -full
get-help Get-Process -detailed
get-help Get-Process -examples
get-help Get-Process -Online
Get-Service
Get-Service -Name Spooler |Stop-Service
Get-Service M* |Format-List
```
## Command-lets y Parametros
```Powershell
Get-Service M* |Format-Custom
Get-Service M* |Where-Object {$_.Status -eq "Running"}
Get-Service M* |Where-Object {$_.Status -eq "Stopped"}
Get-Service | Sort-Object Status
Get-Service | Sort-Object Status |Format-Table -GroupBy Status Name, DisplayName
Get-Service |Get-Member
Get-Process |Get-Member
Get-Service Spooler |Select-Object ServicesDependedOn
Show-Command Get-Service
Stop-Service M* -WhatIf
Stop-Service M* -Confirm
```

## Pasando resultados con el Pipe o Piping
```Powershell
Get-Process |sort CPU -Descending
Get-EventLog -LogName System -EntryType Error -Newest 10
Get-EventLog -LogName System -EntryType Error -Newest 10 |fl
Get-EventLog -LogName System -EntryType Error -Newest 10 |ogv
```

## Variables y Scripts
```Powershell
$var = "Hola DevOps2020"
$var
Write-host "$var" -ForegroundColor Yellow
Get-Variable 
#podemos manejar strings
$num = 123
$num
#Podemos contener listas o arrays tambien
$num = 1,2,3
$num
$var.GetType().FullName
$var.Length
$var[1]
[int[]] $var = (1,2,3)
$var[2] = "1234"
$var[2]
$v1 = "Hello "
$v2 = "world"
$v1+$v2
($var1+$var2).Length
#Comparar con -eq -ne -gt -lt -ge -le
"Juan Perez" -eq "Ramon Valdez"
#formatos
"{0:f2}" -f 12.4
"|{0,10:C}|" -f 12.4
"{0:hh:mm}" -f (get-date)
#Scripting
Get-ExecutionPolicy
Set-ExecutionPolicy Unrestricted
Get-ExecutionPolicy
runas /user:username PowerShell
##Mas info sobre seguridad en PoSh http://technet.microsoft.com/en-us/magazine/2007.09.powershell.aspx
#------------------------
#test.ps1
#Show Hello and time
"" #Blank line
"Hello"+$Env:USERNAME +"!"
"Time is" + "{0:HH}:{0:mm}" -f (get-Date)
"" #blank line
#------------------------
#Funciones
function get-soup (
    [switch] $please,
    [String] $soup = "chicken noodle"
    )
{
    if ($please){
        "Here's your $soup soup"
    }
    else {
        "No soup for you!"
    }
}
#------------------------
```

## Entornos y modulos
```Powershell
#---------
$User = "fabian.campo@MiDominio.com"
$Pass = ConvertTo-SecureString "AzurePa55w.rd" -AsPlainText -Force
$Cred = New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $User, $Pass
Import-Module MSOnline
Connect-msolservice -Credential $Cred
$Session = New-PSSession -ConfigurationName Microsoft.Exchange -ConnectionUri https://ps.outlook.com/powershell/ -Credential $Cred -Authentication Basic -AllowRedirection
Import-PSSession $Session
Import-Module LyncOnlineConnector
import-module microsoft.online.sharepoint.powershell
$Session = New-CsOnlinesession -Credential $Cred
Import-PSSession $Session
[string]$tenant = get-msoldomain | where {$_.name -like "*.onmicrosoft.com" -and -not($_.name -like "*mail.onmicrosoft.com")} | select name
$tenant
$tenant3 = $tenant -split("=")
[string]$tenant4 = $tenant3[1]
$tenant4
$tenant5 = $tenant4 -split(".on")
[string]$tenant6 = $tenant5[0]
$url = "https://" + $tenant6 + "-admin.sharepoint.com"
Connect-SPOService -Url $url -credential $Cred  
#---------
```
## Registro, Certificados y otros almacenes

```Powershell
Get-psprovider
Get-psDrive
New-PsDrive -Name HKCS -PSProvider Registry -Root "HKEY_CLASSES_ROOT"
cd HKCS:
dir .ht*
$process = Get-WmiObject -Class win32_Process
$item.name
foreach ($item in $process)
{
    $item.name
}
```

## Creando Maquinas virtuales en Azure
```Powershell
Connect-AzAccount
Get-AZSubscription | Sort SubscriptionName | Select SubscriptionName
$subscrName="Plataformas de MSDN"
Select-AzSubscription -SubscriptionName $subscrName
$rgName="Devops2020_rg"
$locName="eastus"
New-AZResourceGroup -Name $rgName -Location $locName
Get-AZStorageAccountNameAvailability "campoh1473"
$rgName="Devops2020_rg"
$saName="campoh1473"
$locName=(Get-AZResourceGroup -Name $rgName).Location
New-AZStorageAccount -Name $saName -ResourceGroupName $rgName -Type Standard_LRS -Location $locName
$rgName="Devops2020_rg"
$locName=(Get-AZResourceGroup -Name $rgName).Location
$vnetName="Vnet01"
$SubnetName="Subnet01"
$Subnet=New-AZVirtualNetworkSubnetConfig -Name $SubnetName -AddressPrefix 10.0.0.0/27
New-AZVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $locName -AddressPrefix 10.0.0.0/24 -Subnet $Subnet -DNSServer 10.0.0.4
$rule1 = New-AZNetworkSecurityRuleConfig -Name "RDPTraffic" -Description "Allow RDP to all VMs on the subnet" -Access Allow -Protocol Tcp -Direction Inbound -Priority 100 -SourceAddressPrefix Internet -SourcePortRange * -DestinationAddressPrefix * -DestinationPortRange 3389
$rule2 = New-AZNetworkSecurityRuleConfig -Name "ExchangeSecureWebTraffic" -Description "Allow HTTPS to the Exchange server" -Access Allow -Protocol Tcp -Direction Inbound -Priority 101 -SourceAddressPrefix Internet -SourcePortRange * -DestinationAddressPrefix "10.0.0.5/32" -DestinationPortRange 443
$rule3 = New-AZNetworkSecurityRuleConfig -Name "SharePointSecureWebTraffic" -Description "Allow HTTPS to the SharePoint server" -Access Allow -Protocol Tcp -Direction Inbound -Priority 102 -SourceAddressPrefix Internet -SourcePortRange * -DestinationAddressPrefix "10.0.0.7/32" -DestinationPortRange 80
New-AZNetworkSecurityGroup -Name $SubnetName -ResourceGroupName $rgName -Location $locName -SecurityRules $rule1, $rule2, $rule3
$vnet=Get-AZVirtualNetwork -ResourceGroupName $rgName -Name $vnetName
$nsg=Get-AZNetworkSecurityGroup -Name $SubnetName -ResourceGroupName $rgName
Set-AZVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name $SubnetName -AddressPrefix "10.0.0.0/27" -NetworkSecurityGroup $nsg
$vnet | Set-AzVirtualNetwork
$rgName="Devops2020_rg"
# Create an availability set for domain controller virtual machines
New-AZAvailabilitySet -ResourceGroupName $rgName -Name dcAvailabilitySet -Location $locName -Sku Aligned  -PlatformUpdateDomainCount 5 -PlatformFaultDomainCount 2
# Create the domain controller virtual machine
$vmName="adVM"
$vmSize="Standard_D1_v2"
$vnet=Get-AZVirtualNetwork -Name $vnetName -ResourceGroupName $rgName
$pip = New-AZPublicIpAddress -Name ($vmName + "-PIP") -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic
$nic = New-AZNetworkInterface -Name ($vmName + "-NIC") -ResourceGroupName $rgName -Location $locName -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id -PrivateIpAddress 10.0.0.4
$avSet=Get-AZAvailabilitySet -Name dcAvailabilitySet -ResourceGroupName $rgName
$vm=New-AZVMConfig -VMName $vmName -VMSize $vmSize -AvailabilitySetId $avSet.Id
$vm=Set-AZVMOSDisk -VM $vm -Name ($vmName +"-OS") -DiskSizeInGB 128 -CreateOption FromImage -StorageAccountType "Standard_LRS"
$diskConfig=New-AZDiskConfig -AccountType "Standard_LRS" -Location $locName -CreateOption Empty -DiskSizeGB 20
$dataDisk1=New-AZDisk -DiskName ($vmName + "-DataDisk1") -Disk $diskConfig -ResourceGroupName $rgName
$vm=Add-AZVMDataDisk -VM $vm -Name ($vmName + "-DataDisk1") -CreateOption Attach -ManagedDiskId $dataDisk1.Id -Lun 1
$cred=Get-Credential -Message "Type the name and password of the local administrator account for adVM."
$vm=Set-AZVMOperatingSystem -VM $vm -Windows -ComputerName adVM -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
$vm=Set-AZVMSourceImage -VM $vm -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
$vm=Add-AZVMNetworkInterface -VM $vm -Id $nic.Id
New-AZVM -ResourceGroupName $rgName -Location $locName -VM $vm
## conecte al adVM y ejecute
$disk=Get-Disk | where {$_.PartitionStyle -eq "RAW"}
$diskNumber=$disk.Number
Initialize-Disk -Number $diskNumber
New-Partition -DiskNumber $diskNumber -UseMaximumSize -AssignDriveLetter
Format-Volume -DriveLetter F
Install-WindowsFeature AD-Domain-Services -IncludeManagementTools
Install-ADDSForest -DomainName campoh.lab -DatabasePath "F:\NTDS" -SysvolPath "F:\SYSVOL" -LogPath "F:\Logs"
Add-WindowsFeature RSAT-ADDS-Tools
New-ADUser -SamAccountName sp_farm_db -AccountPassword (read-host "Set user password" -assecurestring) -name "sp_farm_db" -enabled $true -PasswordNeverExpires $true -ChangePasswordAtLogon $false
New-ADUser -Name EXCHSVC -GivenName EXCHSVC -UserPrincipalName EXCHSVC -ChangePasswordAtLogon $false -AccountPassword (ConvertTo-SecureString -AsPlainText "p@ssw0rd" -Force) -Enabled $true
New-ADUser -Name SYSADMIN -GivenName EXCHADMIN -UserPrincipalName EXCHADMIN -ChangePasswordAtLogon $false -AccountPassword (ConvertTo-SecureString -AsPlainText "p@ssw0rd" -Force) -Enabled $true
New-ADUser -Name EFlores -GivenName "Maria Elena" -surname "Flores" -UserPrincipalName EFlores -EmailAddress EFlores@campoh.lab -ChangePasswordAtLogon $false -AccountPassword (ConvertTo-SecureString -AsPlainText "p@ssw0rd" -Force) -Enabled $true
New-ADUser -Name Vgarcia -GivenName "Victor" -surname "Garcia" -UserPrincipalName Vgarcia -EmailAddress Vgarcia@campoh.lab -ChangePasswordAtLogon $false -AccountPassword (ConvertTo-SecureString -AsPlainText "p@ssw0rd" -Force) -Enabled $true
New-ADUser -Name LRodriguez -GivenName "Lourdes" -surname "Rodriguez" -UserPrincipalName LRodriguez -EmailAddress LRodriguez@campoh.lab -ChangePasswordAtLogon $false -AccountPassword (ConvertTo-SecureString -AsPlainText "p@ssw0rd" -Force) -Enabled $true
New-ADUser -Name JLSanchez -GivenName "Jorge Luis" -surname "Sanchez" -UserPrincipalName JLSanchez -EmailAddress JLSanchez@campoh.lab -ChangePasswordAtLogon $false -AccountPassword (ConvertTo-SecureString -AsPlainText "p@ssw0rd" -Force) -Enabled $true
New-ADUser -Name CZambrano -GivenName "Cecilia" -surname "Zambrano" -UserPrincipalName CZambrano -EmailAddress CZambrano@campoh.lab -ChangePasswordAtLogon $false -AccountPassword (ConvertTo-SecureString -AsPlainText "p@ssw0rd" -Force) -Enabled $true
New-ADUser -Name MSalazar -GivenName "Mariana " -surname "Salazar" -UserPrincipalName MSalazar -EmailAddress MSalazar@campoh.lab -ChangePasswordAtLogon $false -AccountPassword (ConvertTo-SecureString -AsPlainText "p@ssw0rd" -Force) -Enabled $true
New-ADUser -Name JCTorres -GivenName "Juan Carlos" -surname "Torres" -UserPrincipalName JCTorres -EmailAddress JCTorres@campoh.lab -ChangePasswordAtLogon $false -AccountPassword (ConvertTo-SecureString -AsPlainText "p@ssw0rd" -Force) -Enabled $true
New-ADUser -Name ALAndrade -GivenName "Ana Lucia" -surname "Andrade" -UserPrincipalName ALAndrade -EmailAddress ALAndrade@campoh.lab -ChangePasswordAtLogon $false -AccountPassword (ConvertTo-SecureString -AsPlainText "p@ssw0rd" -Force) -Enabled $true
New-ADUser -Name MVera -GivenName "Miguel Angel" -surname "Vera" -UserPrincipalName MVera -EmailAddress MVera@campoh.lab -ChangePasswordAtLogon $false -AccountPassword (ConvertTo-SecureString -AsPlainText "p@ssw0rd" -Force) -Enabled $true
## Fase 2- Creamos ahora la maquina de Exchange
$vmDNSName="mail2393"
$rgName="Devops2020_rg"
$locName=(Get-AZResourceGroup -Name $rgName).Location
Test-AZDnsAvailability -DomainQualifiedName $vmDNSName -Location $locName
# Set up key variables
$subscrName="Plataformas de MSDN"
$rgName="Devops2020_rg"
$vmDNSName="mail2393"
# Set the Azure subscription
Select-AzSubscription -SubscriptionName $subscrName
# Get the Azure location and storage account names
$locName=(Get-AZResourceGroup -Name $rgName).Location
$saName=(Get-AZStorageaccount | Where {$_.ResourceGroupName -eq $rgName}).StorageAccountName
# Create an availability set for Exchange virtual machines
New-AZAvailabilitySet -ResourceGroupName $rgName -Name exAvailabilitySet -Location $locName -Sku Aligned  -PlatformUpdateDomainCount 5 -PlatformFaultDomainCount 2
# Specify the virtual machine name and size
$vmName="exVM"
$vmSize="Standard_D3_v2"
$vnet=Get-AZVirtualNetwork -Name $vnetName -ResourceGroupName $rgName
$avSet=Get-AZAvailabilitySet -Name exAvailabilitySet -ResourceGroupName $rgName
$vm=New-AZVMConfig -VMName $vmName -VMSize $vmSize -AvailabilitySetId $avSet.Id
# Create the NIC for the virtual machine
$nicName=$vmName + "-NIC"
$pipName=$vmName + "-PublicIP"
$pip=New-AZPublicIpAddress -Name $pipName -ResourceGroupName $rgName -DomainNameLabel $vmDNSName -Location $locName -AllocationMethod Dynamic
$nic=New-AZNetworkInterface -Name $nicName -ResourceGroupName $rgName -Location $locName -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id -PrivateIpAddress "10.0.0.5"
# Create and configure the virtual machine
$cred=Get-Credential -Message "Type the name and password of the local administrator account for exVM."
$vm=Set-AZVMOSDisk -VM $vm -Name ($vmName +"-OS") -DiskSizeInGB 128 -CreateOption FromImage -StorageAccountType "Standard_LRS"
$vm=Set-AZVMOperatingSystem -VM $vm -Windows -ComputerName $vmName -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
$vm=Set-AZVMSourceImage -VM $vm -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
$vm=Add-AZVMNetworkInterface -VM $vm -Id $nic.Id
New-AZVM -ResourceGroupName $rgName -Location $locName -VM $vm
## Conectese a exVM y ejecute
Add-Computer -DomainName "campoh.lab"
Install-WindowsFeature AS-HTTP-Activation, Server-Media-Foundation, NET-Framework-45-Features, RPC-over-HTTP-proxy, RSAT-Clustering, RSAT-Clustering-CmdInterface, RSAT-Clustering-Mgmt, RSAT-Clustering-PowerShell, WAS-Process-Model, Web-Asp-Net45, Web-Basic-Auth, Web-Client-Auth, Web-Digest-Auth, Web-Dir-Browsing, Web-Dyn-Compression, Web-Http-Errors, Web-Http-Logging, Web-Http-Redirect, Web-Http-Tracing, Web-ISAPI-Ext, Web-ISAPI-Filter, Web-Lgcy-Mgmt-Console, Web-Metabase, Web-Mgmt-Console, Web-Mgmt-Service, Web-Net-Ext45, Web-Request-Monitor, Web-Server, Web-Stat-Compression, Web-Static-Content, Web-Windows-Auth, Web-WMI, Windows-Identity-Foundation, RSAT-ADDS
Restart-Computer
## Configurar el Exchange
Write-Host (Get-AZPublicIpaddress -Name "40.84.58.22" -ResourceGroup $rgName).DnsSettings.Fqdn
## Desde exVM
## descarga los pre-requisitos, 
## https://go.microsoft.com/fwlink/p/?linkid=863265 ".NET Framework 4.7.2 "
## https://go.microsoft.com/fwlink/?linkid=327788 "Paquete redistribuible de Visual C++ para Visual Studio 2012"
## https://go.microsoft.com/fwlink/?linkid=2002913 "Paquete redistribuible de Visual C++ para Visual Studio 2013"
## https://go.microsoft.com/fwlink/p/?linkId=258269 "API administrada de comunicaciones unificadas de Microsoft 4.0, Core Runtime (64 bits)" 
## descarga el ISO de Exchange con el ultimo CU
e:
.\setup.exe /prepareSchema /IacceptExchangeServerLicenseTerms
.\setup.exe /prepareAD /OrganizationName:LandonHotel /IacceptExchangeServerLicenseTerms
.\setup.exe /mode:Install /role:Mailbox /OrganizationName:LandonHotel /IacceptExchangeServerLicenseTerms
Restart-Computer
$dnsName="mail1473.eastus2.cloudapp.azure.com"
$user1Name="chris@" + $dnsName
$user2Name="janet@" + $dnsName
$db=Get-MailboxDatabase
$dbName=$db.Name
$password = Read-Host "Enter password" -AsSecureString
New-Mailbox -UserPrincipalName $user1Name -Alias chris -Database $dbName -Name ChrisAshton -OrganizationalUnit Users -Password $password -FirstName Chris -LastName Ashton -DisplayName "Chris Ashton"
New-Mailbox -UserPrincipalName $user2Name -Alias janet -Database $dbName -Name JanetSchorr -OrganizationalUnit Users -Password $password -FirstName Janet -LastName Schorr -DisplayName "Janet Schorr"
## Fase 3- Creamos ahora la maquina de SQL Server
# Log in to Azure
Connect-AzAccount
# Set up key variables
$subscrName="Plataformas de MSDN"
$rgName="Devops2020_rg"
# Set the Azure subscription and location
Select-AzSubscription -SubscriptionName $subscrName
$locName=(Get-AzResourceGroup -Name $rgName).Location
# Create an availability set for SQL Server virtual machines
New-AzAvailabilitySet -ResourceGroupName $rgName -Name sqlAvailabilitySet -Location $locName -Sku Aligned  -PlatformUpdateDomainCount 5 -PlatformFaultDomainCount 2
# Create the SQL Server virtual machine
$vmName="sqlVM"
$vmSize="Standard_D3_V2"
$vnetName="Vnet01"
$vnet=Get-AzVirtualNetwork -Name $vnetName -ResourceGroupName $rgName
$pip=New-AzPublicIpAddress -Name ($vmName + "-PIP") -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic
$nic=New-AzNetworkInterface -Name ($vmName + "-NIC") -ResourceGroupName $rgName -Location $locName -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id -PrivateIpAddress "10.0.0.6"
$avSet=Get-AzAvailabilitySet -Name sqlAvailabilitySet -ResourceGroupName $rgName 
$vm=New-AzVMConfig -VMName $vmName -VMSize $vmSize -AvailabilitySetId $avSet.Id
$vm=Set-AzVMOSDisk -VM $vm -Name ($vmName +"-OS") -DiskSizeInGB 128 -CreateOption FromImage -StorageAccountType "Standard_LRS"
$diskSize=100
$diskConfig=New-AzDiskConfig -AccountType "Standard_LRS" -Location $locName -CreateOption Empty -DiskSizeGB $diskSize
$dataDisk1=New-AzDisk -DiskName ($vmName + "-SQLData") -Disk $diskConfig -ResourceGroupName $rgName
$vm=Add-AzVMDataDisk -VM $vm -Name ($vmName + "-SQLData") -CreateOption Attach -ManagedDiskId $dataDisk1.Id -Lun 1
$cred=Get-Credential -Message "Type the name and password of the local administrator account of the SQL Server computer." 
$vm=Set-AzVMOperatingSystem -VM $vm -Windows -ComputerName $vmName -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
$vm=Set-AzVMSourceImage -VM $vm -PublisherName MicrosoftSQLServer -Offer SQL2017-WS2016 -Skus Standard -Version "latest"
$vm=Add-AzVMNetworkInterface -VM $vm -Id $nic.Id
New-AzVM -ResourceGroupName $rgName -Location $locName -VM $vm
## Connect sqlVM 
Add-Computer -DomainName "campoh.lab"
Restart-Computer
Get-Disk | Where PartitionStyle -eq "RAW" | Initialize-Disk -PartitionStyle GPT -PassThru | New-Partition -AssignDriveLetter -UseMaximumSize | Format-Volume -FileSystem NTFS -NewFileSystemLabel "SQL Data"
md f:\Data
md f:\Log
md f:\Backup
New-NetFirewallRule -DisplayName "SQL Server ports 1433, 1434, and 5022" -Direction Inbound -Protocol TCP -LocalPort 1433,1434,5022 -Action Allow
## Fase 4- Creamos ahora la maquina de SP Server
# Set up key variables
$subscrName="Plataformas de MSDN"
$rgName="Devops2020_rg"
$dnsName="campoh1473"
# Set the Azure subscription
Select-AzSubscription -SubscriptionName $subscrName
# Get the location
$locName=(Get-AzResourceGroup -Name $rgName).Location
# Create an availability set for SharePoint virtual machines
New-AzAvailabilitySet -ResourceGroupName $rgName -Name spAvailabilitySet -Location $locName -Sku Aligned  -PlatformUpdateDomainCount 5 -PlatformFaultDomainCount 2
# Create the spVM virtual machine
$vmName="spVM"
$vmSize="Standard_D3_V2"
$vnetName="Vnet01"
$vm=New-AzVMConfig -VMName $vmName -VMSize $vmSize
$pip=New-AzPublicIpAddress -Name ($vmName + "-PIP") -ResourceGroupName $rgName -DomainNameLabel $dnsName -Location $locName -AllocationMethod Dynamic
$vnet=Get-AzVirtualNetwork -Name $vnetName -ResourceGroupName $rgName
$nic=New-AzNetworkInterface -Name ($vmName + "-NIC") -ResourceGroupName $rgName -Location $locName -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id -PrivateIpAddress "10.0.0.7"
$avSet=Get-AzAvailabilitySet -Name spAvailabilitySet -ResourceGroupName $rgName 
$vm=New-AzVMConfig -VMName $vmName -VMSize $vmSize -AvailabilitySetId $avSet.Id
$vm=Set-AzVMOSDisk -VM $vm -Name ($vmName +"-OS") -DiskSizeInGB 128 -CreateOption FromImage -StorageAccountType "Standard_LRS"
$cred=Get-Credential -Message "Type the name and password of the local administrator account."
$vm=Set-AzVMOperatingSystem -VM $vm -Windows -ComputerName $vmName -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
$vm=Set-AzVMSourceImage -VM $vm -PublisherName "MicrosoftSharePoint" -Offer "MicrosoftSharePointServer" -Skus "2019" -Version "latest"
$vm=Add-AzVMNetworkInterface -VM $vm -Id $nic.Id
New-AzVM -ResourceGroupName $rgName -Location $locName -VM $vm
## Connect spVM 
Add-Computer -DomainName "campoh.lab"
Restart-Computer
New-NetFirewallRule -DisplayName "SQL Server ports 1433, 1434, and 5022" -Direction Inbound -Protocol TCP -LocalPort 1433,1434,5022 -Action Allow
Write-Host (Get-AzPublicIpaddress -Name "13.77.127.118" -ResourceGroup $rgName).DnsSettings.Fqdn
##Desplegar un cliente
$rgName="Devops2020_rg"
$locName=(Get-AzResourceGroup -Name $rgName).Location
$vnet=Get-AzVirtualNetwork -Name $vnetName -ResourceGroupName $rgName
$pip=New-AzPublicIpAddress -Name CLIENT1-PIP -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic
$nic=New-AzNetworkInterface -Name CLIENT1-NIC -ResourceGroupName $rgName -Location $locName -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id
$vm=New-AzVMConfig -VMName CLIENT1 -VMSize Standard_A1
$cred=Get-Credential -Message "Type the name and password of the local administrator account for CLIENT1."
$vm=Set-AzVMOperatingSystem -VM $vm -Windows -ComputerName CLIENT1 -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
$vm=Set-AzVMSourceImage -VM $vm -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2016-Datacenter -Version "latest"
$vm=Add-AzVMNetworkInterface -VM $vm -Id $nic.Id
$vm=Set-AzVMOSDisk -VM $vm -Name "CLIENT1-OS" -DiskSizeInGB 128 -CreateOption FromImage -StorageAccountType "Standard_LRS"
New-AzVM -ResourceGroupName $rgName -Location $locName -VM $vm
## Detener / Iniciar las maquinas virtuales
$rgName="Devops2020_rg"
Stop-AZVM -Name Client1 -ResourceGroupName $rgName -Force
Stop-AZVM -Name exVM -ResourceGroupName $rgName -Force
Stop-AzVM -Name spVM -ResourceGroupName $rgName -Force
Stop-AzVM -Name sqlVM -ResourceGroupName $rgName -Force
Stop-AZVM -Name adVM -ResourceGroupName $rgName -Force
$rgName="Devops2020_rg"
Start-AZVM -Name adVM -ResourceGroupName $rgName
Start-AZVM -Name exVM -ResourceGroupName $rgName
Start-AzVM -Name sqlVM -ResourceGroupName $rgName
Start-AzVM -Name spVM -ResourceGroupName $rgName
Start-AZVM -Name Client1 -ResourceGroupName $rgName
```
## Plantillas ARM

```Powershell
$location = 'eastus'
$rgName = 'az-rg1'
New-AzResourceGroup -Name $rgName -Location $location
New-AzResourceGroupDeployment `
   -ResourceGroupName $rgName `
   -TemplateFile $HOME/az-vms-template.json `
   -TemplateParameterFile $HOME/az-vm-parameters.json `
   -AsJob

$rgName = 'az-rg2'
New-AzResourceGroup -Name $rgName -Location $location

New-AzResourceGroupDeployment `
   -ResourceGroupName $rgName `
   -TemplateFile $HOME/az-vm-template.json `
   -TemplateParameterFile $HOME/az-vm-parameters.json `
   -nameSuffix 2 `
   -AsJob

$rgName = 'az-rg3'
New-AzResourceGroup -Name $rgName -Location $location

New-AzResourceGroupDeployment `
   -ResourceGroupName $rgName `
   -TemplateFile $HOME/az-vm-template.json `
   -TemplateParameterFile $HOME/az-vm-parameters.json `
   -nameSuffix 3 `
   -AsJob

$virtualNetwork1 =az-vnet01
$virtualNetwork2 =az-vnet02
$virtualNetwork3 =az-vnet03

Add-AzVirtualNetworkPeering `
  -Name myVirtualNetwork1-myVirtualNetwork2 `
  -VirtualNetwork $virtualNetwork1 `
  -RemoteVirtualNetworkId $virtualNetwork2.Id

Add-AzVirtualNetworkPeering `
  -Name myVirtualNetwork2-myVirtualNetwork1 `
  -VirtualNetwork $virtualNetwork2 `
  -RemoteVirtualNetworkId $virtualNetwork1.Id

Get-AzVirtualNetworkPeering `
  -ResourceGroupName myResourceGroup `
  -VirtualNetworkName myVirtualNetwork1 `
  | Select PeeringState

Add-AzVirtualNetworkPeering `
  -Name myVirtualNetwork2-myVirtualNetwork3 `
  -VirtualNetwork $virtualNetwork2 `
  -RemoteVirtualNetworkId $virtualNetwork3.Id

Add-AzVirtualNetworkPeering `
  -Name myVirtualNetwork3-myVirtualNetwork2 `
  -VirtualNetwork $virtualNetwork3 `
  -RemoteVirtualNetworkId $virtualNetwork2.Id

Get-AzVirtualNetworkPeering `
  -ResourceGroupName myResourceGroup `
  -VirtualNetworkName myVirtualNetwork2 `
  | Select PeeringState

$PIP=Get-AzPublicIpAddress `
  -Name az-vm0 `
  -ResourceGroupName myResourceGroup | Select IpAddress

mstsc /v:$PIP

New-NetFirewallRule –DisplayName "Allow ICMPv4-In" –Protocol ICMPv4

mstsc /v:10.1.0.4
ping 10.0.0.4

Install-WindowsFeature RemoteAccess -IncludeManagementTools
Install-WindowsFeature -Name Routing -IncludeManagementTools -IncludeAllSubFeature
Install-WindowsFeature -Name "RSAT-RemoteAccess-Powershell"
Install-RemoteAccess -VpnType RoutingOnly
Get-NetAdapter | Set-NetIPInterface -Forwarding Enabled


```
## Contribuciones
Las solicitudes de extracción son bienvenidas. Para cambios importantes, abra primero un problema para discutir qué le gustaría cambiar.

Asegúrese de actualizar las pruebas según corresponda.

Fabian Alberto Campo
Microsoft Most Valuable Professional 2018-2020
Microsoft Certified Trainer /LinkedIn Learning Author
Website: [www.campohenriquez.com] 
LinkedIn: [https://www.linkedin.com/in/fcampo/]
Twitter: [https://twitter.com/fcampo]
Youtube: [https://www.youtube.com/channel/UC5_-RLpII-4R_xyF6R4CZ2Q?view_as=subscriber]

## License
[MIT](https://choosealicense.com/licenses/mit/)