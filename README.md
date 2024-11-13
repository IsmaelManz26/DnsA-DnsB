# Proyecto de Configuración de Servidores DNS con Vagrant

Este proyecto tiene como objetivo la configuración de un servidor DNS maestro y un servidor DNS esclavo utilizando Vagrant en una red privada. El servidor DNS maestro (DNS-A) gestionará las zonas de dominio, mientras que el servidor DNS esclavo (DNS-B) replicará las zonas del maestro.

## Solución

### Servidores DNS

El proyecto está compuesto por dos máquinas virtuales configuradas con Vagrant:

- **DNS-A**: Servidor DNS Maestro, encargado de gestionar las zonas de dominio.
- **DNS-B**: Servidor DNS Esclavo, encargado de replicar las zonas del servidor maestro.

### Archivos Configurados

- **Vagrantfile**: Configura las máquinas virtuales de Vagrant para la red interna 192.168.57.0/24. Se define la creación de las dos máquinas virtuales y la configuración de la red.
- **named.conf.local (DNS-A)**: Este archivo contiene las configuraciones de las zonas de dominio gestionadas por el servidor DNS-A. Las zonas configuradas incluyen:
  - `ies.test` - Zona directa para el dominio principal.
  - `57.168.192.in-addr.arpa` - Zona inversa para la red 192.168.57.0/24.
  - `aulas.ies.test`, `informatica.ies.test`, `departamentos.ies.test` - Subdominios específicos.
- **named.conf.local (DNS-B)**: Configura el servidor DNS-B como esclavo de las zonas gestionadas por el servidor maestro (DNS-A). El archivo especifica que el servidor DNS-B obtiene las zonas `ies.test`, `informatica.ies.test`, `aulas.ies.test`, y `57.168.192.in-addr.arpa` desde el maestro.

- **Archivos de zona**: Los archivos de zona como `db.ies.test`, `db.192.168.57`, `db.aulas`, `db.informatica`, `db.departamentos` están configurados en el servidor DNS-A y son replicados por el servidor DNS-B.

- **Configuración del servicio BIND**: El servicio BIND está configurado para registrar los archivos de log y asegurar que las consultas se manejen adecuadamente.

### Pruebas

Las pruebas de funcionamiento incluyen consultas DNS realizadas desde el sistema anfitrión para verificar que tanto el servidor maestro como el servidor esclavo responden correctamente a las consultas de los dominios configurados.

## Conclusión

La solución permite la configuración de un entorno DNS redundante, con un servidor maestro gestionando las zonas de dominio y un servidor esclavo replicando las configuraciones, lo que garantiza la alta disponibilidad y fiabilidad del servicio DNS. Esta configuración es ideal para entornos empresariales o educativos que requieren una infraestructura de DNS robusta y escalable.

## Responder a las preguntas:
¿Para qué me puede servir esta utilidad? Los logs de BIND son útiles para:

Auditoría y solución de problemas: 
Puedes rastrear qué consultas de DNS se están haciendo, si algún cliente está teniendo problemas para resolver nombres o si hay patrones sospechosos.
Seguridad: Permiten detectar consultas maliciosas, como intentos de acceso a dominios no autorizados.
Rendimiento: Ayuda a evaluar el rendimiento y ver si hay picos en el tráfico DNS.

¿Es consistente con la protección de datos? 
En términos de protección de datos, es importante tener en cuenta que los logs de DNS pueden contener información sobre las consultas realizadas por los usuarios. Si estas consultas contienen información sensible o personal (por ejemplo, si los usuarios están accediendo a servicios internos o sitios con información sensible), se debe considerar si es adecuado almacenar esa información. Para cumplir con normativas de privacidad (como el GDPR), se puede implementar un sistema de retención adecuado y anonimizar los datos cuando sea necesario.
