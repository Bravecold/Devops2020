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


//ssh bravecold@devfachops794326318000.eastus.cloudapp.azure.com:60999





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