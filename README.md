# Autenticación WPA2 Enterprise con FreeRadius

---

## ¿Cómo funciona FreeRadius?

Es un protocolo de autenticación y autorización para aplicaciones de acceso a la red, en nuestro caso red inalámbrica (WiFi). Cuando se realiza la conexión con un punto de acceso WiFi, en lugar de una clave de red típica, se enviará un nombre de usuario y una contraseña. 

---

## Herramientas utilizadas

* 1 Access Point (UniFi AC-PRO)

<p align="center"><img src="https://cdn.shopify.com/s/files/1/0019/7518/9613/products/UAP-AC-PRO_grande.png?v=1616583501" alt="UniFi AC-PRO" width="150" height="150"></p>

* Software controlador de nuestro AP (UniFi Controller)

<a href="https://www.ui.com/download/unifi/#" class="button">Descargar para Windows (link)</a>

<a href="https://www.ui.com/download/unifi/#" class="button">Descargar para Linux (link)</a>

* Instancia de PfSense configurada

<p align="center"><img src="https://upload.wikimedia.org/wikipedia/commons/thumb/b/b9/PfSense_logo.png/1200px-PfSense_logo.png" alt="PfSense" width="250" height="70"></p>

* Paquete de FreeRadius para PfSense

<p align="center"><img src="https://upload.wikimedia.org/wikipedia/commons/thumb/5/5b/Freeradius_logo.svg/2560px-Freeradius_logo.svg.png" alt="FreeRadius" width="250" height="70"></p>

---

## Guía paso a paso

