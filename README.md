# Lab 01 - Gestión de identidades de Microsoft Entra ID

## 📑 Índice

1. [Descripción](#-descripción)  
2. [Tareas principales](#tareas-principales)  
3. [Diagrama de flujo del Lab](#-diagrama-de-flujo-del-lab-01)  
4. [Resultados Esperados](#-resultados-esperados)  
5. [Mejoras aplicadas](#-mejoras-aplicadas)
6. [Desarrollo del Lab](#-desarrollo-del-lab)  
   - [Tarea 1: Crear y configurar cuentas de usuario (manual)](#-tarea-1-crear-y-configurar-cuentas-de-usuario-de-manera-manual)  
   - [Crear un nuevo usuario (manual)](#-crear-un-nuevo-usuario-manual)  
   - [Invitar a un usuario externo (manual)](#-invitar-a-un-usuario-externo-manual)  
   - [Tarea 2: Crear grupos y agregar miembros (manual)](#-tarea-2-crear-grupos-y-agregar-miembros-manual)  
7. [Automatización de las tareas](#automatización-de-las-tareas)  
   - [Automatización con Microsoft Graph en PowerShell](#-automatización-con-microsoft-graph-en-powershell)
   - [Errores encontrados con Microsoft Graph PowerShell](#errores-encontrados-con-microsoft-graph-powershell)  
   - [Automatización con Azure CLI o Azure Cloud Shell](#automatización-con-azure-cli-o-azure-cloud-shell)
   - [Otras formas de automatización](#-otras-formas-de-automatización)
8. [Contribuciones](#-contribuciones)  
9. [Licencia](#-licencia)  

---

## 📘 Descripción

Este laboratorio corresponde al primero de una serie de prácticas diseñadas para Administradores de Azure y forma parte del itinerario del examen AZ-104. Los laboratorios oficiales se encuentran disponibles en [Microsoft Learning Labs AZ-104](https://microsoftlearning.github.io/AZ-104-MicrosoftAzureAdministrator/).

Si bien este ejercicio toma como referencia los laboratorios originales, he incorporado mejoras orientadas a la automatización de tareas mediante PowerShell (Microsoft Graph) y Azure CLI. De esta manera, además de introducir los conceptos fundamentales de usuarios y grupos —los bloques esenciales de cualquier solución de identidad en la nube—, se establece una base sólida para comprender cómo se gestionan las identidades en Microsoft Entra ID.

El enfoque automatizado permite reproducir y escalar la gestión de identidades de forma más eficiente y profesional, preparando el camino para escenarios más avanzados de administración y seguridad en la nube.

>💡 **Nota profesional:**  
Este y los demás laboratorios que publicaré ya los había realizado previamente; sin embargo, tras completar el curso oficial del [**AZ-104 en Microsoft Learn**](https://learn.microsoft.com/es-es/training/courses/az-104t00), adquirí una perspectiva más amplia y una visión más profesional sobre los servicios que ofrece Azure. Esto me permitió comprender mejor el potencial de la plataforma y enriquecer los ejercicios con prácticas de automatización y escenarios más cercanos a la realidad empresarial.

---

## Tareas Principales

Crear y configurar identidades en **Microsoft Entra ID**, tanto de manera manual en el portal de Azure como de forma automatizada mediante **PowerShell (Microsoft Graph)** y **Azure CLI**:

- **Cuentas de usuario**: almacenar datos como nombre, departamento, ubicación y contacto, ya sea creando usuarios individuales o en lote mediante scripts.  
- **Invitar usuarios externos (guest)**: habilitar la colaboración con cuentas externas, automatizando el envío de invitaciones y la configuración de propiedades básicas.  
- **Crear grupos y agregar miembros**: establecer grupos de seguridad y asignar usuarios, con opciones de membresía estática, dinámica o mediante scripts que simplifican la administración masiva.  

---

### 📐 Diagrama de flujo del Lab 01

El siguiente esquema resume las dos tareas principales del laboratorio:

- **Tarea 1**: Creación de usuarios internos y externos (guest), ambos con el rol de *Administrador TI Lab*.

- **Tarea 2**: Agrupación de esos usuarios dentro del grupo de seguridad *Administrador TI Lab*.

<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/7b4e977d-d798-4bab-a7c6-3fb44b70a769" />



---

## 📊 Resultados Esperados

- Comprender cómo se gestionan las identidades en **Microsoft Entra ID** tanto de forma manual como automatizada.  
- Diferenciar entre cuentas internas y externas, incluyendo la invitación de usuarios invitados (guest).  
- Configurar grupos de seguridad y observar cómo la membresía puede ser administrada manualmente, mediante reglas dinámicas o con scripts automatizados.  
- Reconocer que un **tenant** representa la instancia de Microsoft Entra ID de tu organización, donde se gestionan usuarios y grupos.  
- Aplicar **automatización con PowerShell (Microsoft Graph)** y **Azure CLI** para crear usuarios y grupos de manera reproducible, reduciendo la carga administrativa.  
- Validar que los scripts permiten escalar la gestión de identidades, integrando escenarios de creación masiva (CSV) y asignación de miembros a grupos.  

---

## 🔧 Mejoras aplicadas

Este laboratorio toma como referencia los ejercicios oficiales del **AZ-104**, pero incorpora mejoras orientadas a la **automatización de tareas** en **Microsoft Entra ID**.  

Las principales optimizaciones son:  

- **Automatización con PowerShell (Microsoft Graph)**:  
  Uso del módulo moderno `Microsoft.Graph` para crear usuarios, invitar externos y gestionar grupos con comandos reproducibles (`New-MgUser`, `New-MgGroup`, `Add-MgGroupMember`). Esto reemplaza los antiguos módulos `AzureAD` y asegura compatibilidad futura.  

- **Automatización con Azure CLI**:  
  Scripts que permiten crear usuarios, invitar invitados y administrar grupos desde la línea de comandos (`az ad user create`, `az ad user invite`, `az ad group create`, `az ad group member add`).  

- **Escalabilidad y reproducibilidad**:  
  Los procesos manuales del portal fueron complementados con scripts que permiten ejecutar las mismas tareas de forma masiva (por ejemplo, leyendo datos desde archivos CSV).  

- **Enfoque profesional**:  
  La combinación de pasos manuales y automatizados refleja escenarios reales de administración en Azure, donde la eficiencia y la seguridad son prioritarias.  

---

## 🚀 Desarrollo del lab

### 📝 Tarea 1: Crear y configurar cuentas de usuario de manera manual

En esta tarea se crearán y configurarán cuentas de usuario. Las cuentas de usuario almacenan datos como nombre, departamento, ubicación y la información de contacto.

1. **Iniciamos sesión en el portal de Azure**: [https://portal.azure.com](https://portal.azure.com).  
2. **Buscamos y seleccionamos en el panel izquierdo: Microsoft Entra ID**.  
<img width="281" height="887" alt="1" src="https://github.com/user-attachments/assets/0732e0c0-c115-4af2-a0c6-2f30197d4162" />

---

### 🔹 Crear un nuevo usuario (manual)

1. En el panel **Administrar (Manage)**, seleccionamos **Usuarios**.
<img width="338" height="677" alt="2" src="https://github.com/user-attachments/assets/2b7f0cde-b158-435d-8b51-48c77bb0118f" />

3. En el menú desplegable **Nuevo usuario**, seleccionamos **Crear nuevo usuario**.
<img width="1093" height="758" alt="4" src="https://github.com/user-attachments/assets/10ae83ea-7a56-4f9a-982c-36e98084905f" />

5. Configuramos el usuario con los siguientes valores:

| Configuración            | Valor                |
|--------------------------|----------------------|
| Nombre principal (UPN)   | Bruce Wayne          |
| Nombre para mostrar      | Bruce Wayne          |
| Generar contraseña auto  | Activado             |
| Cuenta habilitada        | Activado             |
| Puesto                   | Administrador IT Lab |
| Departamento             | TI                   |
| Ubicación de uso         | United States        |

<img width="1021" height="838" alt="5" src="https://github.com/user-attachments/assets/51016f36-5634-4efe-bae9-4e2ce145083a" />

<img width="1057" height="743" alt="6" src="https://github.com/user-attachments/assets/5255cefd-8053-46b2-be00-9f4bc8da8900" />

1. Seleccionamos **Revisar + crear** y luego **Crear**.
<img width="1057" height="743" alt="6" src="https://github.com/user-attachments/assets/094d5de2-0640-4282-9541-d150af7b5ebf" />

3. Actualizamos la página y confirmamos que el nuevo usuario fue creado.  
<img width="1920" height="842" alt="8" src="https://github.com/user-attachments/assets/6aa47050-6b79-486e-bd40-aab81542ddfc" />


---

### 🔹 Invitar a un usuario externo (manual)

1. En el menú desplegable **Nuevo usuario**, seleccionamos **Invitar a un usuario externo**.
<img width="1093" height="758" alt="4" src="https://github.com/user-attachments/assets/3add2312-5ff8-4f56-89d5-4c10f7425821" />

3. Configuramos con los siguientes valores:

| Configuración            | Valor                                      |
|--------------------------|--------------------------------------------|
| Correo electrónico       | Nuestra dirección de correo                |
| Nombre para mostrar      | Nuestro nombre                             |
| Enviar mensaje de invitación | Activado                              |
| Mensaje                  | Bienvenido a Azure y a nuestro Proyecto    |

<img width="1411" height="840" alt="9" src="https://github.com/user-attachments/assets/d30592b8-2cdb-40b9-b085-059fc834573d" />

1. En la pestaña **Propiedades**, completamos la información básica:

| Configuración            | Valor                |
|--------------------------|----------------------|
| Puesto                   | Administrador IT Lab |
| Departamento             | TI                   |
| Ubicación de uso         | United States        |

<img width="1070" height="840" alt="10" src="https://github.com/user-attachments/assets/1c2670e7-f442-478e-ac67-607a9ccecc2f" />

1. Seleccionamos **Revisar + invitar** y luego **Invitar**.
<img width="857" height="838" alt="11" src="https://github.com/user-attachments/assets/63064343-4a04-49ef-b430-5c39f32a4f90" />

3. Confirmamos que el usuario invitado fue creado y recibimos el correo de invitación.  
<img width="1107" height="876" alt="11a" src="https://github.com/user-attachments/assets/e124e170-a4ba-4476-92d6-0ef8291ddcee" />

---

### 📝 Tarea 2: Crear grupos y agregar miembros (manual)

En esta tarea se creará una cuenta de grupo. Las cuentas de grupo pueden incluir cuentas de usuario o dispositivos. Existen dos formas básicas de asignar miembros: **estática** y **dinámica**.

#### 🔹 Membresía estática

- En este modelo, los miembros se agregan **manualmente** al grupo.  
- El administrador selecciona usuarios específicos y los asigna uno por uno.  
- Es útil en escenarios pequeños o controlados, donde los cambios en la composición del grupo son poco frecuentes.  
- Ejemplo: un grupo de administradores de laboratorio donde solo se agregan usuarios internos previamente definidos.  
- **Ventaja:** control total sobre quién pertenece al grupo.  
- **Desventaja:** requiere mantenimiento manual cada vez que se incorporan o eliminan usuarios.

#### 🔹 Membresía dinámica

- En este modelo, los miembros se agregan al grupo de forma **automática** según **reglas definidas** (atributos de usuario como departamento, cargo, ubicación, etc.).  
- El administrador configura una consulta de reglas en Azure AD que determina quién entra o sale del grupo.  
- Es útil en escenarios grandes o cambiantes, donde los usuarios comparten características comunes.  
- Ejemplo: un grupo dinámico que incluye automáticamente a todos los usuarios cuyo departamento sea “TI” o cuya ubicación sea “US”.  
- **Ventaja:** escalabilidad y reducción del trabajo manual.  
- **Desventaja:** requiere una correcta definición de atributos y reglas

---

1. En el **portal de Azure**, buscamos y seleccionamos **Microsoft Entra ID**.

<img width="281" height="887" alt="1" src="https://github.com/user-attachments/assets/0732e0c0-c115-4af2-a0c6-2f30197d4162" />

3. En el panel **Administrar (Manage)**, seleccionamos **Grupos (Groups)**.

<img width="338" height="677" alt="2" src="https://github.com/user-attachments/assets/99911491-17b4-4da3-bf93-2b1cfa6db61e" />

5. Creamos un nuevo grupo con los siguientes valores:

| Configuración   | Valor                                   |
|-----------------|-----------------------------------------|
| Tipo de grupo   | Seguridad (Security)                    |
| Nombre de grupo | Administradores TI LAB                  |
| Descripción     | Administradores que gestionan el laboratorio de TI |
| Tipo de membresía | Asignada (Assigned)                  |

1. Asignamos propietarios y miembros (Bruce Wayne y el usuario invitado).
 
<img width="1920" height="847" alt="13" src="https://github.com/user-attachments/assets/caa667f5-dff6-4fb9-ab7d-4e8217227fa0" />
<img width="1920" height="888" alt="14" src="https://github.com/user-attachments/assets/4b68e8ec-fd52-4d92-869e-3aadf06cac25" />

3. Confirmamos la creación y revisamos la información de miembros y propietarios.

<img width="802" height="836" alt="15" src="https://github.com/user-attachments/assets/af2bfe0a-0b52-44fe-a63b-610458663d9a" />
<img width="1920" height="843" alt="16" src="https://github.com/user-attachments/assets/b36663e0-529a-4553-a10b-f3f4ef532ddc" />
<img width="1683" height="845" alt="17" src="https://github.com/user-attachments/assets/604e2178-6f32-4c69-acad-36a72f6ee370" />

---

## Automatización de las tareas

### Enfoque de la automatización

Mientras realizaba el laboratorio en Microsoft Learn, me surgió una pregunta clave: en el ejercicio se crean solo dos usuarios y un grupo, pero ¿qué ocurriría en un entorno real de empresa donde se deben agregar decenas o incluso cientos de usuarios al mismo tiempo?

Esa inquietud me llevó a pausar el laboratorio y explorar la documentación oficial de Azure sobre automatización de identidades. El objetivo fue simular un escenario más cercano a la práctica profesional, integrando procesos automatizados para la creación de usuarios y grupos.

De esta manera, además de la creación manual en el portal, este laboratorio se complementa con automatización mediante PowerShell (Microsoft Graph) y Azure CLI, lo que permite reproducir y escalar la gestión de identidades de forma más eficiente, reduciendo la carga administrativa y preparando el camino para escenarios más avanzados de administración en la nube.

---

### 🔹 Automatización con Microsoft Graph en PowerShell

```powershell
# Conectarse a Microsoft Graph
Connect-MgGraph -Scopes "User.ReadWrite.All","Group.ReadWrite.All" -TenantId 0c137d82-7c53-47de-be0d-7eb215aff297

# Crear usuario interno
$bruce = New-MgUser -DisplayName "Bruce Wayne" `
            -UserPrincipalName "bruce.wayne@drvoets2024outlook.onmicrosoft.com" `
            -MailNickname "brucewayne" `
            -PasswordProfile @{Password="P@ssw0rd123"; ForceChangePasswordNextSignIn=$true} `
            -AccountEnabled:$true `
            -JobTitle "Administrador IT Lab" `
            -Department "TI" `
            -UsageLocation "US"

# Invitar usuario externo (solo funciona con correo corporativo, no hotmail/outlook)
$invitation = New-MgInvitation -InvitedUserEmailAddress "diegorojas@motoingeniero2026outlook.onmicrosoft.com" `
                 -InviteRedirectUrl "https://portal.azure.com" `
                 -SendInvitationMessage:$true `
                 -InvitedUserDisplayName "Usuario Invitado"

# Obtener IDs de usuarios
$bruceId = $bruce.Id
$guestId = (Get-MgUser -Filter "userPrincipalName eq 'diegorojas@motoingeniero2026outlook.onmicrosoft.com'").Id

# 1. Crear el grupo vacío
$group = New-MgGroup -DisplayName "Administradores TI LAB" `
                     -MailEnabled:$false `
                     -MailNickname "ITLabAdmins" `
                     -SecurityEnabled:$true `
                     -Description "Administradores que gestionan el laboratorio de TI"

# 2. Recuperar usuarios ya existentes
$bruce = Get-MgUser -Filter "userPrincipalName eq 'bruce.wayne@drvoets2024outlook.onmicrosoft.com'"
$guest = Get-MgUser -Filter "userPrincipalName eq 'diegorojas@motoingeniero2026outlook.onmicrosoft.com'"

# 3. Agregar miembros al grupo
New-MgGroupMember -GroupId $group.Id -BodyParameter @{
    "@odata.id" = "https://graph.microsoft.com/v1.0/directoryObjects/$($bruce.Id)"
}

New-MgGroupMember -GroupId $group.Id -BodyParameter @{
    "@odata.id" = "https://graph.microsoft.com/v1.0/directoryObjects/$($guest.Id)"
}

```
<img width="1102" height="845" alt="20" src="https://github.com/user-attachments/assets/a0bacfc3-7e2c-43fd-9f14-fe634e7b0d7b" />
<img width="1102" height="842" alt="21" src="https://github.com/user-attachments/assets/af2094ea-055b-45d0-b30b-39e88a64a5e1" />
<img width="1920" height="886" alt="22" src="https://github.com/user-attachments/assets/989c91a9-0f4e-4d5f-8a2e-de2a4585ec88" />
<img width="1805" height="717" alt="23" src="https://github.com/user-attachments/assets/70d3e98e-2a9d-4353-933f-5d65b2adf052" />
<img width="1920" height="843" alt="24" src="https://github.com/user-attachments/assets/27e2f209-0f2a-4cdf-b1d9-a44bf7d7f31e" />
<img width="1102" height="845" alt="25" src="https://github.com/user-attachments/assets/642ebb33-5902-4042-83f0-b8c5c342c192" />

---

## Errores encontrados con Microsoft Graph PowerShell

>⚠️ Nota: La invitación de usuarios externos mediante Microsoft Graph solo funciona con correos corporativos
>(ejemplo: <usuario@empresa.com>). Si se intenta invitar una cuenta MSA (hotmail.com, outlook.com), Graph devuelve error.

### 1. **Invalid target for navigation property update**

```
Invalid target for navigation property update. URI must target an entity.
Status: 400 (BadRequest)
```

- **Cuándo ocurrió:** al intentar crear el grupo y asignar miembros/owners en un solo paso usando `New-MgGroup -BodyParameter`.
- **Causa:** el invitado externo aún no estaba disponible como objeto válido en el tenant (estado *PendingAcceptance*).
- **Solución:** crear el grupo vacío primero y luego agregar miembros con `New-MgGroupMember`. Para invitados, usar el `Id` de `$invitation.InvitedUser.Id`.

---

### 2. **ResourceNotFound (404)**

```
Get-MgUser_Get: Resource 'diegorojas@motoingeniero2026outlook.onmicrosoft.com' does not exist...
Status: 404 (NotFound)
```

- **Cuándo ocurrió:** al ejecutar `Get-MgUser` inmediatamente después de enviar la invitación.
- **Causa:** el invitado aún no había aceptado la invitación, por lo que no existía como objeto en el directorio.
- **Solución:** esperar a que el invitado acepte la invitación (correo o `InviteRedeemUrl`). Una vez aceptada, aparece como *Guest* en el tenant.

---

### 3. **Duplicate UserPrincipalName**

```
Another object with the same value for property userPrincipalName already exists.
Status: 400 (BadRequest)
```

- **Cuándo ocurrió:** al intentar crear a Bruce Wayne más de una vez con el mismo `UserPrincipalName`.
- **Causa:** ya existía un usuario con ese UPN en el tenant.
- **Solución:** usar otro UPN (ejemplo: `bruce.wayne2@...`) o recuperar el usuario existente con `Get-MgUser`.

---

### 4. **Invalid target al agregar invitado**

```
Invalid target for navigation property update. URI must target an entity.
```

- **Cuándo ocurrió:** al intentar agregar el invitado al grupo usando `$guest.Id` cuando `Get-MgUser` aún no devolvía nada.
- **Causa:** `$guest` era nulo porque el invitado no estaba completamente aprovisionado.
- **Solución:** usar directamente el GUID de `$invitation.InvitedUser.Id` en el `@odata.id`.

---

### ✅ Resultado final

- Bruce Wayne (interno) agregado como **Member**.  
- Diego Rojas (externo, otro tenant) agregado como **Guest** usando el `Id` de la invitación.  
- Verificación con `Get-MgGroupMember` y `Get-MgUser` muestra ambos usuarios en el grupo.

---

## Automatización con Azure CLI o Azure Cloud Shell

### Limitación importante de Azure CLI

Azure CLI permite **crear usuarios internos**, **crear grupos** y **agregar miembros internos** a dichos grupos.  
Sin embargo, **no soporta la invitación ni la gestión de usuarios externos (Guest)** en Microsoft Entra ID.  

- Los comandos como `az ad user create` y `az ad group member add` funcionan únicamente con cuentas internas del tenant.  
- No existe un comando `az ad user invite` en Azure CLI.  
- Para invitar usuarios externos (por ejemplo, de otro tenant o con correo corporativo), se debe usar **Microsoft Graph PowerShell SDK** (`New-MgInvitation`) o la **API REST de Microsoft Graph**.

💡 **Conclusión:**  
En este laboratorio, los **usuarios internos** se gestionan con Azure CLI, mientras que los **usuarios externos invitados** se gestionan con **Microsoft Graph PowerShell**. Esto refleja las capacidades reales de cada herramienta y evita errores al intentar usar comandos no soportados.


```powershell
# Crear usuario
az ad user create --display-name "Bruce Wayne" `
  --user-principal-name "bruce.wayne@drvoets2024outlook.onmicrosoft.com" `
  --password "P@ssw0rd123" `
  --force-change-password-next-sign-in true `

az ad user update --id "bruce.wayne@drvoets2024outlook.onmicrosoft.com" --set department="TI" jobTitle="Administrador TI Lab" usageLocation="US"

# Crear grupo
az ad group create --display-name "Administradores TI LAB" `
  --mail-nickname "TILabAdmins" `
  --description "Administradores que gestionan el laboratorio de TI"

# Agregar miembros
$GROUP_ID = az ad group show --group "Administradores TI LAB" --query id -o tsv
$USER_ID  = az ad user show --id "bruce.wayne@drvoets2024outlook.onmicrosoft.com" --query id -o tsv

az ad group member add --group "Administradores TI LAB" --member-id $USER_ID

```

---

## 🔄 Otras formas de automatización

>💡 **Nota:** Este apartado no forma parte del laboratorio práctico, sino que se incluye como información adicional para demostrar conocimiento de otras formas de automatización en Azure y mostrar que se conoce también **ARM y Bicep** como alternativas profesionales para escalar la gestión de identidades mediante IaC.

En este laboratorio usamos **Microsoft Graph PowerShell SDK** directamente desde la consola para crear usuarios y grupos de manera manual.  
Una alternativa profesional para escalar y programar estos mismos scripts es **Azure Automation**, un servicio en la nube que permite ejecutar **runbooks** (scripts automatizados) con PowerShell o Azure CLI de manera programada y gestionada.  
De esta forma, las tareas de gestión de identidades pueden automatizarse sin depender de la ejecución manual en un equipo local.

### 📌 Escenarios posibles con Azure Automation

- **Creación masiva de usuarios**: ejecutar un runbook que lea un archivo CSV y cree decenas o cientos de cuentas en Entra ID.  
- **Gestión de grupos**: automatizar la creación de grupos de seguridad y la asignación de miembros según reglas o datos externos.  
- **Tareas recurrentes**: programar la ejecución periódica de scripts para mantener sincronizadas las identidades con sistemas externos.  

### 📝 Ejemplo conceptual de runbook en PowerShell

```powershell
# Conectarse a Microsoft Graph con Managed Identity
Connect-MgGraph -Identity

# Leer usuarios desde un archivo CSV
$usuarios = Import-Csv "usuarios.csv"

foreach ($u in $usuarios) {
    New-MgUser -DisplayName $u.Nombre `
               -UserPrincipalName $u.UPN `
               -MailNickname $u.Alias `
               -PasswordProfile @{Password=$u.Password; ForceChangePasswordNextSignIn=$true} `
               -AccountEnabled $true `
               -JobTitle $u.Puesto `
               -Department $u.Departamento `
               -UsageLocation $u.Ubicacion
}

# Crear grupo y asignar miembros
$grupo = New-MgGroup -DisplayName "Administradores TI LAB" `
                     -MailEnabled:$false `
                     -MailNickname "ITLabAdmins" `
                     -SecurityEnabled:$true

foreach ($u in $usuarios) {
    $id = (Get-MgUser -UserPrincipalName $u.UPN).Id
    Add-MgGroupMember -GroupId $grupo.Id -DirectoryObjectId $id
}
```

---

## 📌 ARM y Bicep

Otra alternativa para automatizar la gestión de identidades en **Microsoft Entra ID** es el uso de **ARM templates** y **Bicep**.  

Estos enfoques permiten definir recursos de Azure de manera declarativa, integrando la gestión de identidades dentro de la filosofía de **Infraestructura como Código (IaC)**. Aunque no todos los objetos de identidad están disponibles directamente como recursos ARM, se pueden utilizar **deployment scripts** dentro de los templates para ejecutar comandos de **PowerShell (Microsoft Graph)** o **Azure CLI**, logrando así la creación de usuarios y grupos de forma automatizada.

### 🔹 Escenarios posibles

- **Creación de grupos de seguridad**: definirlos como recursos declarativos en Bicep o ARM.  
- **Automatización de usuarios**: usar deployment scripts que lean datos desde archivos CSV y creen usuarios en Entra ID.  
- **Integración en pipelines CI/CD**: versionar y desplegar identidades junto con otros recursos de infraestructura.  

### 📝 Ejemplo conceptual en Bicep

```bicep
// Ejemplo conceptual: crear un grupo en Entra ID
resource itLabAdmins 'Microsoft.Graph/groups@1.0' = {
  displayName: 'Administradores TI LAB'
  description: 'Administradores que gestionan el laboratorio de TI'
  mailEnabled: false
  mailNickname: 'ITLabAdmins'
  securityEnabled: true
}
```

---

### 📝 Ejemplo conceptual en ARM (JSON)

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "resources": [
    {
      "type": "Microsoft.Graph/groups",
      "apiVersion": "1.0",
      "name": "AdministradoresTILAB",
      "properties": {
        "displayName": "Administradores TI LAB",
        "description": "Administradores que gestionan el laboratorio de TI",
        "mailEnabled": false,
        "mailNickname": "ITLabAdmins",
        "securityEnabled": true
      }
    }
  ]
}
```

---

## 📊 Comparativa de enfoques de automatización

| Enfoque                        | Características principales                                                                 | Escenarios ideales                                                                 |
|--------------------------------|---------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------|
| **PowerShell (Microsoft Graph)** | - Cmdlets modernos (`New-MgUser`, `New-MgGroup`) <br> - Control granular <br> - Scripts reproducibles | - Creación de usuarios y grupos <br> - Integración con pipelines <br> - Escenarios de administración avanzada |
| **Azure CLI**                   | - Comandos simples y directos (`az ad user create`, `az ad group create`) <br> - Multiplataforma <br> - Fácil de integrar en scripts Bash | - Automatización rápida <br> - Integración con DevOps <br> - Escenarios multiplataforma |
| **Azure Automation**            | - Runbooks programados <br> - Integración con Graph/CLI <br> - Ejecución recurrente y masiva | - Creación masiva de usuarios desde CSV <br> - Tareas periódicas <br> - Escenarios empresariales |
| **ARM / Bicep (IaC)**           | - Declarativo y versionable <br> - Integración con CI/CD <br> - Deployment scripts para identidades | - Infraestructura como Código <br> - Integración en pipelines <br> - Escenarios de gobernanza y trazabilidad |

---
<br>

---

## 🤝 Contribuciones

Las contribuciones son bienvenidas.  

1. Haz un fork del repositorio.  
2. Crea una rama (`git checkout -b feature/nueva-funcionalidad`).  
3. Haz commit de tus cambios (`git commit -m 'Agregada nueva funcionalidad'`).  
4. Haz push (`git push origin feature/nueva-funcionalidad`).  
5. Abre un Pull Request.

---

## 📜 Licencia

Este proyecto está bajo la licencia MIT.

---
