### Primero hay que instalar el entorno de azure cli(empezamos con azure), para ello vas a necesitar los siguientes comandos:
    Te dejo el link de los pasos por si ha sucedido algún cambio, de cara al futuro:
	-https://learn.microsoft.com/es-es/cli/azure/install-azure-cli-windows?view=azure-cli-latest&pivots=winget
	-Aclaración, los comandos estarán siempre entre corchetes (comando).
	
	Tras instalar el az CLI en tu equipo, debes cerrar tu terminal y volver a abrir una nueva,tras ello realiza el comando: (az)
	Tras ello deberías ver la consola de azure.
	Lo primero que vamos a mirar es el (az --version) para saber que versión tenemos instalada.
	Tras esto, ya hemos terminado el primer paso, vamos con el segundo.
	Segundo paso, realizamos az login con nuestra cuenta con suscripción, de la que dispongaís.
	Si se te complica, solo tienes que mirar la última parte de tu comando y mirar que tenants tienes y saber cuál es el que tiene la suscripción, tras ello realizas simplemente.
		-(az login --tenant (aqui va el id de tu tenant con suscripción, será una serie de números tipo 00f9-19f1-d1be-n1u2 o similar))
