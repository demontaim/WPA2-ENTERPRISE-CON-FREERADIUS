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

### Instalación del paquete “FreeRadius” <a name="freeradius-install"></a>

Lo primero que debemos realizar es instalar el paquete necesario para realizar la
configuración.

Lo encontraremos en la siguiente ruta una vez nos situemos en el panel de control de
PfSense:

**System →Package Manager**

<p align="center"><img src="https://github.com/demontaim/WPA2-ENTERPRISE-CON-FREERADIUS/blob/main/img/packagemanager.png?raw=true" alt="Package Manager"></p>

Una vez nos situamos en el manejador de paquetes de PfSense debemos buscar en
**“Available Packages”** la instancia necesaria de **FreeRadius**.

<p align="center"><img src="https://github.com/demontaim/WPA2-ENTERPRISE-CON-FREERADIUS/blob/main/img/packagemanager.png?raw=true" alt="Freeradius Package"></p>

<span style="color:red";font-weight:700;font-size:20px">"En mi caso no aparece puesto que ya lo tenemos instalado.”</span>