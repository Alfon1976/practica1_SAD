# Práctica 1 de SAD · SecureCorp — Respuestas

**Nombre y apellidos:** Alfonso Roman 
**Usuario:** alfon

Responde con tus palabras, en 1-3 líneas. En la defensa te preguntaré lo mismo en voz alta.

**Contraseñas que has usado** (solo porque es un laboratorio; en una empresa, jamás en un fichero):

- Tu usuario: SecureCorp2026
- mtorres: SecureCorp2026


---

**1. (A1)** ¿Quién es el `issuer` de tu `ca.crt`? ¿Hasta qué fecha es válido? ¿Por qué el `subject`
y el `issuer` de la CA son iguales y los de `ldap.crt` no?

Issuer y Validez: El emisor es la propia CA (autofirmado) y es válido desde el 07/10/2026 hasta el 04/10/2036.

Subject e Issuer: Son iguales en la CA porque se firma a sí misma, pero distintos en ldap.crt porque la CA emite el certificado para el servidor.

**2. (A3)** Pega el comando y el resultado de tus dos búsquedas:

```
a) miembros de rrhh: 

ldapsearch -x -H ldap://localhost -b "dc=securecorp,dc=local" "(&(objectClass=person)(memberOf=cn=rrhh,ou=groups,dc=securecorp,dc=local))"

b) cn y mail de todas las personas: 

ldapsearch -x -H ldap://localhost -b "dc=securecorp,dc=local" "(objectClass=person)" cn mail

```

**3. (A4)** ¿Por qué la clave `ldap.key` tiene que ser de `openldap` y tener permisos 600?

Debe ser de openldap con permisos 600 para que solo el demonio pueda leer la clave privada y se eviten accesos no autorizados en el sistema.

Indica los protocolos y puertos (ldap://, ldaps://) que el servidor va a escuchar para aceptar conexiones tanto claras como cifradas.

**4. (A4)** ¿Qué valor has puesto en `SLAPD_SERVICES` y por qué?

Porque sin indicarle dónde está el certificado de la CA, el cliente no tiene forma de verificar ni confiar en el certificado del servidor.


**5. (A4)** Antes de añadir `TLS_CACERT` en el cliente, `ldaps://` no funcionaba. ¿Por qué?

No funcionaba porque el cliente desconoce y no confía por defecto en la autoridad certificadora (ca.crt) que firmó el certificado del servidor, necesitando esa directiva para validar su autenticidad.


**6. (B3)** Pega la salida de `klist` con tus dos tickets. ¿Para qué sirve cada uno? ¿Ha viajado tu
contraseña por la red?

Ticket cache: FILE:/tmp/krb5cc_0
Default principal: alfon@SECURECORP.LOCAL

Valid starting     Expires            Service principal
10/09/26 14:23:53  10/10/26 00:23:53  krbtgt/SECURECORP.LOCAL@SECURECORP.LOCAL
renew until 10/16/26 14:23:53


Uso: El TGT sirve para autenticarse en el dominio y pedir otros tickets (TGS). La contraseña nunca viaja por la red, solo se usa localmente para validar la sesión.


**7. (C)** En el `docker-compose.yml`, ¿qué diferencia hay entre `build:` e `image:`? ¿Qué
significa la línea `- "8081:80"` del servicio `phpldapadmin`?


build vs image: build compila la imagen localmente desde un Dockerfile y image descarga una ya hecha de un repositorio.

- "8081:80": Conecta el puerto 80 del contenedor web con el puerto 8081 del host para poder entrar desde el navegador.


**8. (C)** ¿Por qué en la máquina `web` no has tenido que escribir a mano `TLS_CACERT`, y en el
cliente sí? ¿Qué pasaría con esa línea del cliente si hicieras `./lab.sh reset`


Diferencia con el cliente: En la máquina web ya viene automatizada en su despliegue, mientras que en el cliente hay que configurarla a mano.

Efecto de ./lab.sh reset: Borraría los cambios manuales del cliente y dejaría el entorno como al principio.