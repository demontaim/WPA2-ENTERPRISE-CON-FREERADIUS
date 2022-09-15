# Autenticación WPA2 Enterprise con FreeRadius

---

### ¿Cómo funciona FreeRadius?

Es un protocolo de autenticación y autorización para aplicaciones de acceso a la red, en nuestro caso red inalámbrica (WiFi). Cuando se realiza la conexión con un punto de acceso WiFi, en lugar de una clave de red típica, se enviará un nombre de usuario y una contraseña. 

### Herramientas utilizadas

* 1 Access Point (UniFi AC-PRO)

<p align="center"><img src="https://cdn.shopify.com/s/files/1/0019/7518/9613/products/UAP-AC-PRO_grande.png?v=1616583501" alt="UniFi AC-PRO" width="150" height="150"></p>

* Software controlador de nuestro AP (UniFi Controller)

<p align="center"><img src="https://assets.stickpng.com/images/586abf59b6fc1117b60b2750.png" alt="Download" width="150" height="150"></p>

* Instancia de PfSense configurada

* Paquete de FreeRadius para PfSense

