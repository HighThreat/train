# Instalación y Configuración de Azure CLI

## Paso 1: Instalación del Azure CLI

Primero hay que instalar el entorno de Azure CLI. Para ello vas a necesitar los siguientes comandos.

Te dejo el link de los pasos por si ha sucedido algún cambio, de cara al futuro:
- https://learn.microsoft.com/es-es/cli/azure/install-azure-cli-windows?view=azure-cli-latest&pivots=winget

**Aclaración:** Los comandos estarán siempre entre paréntesis `(comando)`.

### Verificación de la instalación

Tras instalar el az CLI en tu equipo, debes cerrar tu terminal y volver a abrir una nueva. Tras ello realiza el comando:

```bash
az
```

Tras ello deberías ver la ayuda de Azure CLI.

Lo primero que vamos a mirar es:

```bash
az --version
```

Para saber qué versión tenemos instalada.

**Tras esto, ya hemos terminado el primer paso.**

---

## Paso 2: Autenticación y Configuración de Suscripción

### Login con tu cuenta

Segundo paso: realizamos `az login` con nuestra cuenta con suscripción, de la que dispongaís.

```bash
az login
```

Si se te complica, solo tienes que mirar la última parte de tu comando y mirar qué tenants tienes y saber cuál es el que tiene la suscripción. Tras ello realizas simplemente:

```bash
az login --tenant <ID_TENANT>
```

*Ejemplo: aquí va el id de tu tenant con suscripción, será una serie de números tipo 00f9-19f1-d1be-n1u2 o similar,  el tenant se usa cuando tienes varios directorios o varias organizaciones*

### Seleccionar la suscripción activa

A continuación nos vamos a pegar los comandos necesarios para dejar seleccionada la suscripción en la que queremos trabajar, en el siguiente orden:

1. Lista las suscripciones disponibles:
   ```bash
   az account list --output table
   ```
   Con esto veremos las suscripciones disponibles que tenemos para elegir.

2. Establece tu suscripción:
   ```bash
   az account set --subscription <00000000-0000-0000-0000-000000000000>
   ```
   De la lista anterior te copias el subscription_id y lo pegas ahí. Con eso ya tenemos el az CLI configurado de forma básica.

---

## Paso 3: Creación de un Grupo de Recursos

### Verificar disponibilidad del nombre

El siguiente comando comprueba existencia dentro de la suscripción actual, del nombre de un grupo de recurso, esto lo realizamos para evitar errores de nombres o name_spaces:

```bash
az group exists --name <myUniqueRGname>
```

Si dice false es que ese nombre no está asignado a ningún resource group en nuestra suscripción y por lo tanto lo podemos utilizar.

**Ejemplo:**
```bash
az group exists --name leogroup
> false
```

### Verificar regiones disponibles

Ahora con nuestro nombre elegido y único vamos a revisar a qué regiones está limitada nuestra suscripción. Puede ser que no, pero si tienes la de estudiante es bastante probable que sí que tengas bloqueos en ciertas regiones.

Vamos a comprobar cuáles con el siguiente comando:

```bash
az account list-locations --query "[].{Region:name}" --output table
```

Este comando nos devolverá las regiones en las cuales podremos desplegar nuestro grupo de recursos y otros servicios.

### Crear el grupo de recursos

El siguiente subpaso es crear el grupo de recursos. Como queremos tener estilo vamos a realizarlo mediante implementación de variables. Os dejo el comando y un ejemplo:

**Opción simple (sin variables):**
```bash
az group create --location <myLocation> --name <myUniqueRGname>
```

Comando vegano pero sencillo por si no quieres usar variables.

**Opción elegante (con variables - Linux/macOS/PowerShell):**
```bash
location="eastus"
resourceGroup="msdocs-tutorial-rg-$randomIdentifier"
az group create --name $resourceGroup --location $location --output json
```

Elegante, como puedes ver, ahora ya no tendrás que repetir y repetir el location por ejemplo, simplemente puedes utilizar la variable.

