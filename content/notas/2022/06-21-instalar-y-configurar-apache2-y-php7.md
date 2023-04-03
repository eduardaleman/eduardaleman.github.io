---
title: Instalar y configurar Apache2 y PHP 7.4 en Ubuntu 22.04
date: 2022-06-21
categories:
  - artículos
lastmod: 2022-07-02T11:03:49.916Z
---

## Resumen 

1. [Descargo de responsabilidad](#anchor-1)
2. [Suposiciones](#anchor-2)
4. [Instalar y configurar Apache2](#anchor-3)
5. [Instalar y configurar PHP 7.4 y varios módulos necesarios](#anchor-4)
6. [Instalar y configurar HTTPS usando un certificado gratis de ZeroSSL](#anchor-5)
7. [Notas acerca de la seguridad del sistema](#anchor-6)

## Descargo de responsabilidad {#anchor-1}

Si bien se han hecho todos los esfuerzos posibles para garantizar que la información presentada en esta quía (y en todo el sitio web) sea precisa, el autor, Eduardo Alemán, no asume responsabilidad por errores, omisiones o información desactualizada y no será responsable de ningún modo por el uso directo, indirecto o indirecto, daños especiales, incidentales, consecuentes, punitivos u otros que resulten de la disponibilidad, el uso, el acceso o la imposibilidad de utilizar esta información.

El autor de esta guía no garantiza que la información contenida en este documento es en todos los aspectos precisa o completa, y no es responsable de los errores u omisiones o por los resultados obtenidos por el uso de dicha información.

Se anima a los lectores a confirmar y verificar la información contenida en este documento con otras fuentes, especialmente las guías oficiales proporcionadas por [Ubuntu](https://help.ubuntu.com/stable/ubuntu-help/index.html.es) y [Apache2](https://httpd.apache.org/docs/2.4/es/).

La información que se ofrece a través de este sitio web es solamente un resumen general preparado para el público. No pretende sustituir las guías anteriormente mencionadas. No soy tampoco responsable por el contenido de estas guías o por cualquiera de los enlaces aquí proporcionados.

## Suposiciones {#anchor-2}

Esta guía supone que el lector:

1. Su sistema es Ubuntu 22.04, si desea actualizar, vea mi artículo: [Actualizando a Ubuntu 22.04](/notas/2022/06-18-actualizando-a-ubuntu-22-04/).
2. Tiene acceso _sudo_ vía SSH.
3. Puede utilizar la terminal de comandos.
4. Puede leer cuidadosamente las instrucciones.
5. Desea instalar y configurar el sistema en este orden:
    - Apache2
    - PHP 7.4 y varios módulos
    - Configurar HTTPS
6. Tiene abiertos los puertos 80 y 443 o puede abrirlos en su cortafuegos (firewall, en inglés).

## Instalar y configurar Apache2 {#anchor-3}

```
sudo apt-get install apache2
```

Verifica que puedes acceder a la página en tu navegador. Si el sistema tiene asignado una dirección, utilízala. Por ejemplo:

```
http//nombredetusitio.com/ 
o 
http://direcciónIPdetusitio/
```

El resultado debe ser una página en la que se verifica que Apache2 está funcionando correctamente en Ubuntu.

### Configurar Apache2

Hay varias maneras de configurar Apache2 para MediaWiki. Las instrucciones siguientes sirven como ejemplo solamente. Empezaremos creando _hosts virtuales_.

### Hosts virtuales

El concepto de _hosts virtuales_ permite más de un sitio web en un servidor web. Los servidores se diferencian por su nombre de _host_. Los visitantes del sitio web se enrutan por nombre de _host_ o _dirección IP_ al host virtual correcto. El alojamiento virtual permite que las entidades que comparten un servidor tengan sus propios nombres de dominio.

### Sitios disponibles (Sites-available)

Nota: Dependiendo de la configuración del sistema el lugar exacto de los directorios puede diferir.

El directorio _/etc/apache2/sites-available_ contiene archivos de configuración para los _hosts virtuales_ de Apache. Recuerde que los _hosts virtuales_ permiten configurar Apache para múltiples sitios que tienen configuraciones separadas.

Los archivos de configuración contendrán directivas como el nombre del sitio, las rutas del directorio donde están los archivos del sitio y algunas funciones que ha configurado para su sitio.

El archivo _000-default.conf_ contiene directivas de configuración para el servidor web predeterminadas, como las directivas de directorio que ubican el sitio predeterminadamente en _/var/www/html_. En esta guía vamos a desactivar este archivo de configuración y crear unos nuevos para nuestras necesidades.

Por último, para que un sitio sea accesible, se debe crear un enlace a su archivo de configuración en el directorio _/etc/apache2/sites-enabled_. Esto se hace usando el comando **a2ensite**. Para deshabilitar un sitio web, se usa el comando **a2dissite**. Vamos a proceder, paso a paso.

Navega (cámbiate) al directorio de _sites-available_:

```
cd /etc/apache2/sites-available
```

En este directorio podrás ver el archivo predeterminado **000-default.conf** que se configuró automáticamente durante la instalación de Apache2. Para que lo veas, ejecuta el siguiente comando:

```
ls /etc/apache2/sites-available
```

Si te cambiaste a este directorio como te señalamos anteriormente, simplemente podrías ejecutar:

```
ls
```

Vamos a deshabilitar el archivo preinstalado de configuración de Apache2 **000-default.conf** con el siguiente comando:

```
sudo a2dissite 000-default
```

No hay necesidad de ejecutar _a2dissite 000-defaul.conf_ ya que el comando **a2dissite** solo ejecuta archivos de configuración con la extensión _.conf_. Si ahora intentas acceder tu sitio con un navegador, ya no podrás ver la página inicial, ya que la hemos inabilitado con ****a2dissite****.

Ahora vamos a crear un archivo de configuración de Apache2 nuevo. Existen varias formas de hacer esto, nosotros estaremos utilizando un editor de texto que se llama **nano**. Pero primeramente asegúrate de que esté instalado con este comando:

```
sudo apt install nano -y
```

Si está instalado el editor de texto nano el sistema te lo dirá. Si no, será instalado. Ahora podemos proceder con la creación del archivo de configuración. Recuerda que asumimos que estas adentro del directorio _sites-available_. Por supuesto, asegúrate de sustituir _nombredelsitiocompleto_ y _ejemplo.com_ con el nombre de tu propio dominio. Si no has adquirido un dominio y deseas usar la dirección IP, asegúrate de sustituir con la dirección IP.

```
sudo nano nombredelsitiocompleto.conf
```

Por ejemplo:

```
sudo nano ejemplo.com.conf 
```

Con el archivo abierto, continua con la sección de abajo.

### Configuración del archivo ejemplo.com.conf

En esta guía no entraremos en mucho detalle acerca de cada una de las directivas en este archivo, solamente le mostraremos a continuación un ejemplo de su contenido con algunos comentarios. Nuevamente, asegúrate de sustituir el nombre del sitio con el tuyo, al igual que el nombre, y también la ruta donde Apache2 debe buscar por la raíz del directorio donde se guardaran los documentos públicos de tu sitio si deseas cambiarla. El símbolo # indica un comentario, por lo que Apache2 lo ignorará cuando ejecute este archivo. Usted debe leer estos pocos comentarios si gusta entender los pasos siguientes. De lo contrario, puede ignorarlos.

```
<VirtualHost *:80>
# La directiva ServerName establece el esquema de solicitud, el nombre de host y el 
# puerto que el servidor usa para identificarse. 
# Esto se usa al crear URL de redirección. 
# En el contexto de hosts virtuales, ServerName
# especifica qué nombre de host debe aparecer en el encabezado Host: de la solicitud para
# coincide con este host virtual. Para el host virtual predeterminado (este archivo) este
# valor no es decisivo, ya que se utiliza como host de último recurso independientemente.
# Sin embargo, debe configurarlo explícitamente para cualquier host virtual adicional.
	ServerName www.ejemplo.com
	ServerAdmin correo@ejemplo.com
# DocumentRoot es el directorio de nivel superior en el árbol de documentos visible desde la
# web y esta directiva establece el directorio en la configuración desde la cual Apache2 busca
# y entrega archivos web desde la URL solicitada a la raíz del documento.
	DocumentRoot /var/www/html/ejemplo.com
	<Directory /var/www/html/ejemplo.com>
	</Directory>
	# Donde puede acceder a los los registros con información acerca de errores y acceso.
	ErrorLog ${APACHE_LOG_DIR}/error.log
	CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

Guarde el archivo y sálgase del editor de texto **nano** utilizando los siguientes comandos:

```
^x  # Usa el teclado. ^ es la tecla CONTROL. 
    # El editor nano le preguntara si desea guardar el archivo
```

```
Y   # Y, por "yes" en inglés, le estas diciendo al ordenador que sí, que guarde el contenido.
```

Finalmente, presiona **ENTER o RETURN** para salirte de **nano**.

### Establecer el directorio raíz (DocumentRoot, en inglés)

Anteriormente le dijimos a Apache2 que el directorio raíz se encontraba en el siguiente lugar: _/var/www/ejemplo.com_, por lo que tenemos que crear un directorio que se llame _ejemplo.com_ dentro de _/var/www/_. (Recuerda sustituir _ejemplo.com_ con el nombre de tu dominio.) Utilizaremos el comando **mkdir** (_make directory_, en inglés) para crear una carpeta para nuestro sitio en la ruta _/var/www/_.

```
mkdir /var/www/html/ejemplo.com        # ejemplo.com es el nombre del directorio
                                       # que estamos creando
```

Ahora vamos a comprobar que nuestra configuración es correcta y que Apache2 puede ejecutarla.

```
apachectl configtest
```

El resultado debe ser indicado con el siguiente resultado en la pantalla de su terminal:

```
Syntax OK
```

Esto indica que la sintaxis del archivo de configuración _ejemplo.com.conf_ es correcta. Si Apache2 se queja de lo contrario, deberás revisar que no hayas escrito o pegado alguna directiva incorrectamente. El mas mínimo error, por ejemplo si le has quitado el símbolo # a un comentario, será indicado y puede impedir el funcionamiento correcto de tu servidor. Suponiendo que todo anda bien, y que todavía estés dentro del directorio _sites-available_ ubicado en _/etc/apache2/_ (si te has salido, navega a _/etc/apache2/sites-available_ usando el comando **cd** **/etc/apache2/sites-available**) puedes habilitar tu sitio con el siguiente comando:

```
sudo a2ensite ejemplo.com
```

Puedes comprobar que tu sitio ha sido habilitado primero navegando al directorio _sites-enabled_ y luego verificando su contenido:

```
cd /etc/apache2/sites-enabled
```

```
ls /etc/apache2/sites-enabled
```

Activa la nueva configuración:

```
systemctl reload apache2
```

Podrás ver que _sites-enabled_ contiene una copia idéntica de _ejemplo.com.conf_. En estos momentos si tratas de usar un navegador para acceder tu sitio _ejemplo.com_ te encontrarás con un error o quizás con una página en blanco. Esto es porque en la configuración le dijimos a Apache2 que buscara por un archivo nombrado _index.php_ en el directorio raíz del árbol de directorios ubicado en _/var/www/ejemplo.com_. Sin embargo, aún no hemos creado tal archivo. Cuando instalemos mediawiki adecuadamente unos pasos mas adelante, el programa creará un archivo con este nombre en el susodicho directorio.

Si has seguido las instrucciones hasta este momento, ya tendrás un servidor con Ubuntu 22.04 y Apache2 instalados. Continua a continuación con la instalación de PHP, versión 7.4 y algunos de sus módulos.

## Instalar y configurar PHP 7.4 y varios módulos {#anchor-4}

El siguiente comando instalará PHP y varios módulos.

sudo apt-get update

sudo apt -y install software-properties-common

sudo add-apt-repository ppa:ondrej/php

sudo apt-get update

sudo apt-get -y install php7.4

sudo apt-get install -y php7.4-cli php7.4-json php7.4-common php7.4-mysql php7.4-zip php7.4-gd php7.4-mbstring php7.4-curl php7.4-xml php7.4-bcmath php7.4-intl

sudo apt-get upgrade

Asegúrate de no alterar los números de las versiones. Esto garantizará la instalación de versiones que ya han sido comprobadas, calificadas como _estables_, y que funcionan correctamente con varios programas como MediaWiki que posteriormente instalaremos. De lo contrario, podrías instalar una versión de PHP, por ejemplo PHP 8.1, que aunque nueva, no es compatible en estos momentos (junio, 2022) con MediaWiki.

Comprueba que PHP ha sido instalado correctamente:

```
php --version
```

El resultado en la pantalla de la terminal debe ser similar a:

```
PHP 7.4.30 (cli) (built: Mar 10 2022 13:33:49) ( NTS )
Copyright (c) The PHP Group
Zend Engine v3.4.0, Copyright (c) Zend Technologies
    with Zend OPcache v7.4.30, Copyright (c), by Zend Technologies
```

Puedes también ver una lista de los módulos PHP instalados:

php -m

### (Opcional) Instalar y configurar el modulo PHP-FPM

FPM es un método altamente eficiente para reducir el consumo de memoria y aumentar el rendimiento de sitios web con mucho tráfico. Es una gran mejora con respecto a los métodos basados en CGI en entornos PHP multiusuario:

sudo apt-get install php7.4-fpm

Comprueba que php-fmp ha sido instalado correctamente:

```
sudo service php$(php -r 'echo PHP_MAJOR_VERSION.".".PHP_MINOR_VERSION;')-fpm status

^C //to exit
```

El resultado en la pantalla de la terminal debe ser similar a:

```
php7.4-fpm.service - The PHP 7.4 FastCGI Process Manager
     Loaded: loaded (/lib/systemd/system/php7.4-fpm.service; enabled; vendor pr>
     Active: active (running) since Sat 2020-06-18 00:52:46 UTC; 19h ago
       Docs: man:php-fpm7.4(8)
    Process: 30287 ExecStartPost=/usr/lib/php/php-fpm-socket-helper install /ru>
   Main PID: 30284 (php-fpm7.4)
     Status: "Processes active: 0, idle: 3, Requests: 1483, slow: 0, Traffic: 0>
      Tasks: 4 (limit: 4692)
     Memory: 152.9M
        CPU: 1min 22.849s
     CGroup: /system.slice/php7.4-fpm.service
             ├─30284 "php-fpm: master process (/etc/php/7.4/fpm/php-fpm.conf)" >
             ├─30285 "php-fpm: pool www" "" "" "" "" "" "" "" "" "" "" "" "" "">
             ├─30286 "php-fpm: pool www" "" "" "" "" "" "" "" "" "" "" "" "" "">
             └─30795 "php-fpm: pool www" "" "" "" "" "" "" "" "" "" "" "" "" "">
```

### (Opcional) Instalar y configurar el modulo php-apcu en php.ini

> Nota: Uno de los módulos que instalamos anteriormente es _php7.4-apcu_. APCu es un almacén de clave-valor en memoria para PHP. Las claves son de tipo cadena y los valores pueden ser cualquier variable de PHP. APCu solo admite el almacenamiento en caché (prememoria) de variables en el espacio del usuario. APCu es el reemplazo oficial de la extensión APC obsoleta. APC proporcionó tanto el almacenamiento en caché de código de operación (opcache) como el almacenamiento en caché de objetos. Como las versiones de PHP 5.5 y superiores incluyen su propio opcache, APC ya no era compatible y su funcionalidad opcache se volvió inútil. Los desarrolladores de APC luego crearon APCu, que ofrece solo la funcionalidad de almacenamiento en caché de objetos (léase "almacenamiento en caché de datos en la memoria") eliminando por completo el opcache obsoleto.
> 
> Para que _php-apcu_ funcione en tu sistema debes indicarlo en el archivo de configuración (_php.ini_). Este archivo se lee cada vez que se inicia PHP. Para las versiones del módulo de servidor de PHP, esto sucede solo una vez cuando se inicia el servidor web. Para las versiones CGI y CLI, sucede en cada invocación.
> 
> Existen al menos 3 archivos con el nombre _php.ini_ que han sido instalados hasta este momento en tu sistema. Uno es utilizado por PHP-CLI, que es una interfaz de línea de comandos para PHP que permite a los usuarios interactuar con PHP a través de la terminal. Otro es para el servidor web, en nuestro caso Apache2. Y otro para el módulo FPM que mejora el consumo de memoria en el sistema. Cada uno de estos archivos _php.ini deben de ser actualizados de forma independiente, ya que sirven diferentes funciones. Los procesos CLI, por ejemplo, generalmente requieren más memoria, mientras que los procesos web deben ser pequeños y livianos. Las líneas de comando y las páginas web tienden a necesitar módulos diferentes también._
> 
> _/etc/php/7.4/cli/php.ini es para el programa CLI PHP. Los cambios en este archivo de configuración solo afectarán a PHP mientras se ejecuta en la terminal; NO afectarán al servidor web. apache_
> 
> _/etc/php/7.4/apache2/php.ini es para el complemento PHP utilizado por Apache. Este es el que necesita editar si estaría_ utilizando el servidor web Apache sin PHP-FPM.
> 
> Nosotros hemos instalado PHP-FPM para que trabaje con Apache2, por lo que necesitamos editar _/etc/php/7.4/fpm/php.ini_.

sudo apt install php7.4-apcu

A continuación queremos editar el archivo _php.ini_ que es utilizado por PHP-FPM para habilitar el módulo _php-apcu_, el cual ayudará a MediWiki gestionar la memoria caché. Este _php.ini_ debería estar en el directorio _/etc/php/7.4/fpm_ por lo que el siguiente comando habrirá el archivo para su posterior edición:

```
sudo nano /etc/php/7.4/fpm/php.ini
```

Habilitar el módulo _php-apcu_ es simple y consiste en des-comentar la directiva _;extension=apcu_. Esto se hace eliminando el punto y coma (;) si existe, o adicionando **extension=apcu** al final del archivo si la directiva no _extension=apcu_ existe de antemano. Luego debes salirte, primero guardandolo con **^x**, **y**, **ENTER**. Es todo.

### Comprobar que nuestro sitio funciona con un simple archivo PHP

Vamos a crear un archivo PHP llamado _info.php_ que colocaremos en el directorio raíz _/var/www/html/ejemplo.com_. Le podemos llamar cualquier nombre, pero deseamos que la extensión sea .php para así comprobar nuestra instalación de PHP al mismo tiempo que comprobamos la configuración de Apache2. Vamos a crear el archivo info.php en el directorio raíz:

```
sudo nano /var/www/html/ejemplo.com/info.php
```

Escribe las siguientes instrucciones dentro del archivo _info.php_:

```
<?php
phpinfo();
?>
```

```
^x  # Usa el teclado. ^ es la tecla CONTROL.      # El editor nano le preguntara si desea 
                                                  # guardar el archivo
```

```
y   # y, por "yes" en inglés, le estas diciendo que sí, que lo guarde.
```

Visita tu sitio en un navegador y navega a http//ejemplo.com/info.php. Deberás ver una página con información acerca de PHP en tu servidor.

Puedes ahora borrar este archivo si no quieres que el público tenga acceso a esta información.

```
sudo rm /var/www/ejemplo.com/info.php
```

## Instalar y configurar HTTPS usando un certificado gratis de ZeroSSL {#anchor-5}

Para habilitar _HTTPS_ en tu página de web, tienes que obtener un certificado (un tipo de archivo) de una Autoridad de Certificación (AC, o CA por sus siglas en inglés). ZeroSSL es una AC.

Existen varias formas de configurar _HTTPS_ en su sitio. Una mención notable es Let´s Encrypt. Let’s Encrypt es una AC que provee certificados gratis. Para obtener un certificado para tu dominio de sitio web de Let’s Encrypt, tienes que demonstrar control sobre ese dominio. Con Let’s Encrypt, puedes hacer esto con software que usa el [protocolo ACME](https://tools.ietf.org/html/rfc8555), como [Certbot](https://certbot.eff.org/), el cual puede instalarse fácilmente en Ubuntu.

La ventaja de utilizar un programa como Certbot para instalar los certificados SSL de Let´s Encrypt, es que todas las configuraciones se realizan automáticamente.

Visita [esta página para más información acerca de Let´s Encrypt](https://letsencrypt.org/es/getting-started/) si deseas configurar SSL de esta manera.

Cualquiera de las dos métodos que uses, Let´s Encrypt o ZeroSSL, requerirá primeramente que accedas y edites los archivos de configuración de tu dominio para verificar que eres el dueño del dominio. **DNS** corresponde a las siglas en inglés de "Domain Name System", es decir, "Sistema de nombres de dominio". Este sistema es básicamente la agenda telefónica de la Web que organiza e identifica dominios. No tiene sentido generar un certificado SSL si aún no has configurado los registros de **DNS** para enrutar a los usuarios finales a tu sitio. **DNS** hace posible que nombres como www.ejemplo.com se enruten a las direcciones IP numéricas como 192.2.2.2 que las computadoras usan para conectarse entre sí.

Esta guía asume que ya usted a creado una cuenta con ZeroSSL, ha llenado el formulario para pedir los certificados, y ZeroSSL se los ha entregado. ZeroSSL le envía un correo electrónico con sus certificados. Si no es así, visite ZeroSSL y obtenga los certificados SSL necesarios antes de continuar.

### Usando los certificados de ZeroSSL 

Verifique los archivos de certificados SSL de ZeroSSL, son tres:

```
certificate.crt
ca_bundle.crt
private.key
```

#### Subir certificados al servidor

Primero, copie los archivos de su certificado en el directorio donde guarda su certificado y los archivos clave. Normalmente, este directorio es **/etc/ssl/** para sus archivos certificate.crt y ca\_bundle.crt, y **/etc/ssl/private/** para su archivo private.key.

Puedes utilizar un programa como **[FileZilla](https://filezilla-project.org/)** con este propósito.

También puedes usar **nano** para crear los certificados y luego pegar el contenido correspondiente en cada uno de estos. Una vez subidos los certificados certificate.crt y ca\_bundle.crt a /etc/ssl/ y private.key a /etc/ssl/private/, reinicie Apache2.

```
systemctl reload apache2
```

#### Configurar HTTPS en Apache2

A continuación deberás regresar a _/etc/apache2/sites-available_ para crear otro archivo de configuración, específicamente para _HTTPS_.

```
cd /etc/apache2/sites-available
```

Una vez adentro, podrás usar el comando **ls** para ver el archivo _ejemplo.com.conf_ que creastes anteriormente.

Ahora vamos a crear nuevamente un archivo de configuración de Apache2 exclusivamente para _HTTPS_. Asegúrate de sustituir _nombredelsitiocompleto_ y _ejemplo.com_ con el nombre del dominio que has adquirido. Si no has adquirido un dominio, asegúrate de sustituir con la dirección IP.

```
sudo nano nombredelsitiocompleto.ssl.conf
```

Por ejemplo:

```
sudo nano ejemplo.com.ssl.conf 
```

Con el archivo abierto, continua con la sección de abajo.

### Configuración del archivo ejemplo.com.ssl.conf

En esta guía no entraremos en mucho detalle acerca de este archivo, solamente le mostraremos a continuación un ejemplo de su contenido. Nuevamente, **asegúrate de sustituir el nombre del sitio, ejemplo.com, con el tuyo, al igual que el nombre y lugar donde Apache2 debe buscar por la raíz del directorio donde se guardaran los documentos públicos de tu sitio.** El símbolo # indica un comentario, por lo que Apache2 lo ignorará cuando ejecute el programa. Usted debe leer estos comentarios dentro del archivo para comprender un poco los lo que está sucediendo a cada paso.

```
# Nótese que este archivo comienza con <IfModule mod_ssl.c>
# Esto quiere decir que el módulo mod_ssl debe estar activado primeramente
# Estaremos seguro de eso en unos próximos pasos
# Note también que el puerto de este VirtualHost o host virtual
# está definido como 443.
# Este es el puerto designado por defecto para las conexiones HTTPS.
<IfModule mod_ssl.c>
	<VirtualHost _default_:443>
		ServerName ejemplo.com
        	ServerAdmin correo@ejemplo.com
        	DocumentRoot /var/www/html/ejemplo.com
        	DirectoryIndex index.php
		<Directory /var/www/html/ejemplo.com>
        	Options Indexes FollowSymLinks MultiViews       
        	AllowOverride All
        	Order allow,deny
        	allow from all
       	 	</Directory>

# Conmutador de motor SSL.
# Habilitar/deshabilitar SSL para este host virtual.

        SSLEngine on

# Esta es la ruta a los directoriorios donde se encuentran
# los certificados

	SSLCertificateFile /etc/ssl/certificate.crt
	SSLCertificateKeyFile /etc/ssl/private/private.key

# La cadena o ruta de certificados del servidor:
# Apunta SSLCertificateChainFile a un archivo que contiene la
# concatenación de certificados de CA codificados con PEM que forman el
# cadena de certificados para el certificado del servidor. Alternativamente
# el archivo al que se hace referencia puede ser el mismo que SSLCertificateFile
# cuando los certificados de CA se adjuntan directamente al servidor
# certificado para mayor comodidad.
        
         SSLCertificateChainFile /etc/ssl/ca_bundle.crt
       
# Opcional: SSLOptions +StdEnvVars controla varias opciones 
# de tiempo de ejecución por directorio. 
# En general, si se aplican varias opciones a un directorio, 
# se aplica la opción más completa (las opciones no se fusionan). 
# Sin embargo, si todas las opciones en una directiva SSLOptions 
# están precedidas por un símbolo más ('+') o menos ('-'), 
# entonces las opciones se fusionan. 
# Las opciones precedidas por un signo más (+)
# se agregan a las opciones actualmente vigentes, 
# y las opciones precedidas por un signo menos (-) 
# se eliminan de las opciones actualmente vigentes.

<Directory /usr/lib/cgi-bin>
	 SSLOptions +StdEnvVars
	</Directory>

</VirtualHost>
```

Guarde el archivo y sálgase del editor de texto **nano** con los siguientes comandos:

```
^x  # Usa el teclado. ^ es la tecla CONTROL. 
    # El editor nano le preguntara si desea guardar el archivo
```

```
y   # y, por "yes" en inglés, le estas diciendo que sí, que lo guarde.
```

Finalmente, presiona **ENTER** o **RETURN** para salirte de **nano**.

Ahora vamos a comprobar que nuestra configuración es correcta y que Apache2 puede entenderla.

```
apachectl configtest
```

El resultado debe ser indicado con el siguiente resultado en la pantalla de su terminal:

```
Syntax OK
```

Ya puedes habilitar tu sitio _HTTPS_ con el siguiente comando:

```
a2ensite ejemplo.com.ssl
```

Puedes comprobar que tu sitio ha sido habilitado primero navegando al directorio sites-enabled y luego verificando su contenido:

```
cd /etc/apache2/sites-enabled
```

```
ls /etc/apache2/sites-enabled
```

Puedes también en estos momentos reiniciar los servicios de FPM y Apache2:

```
sudo systemctl restart php7.4-fpm.service
```

```
systemctl reload apache2
```

## Notas acerca de la seguridad del sistema {#anchor-6}

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
4. Instala y configura Snuffleupagus: Snuffleupagus es un módulo PHP7+ y PHP8+ diseñado para aumentar drásticamente el costo de los ataques contra sitios web. Esto se logra eliminando clases de errores completas y proporcionando un poderoso sistema de parches virtuales, lo que permite al administrador corregir vulnerabilidades específicas sin tener que tocar el código PHP. Para más información, visite [https://snuffleupagus.readthedocs.io/](https://snuffleupagus.readthedocs.io/).
5. Installa y configura Rkhunter: El paquete "rkhunter" es útil para realizar un análisis rápido de su sistema en busca de rootkits conocidos (programas informático maliciosos y clandestinos diseñados para proporcionar acceso privilegiado continuo a una computadora mientras oculta activamente su presencia). Para más información visite [https://help.ubuntu.com/community/RKhunter](https://help.ubuntu.com/community/RKhunter).
