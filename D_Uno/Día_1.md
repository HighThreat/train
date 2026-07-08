# Primero hay que instalar el entorno de azure cli(empezamos con azure), para ello vas a necesitar los siguientes comandos:
    Te dejo el link de los pasos por si ha sucedido algún cambio, de cara al futuro:
```markdown[https://learn.microsoft.com/es-es/cli/azure/install-azure-cli-windows?view=azure-cli-latest&pivots=winget]inicio
```
	-Aclaración, los comandos estarán siempre entre corchetes (comando).
	
	Tras instalar el az CLI en tu equipo, debes cerrar tu terminal y volver a abrir una nueva,tras ello realiza el comando: (az)
	Tras ello deberías ver la consola de azure.
	Lo primero que vamos a mirar es el (az --version) para saber que versión tenemos instalada.
	Tras esto, ya hemos terminado el primer paso, vamos con el segundo.
	Segundo paso, realizamos az login con nuestra cuenta con suscripción, de la que dispongaís.
	Si se te complica, solo tienes que mirar la última parte de tu comando y mirar que tenants tienes y saber cuál es el que tiene la suscripción, tras ello realizas simplemente.
		-(az login --tenant (aqui va el id de tu tenant con suscripción, será una serie de números tipo 00f9-19f1-d1be-n1u2 o similar))

	A continuación nos vamos a pegar los comandos necesarios para dejar seleccionada la suscripción en la que queremos trabajar, en el siguiente orden.
	-(az account list --output table) con esto veremos las suscripciónes disponibles que tenemos para elegir.
	-(az account set --subscription <00000000-0000-0000-0000-000000000000>) De la lista anterior te copias el subscription_id y lo pegas ahí, con eso ya tenemos el az cli configurado 
	de forma básica.

# Creación de un grupo de recursos.
Vamos a mirar que tenemos en el siguiente orden, vamos a mirar si el  x nombre que queremos asignar a nuestro recursos ya está siendo usado, para evitar repetirlo.
-(az group exists --name <myUniqueRGname>), si dice false es que nadie lo está usando y por lo tanto lo podemos utilizar nosotros.
ejemplo: >az group exists --name leogroup
		 >false
Ahora con nuestro nombre elegido y único vamos a, pos no, antes de seguir debemos revisar a qué regiones está limitada nuestra suscripción, puede ser que no, pero si tienes la de estudiante
es bastante probable que si que tengas bloqueos en ciertas regiones, vamos a comprobar cuales con el siguiente comando:
-(az account list-locations --query "[].{Region:name}" --output table), este comando nos devolverá las regiones en las cuales podremos desplegar nuestro grupo de recursos y otros servicios.

El siguiente subpaso es crear el grupo de recursos, pero como queremos tener estilo vamos a realizarlo mediante implementación de variables os dejo el comando y un ejemplo:
	-(az group create --location <myLocation> --name <myUniqueRGname>) Comando vegano pero sencillo por si no quieres usar variables.
	-(location="eastus"
	resourceGroup="msdocs-tutorial-rg-$randomIdentifier"
	az group create --name $resourceGroup --location $location --output json)  Elegante, como puedes ver, ahora ya no tendrás que repetir y repetir el location por ejemplo, simplemente puedes utilizar la variable.
Ahora bien, probablemente si usas el cmd, tendrás que realizar ( set location="spaincentral") y después llamar a la variable con %location%, ejemplo:
	-(set location="eastus"
	  set resourceGroup="msdocs-tutorial-rg-$randomIdentifier"
	  az group create --name %resourceGroup% --location %location% --output json)

	  