**Opción para CMD (Windows):**

Ahora bien, probablemente si usas el cmd, tendrás que realizar:

```cmd
set location="eastus"
set resourceGroup="msdocs-tutorial-rg-1"
az group create --name %resourceGroup% --location %location% --output json
```

Y después llamar a la variable con %location%, ejemplo:

```cmd
set location="spaincentral"
set resourceGroup="msdocs-tutorial-rg-11111"
az group create --name %resourceGroup% --location %location% --output json
```
### Vamos a crear un storage!

Recordando los pasos anteriores, ya tenemos una variable que guarda nuestra zona de despliegue, en este caso spaincentral, y ahora vamos a guardar en variable nuestro grupo de recursos y lo confirmaremos con el comando echo y la variable para ver si se ha guardado.

```cmd
set  Rg_p1="leoGroup"
echo %Rg_p1%
"leoGroup"
```

### Paso 2 de crear el storage

Vamos a crear una varible para nuestro storage para almacenar el nombre dentro, y tras ello vamos a crear el storage.

```cmd
set storagenameP1="leorage"
echo %storagenameP1%
>"leorage"
set location="eastus"
set resourceGroup="<msdocs-tutorial-rg-00000000>"
set storageAccount="msdocssa%randomIdentifier%"

:: Create a storage account.
echo "Creating storage account %storageAccount% in resource group %resourceGroup%"
az storage account create --name %storageAccount% ^
                          --resource-group %resourceGroup% ^
                          --location %location% ^
                          --sku Standard_RAGRS ^
                          --kind StorageV2 ^
                          --output json
```

Te pongo otro ejemplo, por cierto, lo hago con cmd porque primero, quería probar las distintas sintaxis entre las distintas shells y segundo utilizar bash es más sencillo en general, así sería en bash.

