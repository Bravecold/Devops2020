# DevOpsDays 2020 (Live from Bogota '30_Jul_2020')

# Visual Studio Code y Azure

En este repositorio exploraremos Visual studio Code, desde una perspectiva dinamica y divertida, es como todas las historias de aventuras de D&D, o del señor de los anillos, estamos todos reunidos en la mayor comodidad, tranquilos disfrutando de la tarde, y el destino nos va a llegar nuevo un "Quest", [una busqueda o reto](https://code.visualstudio.com/docs), en este caso vamos a acompañar a este valiente explorador, de abultado vientre, y corta vista, en un terreno poco explorado, como lo es el uso de las diferentes plataformas de administracion de la nube como las lineas de comando y particularmente el vscode para gestionar plantillas ARM.

## Instalacion de prerequisitos

Si usted tiene un equipo con Windows, lo invito a que prepare los pre-requisitos:

- Una enorme taza del café de su preferencia 

- El modulo de Azure para powershell, el cual se puede instalar iniciando el PowerShell en modo administrador, y usando los siguientes comandos:

```PowerShell
$PSVersionTable.PSVersion #para verificar que version de Powershell se tiene con version 5.0 o superior podemos hacer uso de
Install-Module -Name Az #Esto tomara un rato, porque son varios modulos, tambien es posible que te solicite confirmacion de que confias en el repositorio de PSGallery antes de continuar.
```
Para mas informacion consulte [aqui](https://docs.microsoft.com/en-us/powershell/azure/install-az-ps?view=azps-4.4.0)

- El Azure CLI tambien vale la pena tenerlo, vamos a instalarlo desde [aqui](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli-windows?view=azure-cli-latest&tabs=azure-cli) esta es otra manera de interactuar con Azure desde PowerShell como administrador ejecutaremos *Invoke-WebRequest -Uri https://aka.ms/installazurecliwindows -OutFile .\AzureCLI.msi; Start-Process msiexec.exe -Wait -ArgumentList '/I AzureCLI.msi /quiet'; rm .\AzureCLI.msi* 

- Es posible ingresar a la tienda de Microsoft Store, y descargar el [Windows Terminal](https://www.microsoft.com/en-us/p/windows-terminal/9n0dx20hk701?activetab=pivot:overviewtab) 

- Descargue el Visual Studio Code [desde aqui](https://code.visualstudio.com/download) el instalador es muy liviano, pesa menos de 100MB. la instalacion es super simple, y vamos a ir complementandola con las extensiones para Azure a lo largo de este workshop


## Guia de laboratorio




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