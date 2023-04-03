---
title: Actualizando a Ubuntu 22.04
date: 2022-06-18
categories:
  - artículos
lastmod: 2022-07-02T11:04:34.760Z
---

## Resumen

1. [Descargo de responsabilidad](#anchor-1)
2. [Suposiciones](#anchor-2)
3. [Actualizar Ubuntu](#anchor-3)
4. [Notas acerca de la seguridad del sistema](#anchor-4)

## Descargo de responsabilidad {#anchor-1}

Si bien se han hecho todos los esfuerzos posibles para garantizar que la información presentada en esta quía (y en todo el sitio web) sea precisa, el autor, Eduardo Alemán, no asume responsabilidad por errores, omisiones o información desactualizada y no será responsable de ningún modo por el uso directo, indirecto o indirecto, daños especiales, incidentales, consecuentes, punitivos u otros que resulten de la disponibilidad, el uso, el acceso o la imposibilidad de utilizar esta información.

El autor de esta guía no garantiza que la información contenida en este documento es en todos los aspectos precisa o completa, y no es responsable de los errores u omisiones o por los resultados obtenidos por el uso de dicha información.

Se anima a los lectores a confirmar y verificar la información contenida en este documento con otras fuentes, especialmente las guías oficiales proporcionadas por [Ubuntu](https://help.ubuntu.com/stable/ubuntu-help/index.html.es).

La información que se ofrece a través de este sitio web es solamente un resumen general preparado para el público. No pretende sustituir las guías anteriormente mencionadas. No soy tampoco responsable por el contenido de estas guías o por cualquiera de los enlaces aquí proporcionados.

## Suposiciones {#anchor-2}

Esta guía supone que el lector:

- Con la excepción de Ubuntu, no ha instalado ninguno de los programas aquí mencionados previamente.
- Desea actualizar directamente a la versión 22.04 desde Ubuntu 21.10 o 20.04. (Si tiene una versión anterior a 20.04, primero debe actualizar a Ubuntu a 20.04, etc.)
- Tiene suficiente espacio libre en el disco (>40 gb)
- Tiene acceso _sudo_.
- Puede utilizar la terminal de comandos vía SSH.
- Puede leer cuidadosamente las instrucciones.

## Actualizar Ubuntu {#anchor-3}

Verifica si tienes al menos 40 Gb de espacio libre en tu disco:

```
df -H
```

Verifica la versión de Ubuntu que tienes. Si NO estás en la versión exactamente anterior a 22.04, es decir 21.10 o 20.04, le recomiendo que repita todo el proceso una vez terminado por completo:

```
lsb_release -a
```

### Copia de seguridad

En primer lugar, asegúrese de hacer una copia de seguridad de sus datos antes de iniciar la actualización principal de su sistema operativo. Si está ejecutando Ubuntu en una máquina virtual, es mejor tomar una instantánea completa del sistema para restaurar rápidamente su máquina en caso de que la actualización salga mal.

### Actualizar los paquetes existentes

Los paquetes marcados como retenidos no se pueden instalar, actualizar ni eliminar automáticamente. Esto puede causar problemas durante el proceso de actualización. Para verificar si hay paquetes retenidos en sus sistemas, ejecute:

```
sudo apt-mark showhold
```

Una salida vacía significa que no hay paquetes retenidos. Si hay paquetes en espera, debe liberar los paquetes con:

```
sudo apt-mark unhold nombre-del-paquete
```

Actualice la lista de _**apt**_ y actualice todos los paquetes instalados:

```
sudo apt-get update    # esto actualiza el repositorio de paquetes
sudo apt-get upgrade   # esto procede a actualizar los paquetes existentes 
                       # - aun no el sistema operativo por completo
```

Si el _kernel_ o núcleo del sistema es actualizado, reinicie la máquina y, espere un par deminutos, y vuelva a iniciar sesión.

### Comience una actualización del sistema:

```
sudo apt full-upgrade
```

apt full-upgrade puede eliminar algunos paquetes actualmente instalados que impiden actualizar el sistema existente en su totalidad.

Continúe con:

```
sudo apt --purge autoremove
```

```
sudo reboot
```

### Actualice el sistema a Ubuntu 22.04 LTS (Jammy Jellyfish)

Asegúrese de que la política de actualización predeterminada en el archivo _/etc/update-manager/release-upgrades_ esté establecida en "Prompt=normal" o "Prompt=lts". De lo contrario, el proceso de actualización no se iniciará.

```
sudo nano /etc/update-manager/release-upgrades
```

_do-release-upgrade_ es parte del paquete "update-manager-core" que se instala de forma predeterminada en la mayoría de los sistemas Ubuntu. Si, por alguna razón, no está instalado en su sistema, instálelo con:

```
sudo apt install update-manager-core
```

Comience el proceso de actualización

```
sudo do-release-upgrade -d
```

El comando _do-release-upgrade_ deshabilitará todos los repositorios de terceros y cambiará la lista _apt_ para señalar los repositorios "jammy". Se le pedirá varias veces que confirme que desea continuar con la actualización.

Durante el proceso de actualización, el comando le hará varias preguntas, como si desea mantener un archivo de configuración existente o instalar la versión del mantenedor del paquete. Si no realizó ningún cambio personalizado en el archivo, debería ser seguro escribir **Y**. De lo contrario, se recomienda mantener la configuración ya existente. Lea atentamente las preguntas antes de hacer una selección.

Cuando se le pregunte si desea que los servicios se reinicien automáticamente durante la actualización, escriba **Y**.

La actualización se ejecuta dentro de una sesión de pantalla GNU y se volverá a conectar automáticamente si se interrumpe la conexión.

Todo el proceso puede llevar algún tiempo dependiendo de la cantidad de actualizaciones y la velocidad de Internet.

Una de las preguntas es que le hará el sistema mientras se actualiza es si deseas mantener el archivo ssh\_config ya que ha sido localmente modificado. Esta guía asume que usted no ha realizado cambios en el archivo, o que las diferencias son mínimas e irrelevantes para su configuración, por lo que puede ignorar y continuar con la instalación de la versión del mantenedor del paquete sin necesidad de editar más el archivo.

Una vez instalados los nuevos paquetes, la herramienta de actualización le preguntará si desea eliminar el software obsoleto. Si no está seguro, ingrese la letra **D** y consulte la lista de paquetes obsoletos. Generalmente, es seguro ingresar **Y** y eliminar todos los paquetes obsoletos.

Cuando se complete el proceso de actualización y suponiendo que todo salió bien, se le pedirá que reinicie su máquina. Ingrese **Y** para continuar:

```
System upgrade is complete.

Restart required

To finish the upgrade, a restart is required.
If you select 'y' the system will be restarted.

Continue [yN] y
```

Reinicie el sistema.

Confirme que el sistema Ubuntu ha sido actualizado:

```
lsb_release -a
```

La pantalla debe mostrar:

```
No LSB modules are available.
Distributor ID:	Ubuntu
Description:	Ubuntu 22.04 LTS
Release:	22.04
Codename:	jammy
```

## Notas acerca de la seguridad del sistema {#anchor-4}

La seguridad de los sistemas operativos, los diversos servidores y programas es un tema muy complejo. Estos son diseñados con seguridad, pero desafortunadamente, siempre se encuentran vulnerabilidades que pueden ser explotadas por hackers. Lo mejor que puedes hacer para comenzar a mantener la seguridad del sistema es mantenerlo actualizado y mantenerte informado.

Una vez más, el primer paso siempre: actualizar la instalación de su software.

```
sudo apt update
sudo apt upgrade
```

A continuación algunas recomendaciones adicionales **que están muy lejos de ser exhaustivas**:

1. Siempre utiliza un cortafuegos. Para más información, consulta la guía oficial del servidor Ubuntu, comenzando con la página 86 en [https://assets.ubuntu.com/v1/f954307f-ubuntu-server-guide.pdf](https://assets.ubuntu.com/v1/f954307f-ubuntu-server-guide.pdf).
2. Configurar inicio de sesión SSH sin contraseña en Ubuntu. Esto está habilitado por defecto si instalastes Ubuntu siguiendo las instrucciones en esta guía. Déjalo así.Los desarrolladores de Ubuntu tomaron una decisión concienzuda para deshabilitar la cuenta raíz administrativa de forma predeterminada en todas las instalaciones de Ubuntu. Esto no significa que la cuenta raíz haya sido eliminada o que no pueda ser accedida. Simplemente se le ha dado un hash de contraseña que no coincide con ningún valor posible, por lo tanto, no puede iniciar sesión directamente por sí mismo. En su lugar, se anima a los usuarios a hacer uso de una herramienta con el nombre de 'sudo' para llevar a cabo tareas administrativas del sistema. En esta guía la hemos utilizado continuamente. Sudo permite que un usuario autorizado eleve temporalmente sus privilegios usando su propia contraseña en lugar de tener que saber la contraseña que pertenece a la cuenta raíz. Esta sencilla pero eficaz metodología proporciona responsabilidad por todas las acciones del usuario y le da al administrador un control granular sobre qué acciones un usuario puede realizar con dichos privilegios.
3. Si utlizas contraseñas para conectarte a tu sistema Ubuntu via SSH (no recomendado), puedes usar Fail2Ban. Ten en cuenta que "los intentos de intrusión por fuerza bruta son bastante frecuentes contra un servidor SSH y otros servicios de Internet protegidos con contraseña (como ftp, pop,…). Los scripts automatizados prueban múltiples combinaciones de nombre de usuario/contraseña (fuerza bruta, ataque de diccionario) y, a veces, no se puede cambiar el puerto a otro que no sea el predeterminado. Además, revisar sus archivos de registro usted mismo no solo requiere mucho tiempo, sino que también puede ser difícil. Fail2ban intenta aliviar estos problemas proporcionando una forma automatizada no solo de identificar posibles intentos de intrusión, sino también de actuar sobre ellos rápida y fácilmente de una manera definible por el usuario." Para más información, consulta el manual oficial de Fail2Ban disponible en [https://www.fail2ban.org/wiki/index.php/MANUAL\_0\_8](https://www.fail2ban.org/wiki/index.php/MANUAL_0_8).
4. Installa y configura Rkhunter: El paquete "rkhunter" es útil para realizar un análisis rápido de su sistema en busca de rootkits conocidos (programas informático maliciosos y clandestinos diseñados para proporcionar acceso privilegiado continuo a una computadora mientras oculta activamente su presencia). Para más información visite [https://help.ubuntu.com/community/RKhunter](https://help.ubuntu.com/community/RKhunter).