1. [Instalación del paquete “FreeRadius”](#freeradius-install)
2. [Verificamos que se nos ha creado una CA](#freeradius-ca)
3. [Configuración de las interfaces de nuestro servidor FreeRadius](#freeradius-interfaces)
4. [Damos de alta todos los AP’s](#freeradius-ap)
5. [Añadir usuarios a nuestro servidor FreeRadius](#freeradius-users)
6. [Configuración de nuestro AP](#freeradius-ap-config)
7. [Configurar la autenticación WPA2 / Enterprise](#freeradius-wpa2)
8. [(TEST) Prueba de autenticación](#freeradius-test)

---

### 1.Instalación del paquete “FreeRadius” <a name="freeradius-install"></a>

Lo primero que debemos realizar es instalar el paquete necesario para realizar la
configuración.

Lo encontraremos en la siguiente ruta una vez nos situemos en el panel de control de
PfSense:

**System →Package Manager**

<p align="center"><img src="https://github.com/demontaim/WPA2-ENTERPRISE-CON-FREERADIUS/blob/main/img/packagemanager.png?raw=true" alt="Package Manager"></p>

Una vez nos situamos en el manejador de paquetes de PfSense debemos buscar en
**“Available Packages”** la instancia necesaria de **FreeRadius**.

<p align="center"><img src="https://github.com/demontaim/WPA2-ENTERPRISE-CON-FREERADIUS/blob/main/img/packagemanager.png?raw=true" alt="Freeradius Package"></p>

<p align="left"><img src="https://www.svgrepo.com/show/178970/eye-medical.svg" alt="ATTENTION PLEASE" width="30px" height="30px"><em>Cabe destacar que a mí no me aparece ya que lo tengo ya instalado.</em></p>

### 2.Verificamos que se nos ha creado una CA <a name="freeradius-ca"></a>

Una vez hemos instalado nuestro paquete **FreeRadius** podemos ver como se nos ha
creado automáticamente una autoridad gestora de certificados con el nombre de
**FreeRadius CA** y un certificado para la autenticación llamado **FreeRadius Server**
**Certificate**.

Lo podemos comprobar si nos situamos en la siguiente ruta:

**System → Cert Manager → CA’s**

<p align="center"><img src="https://github.com/demontaim/WPA2-ENTERPRISE-CON-FREERADIUS/blob/main/img/freeradius_ca.jpg" alt="Freeradius CA"></p>

**System → Cert Manager → Certificates**

<p align="center"><img src="https://github.com/demontaim/WPA2-ENTERPRISE-CON-FREERADIUS/blob/main/img/freeradius_cert.png" alt="Freeradius Certificate"></p>

### 3.Configuración de las interfaces de nuestro servidor FreeRadius <a name="freeradius-interfaces"></a>

Una vez realizada la instalación podemos continuar con la configuración de nuestras
interfaces por la que un usuario al acceder al Wifi va a poder autenticarse con nuestro
servidor.

Para configurar nuestro servidor Radius debemos ir a **Services → FreeRadius →**
**Interfaces**

Crearemos en nuestro caso una para la **“Autenticación”**:

Añadiremos una nueva interfaz con los siguientes parámetros:

<p align="center"><img src="https://github.com/demontaim/WPA2-ENTERPRISE-CON-FREERADIUS/blob/main/img/auth_interface.png" alt="Auth Interface"></p>

Añadiremos una segunda interfaz de tipo **“Accounting”** con los siguientes parámetros:

<p align="center"><img src="https://github.com/demontaim/WPA2-ENTERPRISE-CON-FREERADIUS/blob/main/img/accounting.png?raw=true" alt="Account Interface"></p>

Listo por ahora deberían verse dos interfaces en nuestro dashboard tal que así:

<p align="center"><img src="https://github.com/demontaim/WPA2-ENTERPRISE-CON-FREERADIUS/blob/main/img/dashboard_interface.png?raw=true" alt="Interface Dashboard "></p>

### 4.Damos de alta todos los AP’s <a name="freeradius-ap"></a>

Para dar de alta nuestros puntos de acceso debemos ir a la siguiente ruta:

**Services → FreeRadius → Nas / Clients**

Cabe destacar que si tenemos algún controlador en nuestra red por ejemplo un Ruckus
también debemos incluirlo.

Añadiremos uno configurando los siguientes parámetros:

<p align="center"><img src="https://github.com/demontaim/WPA2-ENTERPRISE-CON-FREERADIUS/blob/main/img/apconfig.png" alt="AP Config"></p>

**Client IP:** Dirección IP del AP.

**Client IP Version**: Versión de la IP del AP.

**Client Shortname**: Nombre corto del AP que nosotros le ponemos para identificarla.

**Client Shared Secret**: Clave compartida que debemos configurar en el AP más adelante y que usarán nuestro servidor Radius y nuestro AP para autenticarse.

Despues de añadir los distintos AP's que se encuentran en nuestra red pasaríamos a la creación de usuarios en nuestro servidor.

### 5.Añadir usuarios a nuestro servidor FreeRadius <a name="freeradius-users"></a>

Para añadir usuarios a nuestro servidor debemos ir a la siguiente ruta:

**Services → FreeRadius → Users**

Una vez allí solo tenemos que añadir nuevos usuarios usando los siguientes parámetros por
defecto:

<p align="center"><img src="https://github.com/demontaim/WPA2-ENTERPRISE-CON-FREERADIUS/blob/main/img/users_config.png?raw=true" alt="Users Config"></p>

Una vez configurado nuestro servidor Radius debemos ir a cada punto de acceso para
configurar la autenticación **WPA2 / Enterprise y dar de alta nuestro servidor Radius**.

### 6.Configuración de los AP’s <a name="freeradius-ap-config"></a>

Tenemos que configurar cada Access Point uno a uno. En nuestro caso se realizaron pruebas con Ruckus pero me resultó bastante lioso y tuve
varios errores ya que no conocía el software de Ruckus.

Luego probé con el UniFi AC-PRO y la configuración fue bastante sencilla la verdad.

En este tutorial vamos a realizar la configuración con el UniFi AC-PRO.

Para configurar inicialmente este dispositivo vamos a tener que conectarlo a un cable de red POE y conectarnos también con un cable ethernet.

Si conectamos nuestro AP a la red directamente este obtendrá una dirección por DHCP.

Debemos tener instalada el controlador de UniFi, el cual voy a dejar los links de las
versiones de Windows y Ubuntu para que no tengáis que buscarlos arriba:

<a href="https://www.ui.com/download/unifi/#" class="button">Descargar para Windows (link)</a>

<a href="https://www.ui.com/download/unifi/#" class="button">Descargar para Linux (link)</a>

Una vez instalada la aplicación y estando conectados al Access Point abriremos la
aplicación.

<p align="center"><img src="https://github.com/demontaim/WPA2-ENTERPRISE-CON-FREERADIUS/blob/main/img/unifi_software.png" alt="UniFi Software"></p>

Se nos iniciará una interfaz web dónde podemos configurar el punto de acceso.

<p align="center"><img src="https://github.com/demontaim/WPA2-ENTERPRISE-CON-FREERADIUS/blob/main/img/unifi_dashboard.png" alt="UniFi Login"></p>

Se nos abrirá una pestaña en la que tendremos que autenticarnos con nuestra cuenta de
UniFi. Si no posees una puedes registrarte accediendo a través de este [link](https://account.ui.com/register).