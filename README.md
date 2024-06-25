# fabric-ops

fabric-ops es una librería de Python que automatiza procesos y trabaja con recursos de Microsoft Fabric.

## Instalación

Para instalar la librería, ejecuta el siguiente comando:

```bash
pip install fabric-ops
```

## Configuración

### Configura tu aplicación de Microsoft Entra ID

Para utilizar esta librería, es necesario crear una aplicación de Entra ID. y posteriormente, agregar los permisos de API con consentimiento de administrador para :

- Microsoft Graph
  - Mail.Send (Aplicación): Send mail as any user - Permisos de administrador
- Power BI Service
  - Tenant.Read.All (Delegada): View all content in tenant - Sin permisos de administrador

Después de configurar la aplicación asignale un rol de colaborador a la suscripciñon de Azure que contiene la capacidad de Microsoft Fabric que quieres administrar el rol Colaborador es opcional en caso de que tengas roles personalizados. Pero deben tener permisos similares a la colaboración para administrar recursos.

### Crea la configuración para las credenciales

Utilizando el id de la aplicación de microsoft Entra ID y el secreto, Crea un secreto con la siguiente estructura:

```json
{"tenant_id": "","client_id": "","client_secret": ""}
```

### Configuración en Windows

Si vas a utilizar la librería en Windows, crea una variable de entorno llamada `MSFABRIC_CONF` con el valor del secreto creado anteriormente.

1. Abre el Panel de Control y ve a Sistema y Seguridad.
2. Haz clic en Sistema y luego en Configuración avanzada del sistema.
3. En la pestaña Avanzado, haz clic en Variables de entorno.
4. En la sección Variables del sistema, haz clic en Nuevo.
5. Ingresa `MSFABRIC_CONF` como el nombre de la variable y el valor del secreto como el valor de la variable.
6. Haz clic en Aceptar y luego en Aceptar nuevamente para guardar los cambios.

Además, si quieres notificar (de forma opcional) cuando la capacidad se haya pausado o reanudado , es necesario crear otra variable de entorno llamada `MSFABRIC_EMAIL_SENDER`, que debe contener el correo electrónico que enviará las notificaciones, el services principal se encargará de enviar correo con los permisos establecidos anteriormente a nombre de la cuenta de elección. Se recomienda utilizar un correo específico para notificaciones y no uno personal.

# Configura el portal de Power BI

- Habilita Service principals can access read-only admin APIs desde el portal de administración de Fabric a un grupo de seguridad (recomendado) o a toda la organización.

- Habilita Service principals can use Fabric APIs a un grupo de seguridad (recomendado) o a toda la organización.


## Ejemplo para automatizar la capacidad de Microsoft Fabric

Aquí hay un ejemplo básico de cómo utilizar la librería:

```python
from fabric_ops import Fabric

# Uso del Fabric con notificaciones opcionales
fabric = Fabric(send_notification=True, email_list=["LISTA DE CORREOS"], language="es")

# Reanudar capacidad
fabric.capacity(name="nombre capacidad").resume()

# O tal vez suspender capacidad
fabric.capacity(name="nombre capacidad").suspend()
```

- `send_notification` es opcional, si se establece en `True`, `email_list` debe ser proporcionada.
- `language` es opcional, y se utiliza para recibir el correo en inglés o español.



--------------------------------------------------------------------------------------------------------------------

English



# fabric-ops

fabric-ops is a Python library that automates processes and works with Microsoft Fabric resources.

## Installation

To install the library, run the following command:

pip install fabric-ops

## Configuration

### Configure your Microsoft Entra ID application

To use this library, you need to grant permissions to create an Entra ID application. Add the following API permissions with admin consent:

- Microsoft Graph
  - Mail.Send (Application): Send mail as any user - Admin permission
- Power BI Service
  - Tenant.Read.All (Delegated): View all content in tenant

After configuring the application, assign a contributor role to the Azure subscription containing the Microsoft Fabric capacity you want to manage. The Contributor role is optional if you have custom roles but they must have permissions similar to the contributor role to manage resources.

### Create the configuration for the credentials

Using the Microsoft Entra ID application ID and secret, create a secret with the following structure:
```json
{"tenant_id": "","client_id": "","client_secret": ""}
```
### Configuration on Windows

If you are going to use the library on Windows, create an environment variable called `MSFABRIC_CONF` with the value of the previously created secret.

1. Open the Control Panel and go to System and Security.
2. Click on System and then on Advanced system settings.
3. In the Advanced tab, click on Environment Variables.
4. In the System variables section, click New.
5. Enter `MSFABRIC_CONF` as the variable name and the secret value as the variable value.
6. Click OK and then OK again to save the changes.

Additionally, if you want to optionally notify when the capacity has been paused or resumed, you need to create another environment variable called `MSFABRIC_EMAIL_SENDER`, which should contain the email address that will send the notifications. The service principal will send emails with the permissions set earlier on behalf of the chosen account. It is recommended to use a specific email for notifications and not a personal one.

# Configure the Power BI portal

- Enable "Service principals can access read-only admin APIs" from the Fabric admin portal to a security group (recommended) or the entire organization.

- Enable "Service principals can use Fabric APIs" to a security group (recommended) or the entire organization.

## Example to automate Microsoft Fabric capacity

Here is a basic example of how to use the library:

```python
from fabric_ops import Fabric

# Use Fabric with optional notifications
fabric = Fabric(send_notification=True, email_list=["EMAIL_LIST"], language="es")

# Resume capacity
fabric.capacity(name="capacity_name").resume()

# Or suspend capacity
fabric.capacity(name="capacity_name").suspend()

```

