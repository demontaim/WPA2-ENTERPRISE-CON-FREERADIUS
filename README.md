# Autenticación WPA2 Enterprise con FreeRadius

---

### ¿Cómo funciona FreeRadius?

Es un protocolo de autenticación y autorización para aplicaciones de acceso a la red, en nuestro caso red inalámbrica (WiFi). Cuando se realiza la conexión con un punto de acceso WiFi, en lugar de una clave de red típica, se enviará un nombre de usuario y una contraseña. 

### Herramientas utilizadas

* 1 Access Point (UniFi AC-PRO)

<p align="center"><img src="https://cdn.shopify.com/s/files/1/0019/7518/9613/products/UAP-AC-PRO_grande.png?v=1616583501" alt="UniFi AC-PRO" width="150" height="150"></p>

* Software controlador de nuestro AP (UniFi Controller)

<a href="https://www.ui.com/download/unifi/#" class="button">Descargar para Windows (link)</a>

<a href="https://www.ui.com/download/unifi/#" class="button">Descargar para Linux (link)</a>

* Instancia de PfSense configurada

<p align="center"><img src="https://upload.wikimedia.org/wikipedia/commons/thumb/b/b9/PfSense_logo.png/1200px-PfSense_logo.png" alt="PfSense" width="250" height="150"></p>

* Paquete de FreeRadius para PfSense

