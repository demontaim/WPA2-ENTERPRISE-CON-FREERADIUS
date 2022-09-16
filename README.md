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

<p align="center"><img src="img" alt="Freeradius CA"></p>

**System → Cert Manager → Certificates**

<p align="center"><img src="img" alt="Freeradius Certificate"></p>

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

<p align="center"><img src="accounting.png" alt="Account Interface"></p>

Listo por ahora deberían verse dos interfaces en nuestro dashboard tal que así:

<p align="center"><img src="dashboardinterface.png" alt="Interface Dashboard "></p>