Otro apunte, la línea let """ la utilizamos para randomizar el nombre o identificador en este caso para evitar duplicados.

```cmd
let "randomIdentifier=$RANDOM*$RANDOM"
location="eastus"
resourceGroup="<msdocs-tutorial-rg-00000000>"
storageAccount="msdocssa$randomIdentifier"

# Create a storage account.
echo "Creating storage account $storageAccount in resource group $resourceGroup"
az storage account create --name $storageAccount \
                          --resource-group $resourceGroup \
                          --location $location \
                          --sku Standard_RAGRS \
                          --kind StorageV2 \
                          --output json
```

El resultado que nos tiene que mostrar el cli si lo hemos realizado correctamente debería ser el siguiente:

```cmd
  "accessTier": "Hot",
  "accountMigrationInProgress": null,
  "allowBlobPublicAccess": false,
  "allowCrossTenantReplication": false,
  "allowSharedKeyAccess": null,
  "allowSharedKeyAccessForServices": null,
  "allowedCopyScope": null,
  "azureFilesIdentityBasedAuthentication": null,
  "blobRestoreStatus": null,
  "creationTime": "2026-07-09T15:31:21.986827+00:00",
  "customDomain": null,
  "dataCollaborationPolicyProperties": null,
  "defaultToOAuthAuthentication": null,
  "dnsEndpointType": null,
  "dualStackEndpointPreference": null,
  "enableExtendedGroups": null,
  "enableHttpsTrafficOnly": true,
  "enableNfsV3": null,
  "encryption": {
    "encryptionIdentity": null,
    "keySource": "Microsoft.Storage",
    "keyVaultProperties": null,
    "requireInfrastructureEncryption": null,
    "services": {
      "blob": {
        "enabled": true,
        "keyType": "Account",
        "lastEnabledTime": "2026-07-09T15:31:22.094333+00:00"
      },
      "file": {
        "enabled": true,
        "keyType": "Account",
        "lastEnabledTime": "2026-07-09T15:31:22.094333+00:00"
      },
      "queue": null,
      "table": null
    }
  },
  "extendedLocation": null,
  "failoverInProgress": null,
  "geoPriorityReplicationStatus": null,
  "geoReplicationStats": null,
  "id": "/subscriptions/9fa4c38e-dcb2-4ca0-b292-89006f020e7a/resourceGroups/leoGroup/providers/Microsoft.Storage/storageAccounts/leorage",
  "identity": null,
  "immutableStorageWithVersioning": null,
  "isHnsEnabled": null,
  "isLocalUserEnabled": null,
  "isSftpEnabled": null,
  "isSkuConversionBlocked": null,
  "keyCreationTime": {
    "key1": "2026-07-09T15:31:22.088205+00:00",
    "key2": "2026-07-09T15:31:22.088205+00:00"
  },
  "keyPolicy": null,
  "kind": "StorageV2",
  "largeFileSharesState": null,
  "lastGeoFailoverTime": null,
  "location": "francecentral",
  "minimumTlsVersion": "TLS1_0",
  "name": "leorage",
  "networkRuleSet": {
    "bypass": "None",
    "defaultAction": "Allow",
    "ipRules": [],
    "ipv6Rules": [],
    "resourceAccessRules": null,
    "virtualNetworkRules": []
  },
  "placement": null,
  "primaryEndpoints": {
    "blob": "https://leorage.blob.core.windows.net/",
    "dfs": "https://leorage.dfs.core.windows.net/",
    "file": "https://leorage.file.core.windows.net/",
    "internetEndpoints": null,
    "ipv6Endpoints": null,
    "microsoftEndpoints": null,
    "queue": "https://leorage.queue.core.windows.net/",
    "table": "https://leorage.table.core.windows.net/",
    "web": "https://leorage.z28.web.core.windows.net/"
  },
  "primaryLocation": "francecentral",
  "privateEndpointConnections": [],
  "provisioningState": "Succeeded",
  "publicNetworkAccess": null,
  "resourceGroup": "leoGroup",
  "routingPreference": null,
  "sasPolicy": null,
  "secondaryEndpoints": {
    "blob": "https://leorage-secondary.blob.core.windows.net/",
    "dfs": "https://leorage-secondary.dfs.core.windows.net/",
    "file": null,
    "internetEndpoints": null,
    "ipv6Endpoints": null,
    "microsoftEndpoints": null,
    "queue": "https://leorage-secondary.queue.core.windows.net/",
    "table": "https://leorage-secondary.table.core.windows.net/",
    "web": "https://leorage-secondary.z28.web.core.windows.net/"
  },
  "secondaryLocation": "francesouth",
  "sku": {
    "name": "Standard_RAGRS",
    "tier": "Standard"
  },
  "statusOfPrimary": "available",
  "statusOfSecondary": "available",
  "storageAccountSkuConversionStatus": null,
  "systemData": null,
  "tags": {},
  "type": "Microsoft.Storage/storageAccounts",
  "zones": null
}
```
### Paso 3, etiquetar los recursos.

Etiquetamos los recursos para poder facilitar las gestiones de cara al futuro, por ejemplo a la hora de crear un budge, poder agruparlo bajo 

```cmd
az storage account update --name $storageAccount \
                          --resource-group $resourceGroup \
                          --tags Team=t1 Environment=e1

```

Salida esperada:

```cmd
 },
  "statusOfPrimary": "available",
  "statusOfSecondary": "available",
  "storageAccountSkuConversionStatus": null,
  "systemData": null,
  "tags": {
    "Environment": "e1",
    "Team": "t1"
  },
  "type": "Microsoft.Storage/storageAccounts",
  "zones": null
}
```

Si no desea sobrescribir etiquetas anteriores mientras trabaja en este paso del tutorial, use el comando az tag update y establezca el parámetro --operation en merge.

Con el siguiente comando podemos ver los storage accounts creados en un determinado resource group:


```cmd
az storage account list --resource-group $resourceGroup
```
