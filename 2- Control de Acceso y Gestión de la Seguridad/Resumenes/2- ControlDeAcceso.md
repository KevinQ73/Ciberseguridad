# Sistemas y Metodologías de Control de Acceso

## índice

- [Sistemas y Metodologías de Control de Acceso](#sistemas-y-metodologías-de-control-de-acceso)
  - [índice](#índice)
    - [Introducción a los sistemas y metodologías de control de acceso](#introducción-a-los-sistemas-y-metodologías-de-control-de-acceso)
    - [Identificación, Autentificación y Autorización](#identificación-autentificación-y-autorización)
      - [Otros conceptos](#otros-conceptos)
    - [Tipos o factores de autentificación](#tipos-o-factores-de-autentificación)
      - [Autenticación basada en Secretos](#autenticación-basada-en-secretos)
        - [Hashes](#hashes)
        - [Amenazas](#amenazas)
        - [Contramedidas](#contramedidas)
      - [Autenticación basada en la posesión de elementos](#autenticación-basada-en-la-posesión-de-elementos)
        - [Tokens](#tokens)
        - [Amenazas](#amenazas-1)
        - [Contramedidas](#contramedidas-1)
      - [Autenticación basada en elementos biométricos](#autenticación-basada-en-elementos-biométricos)
        - [Huella Dactilar](#huella-dactilar)
        - [Retina](#retina)
        - [Iris](#iris)
        - [Geometría de la Mano](#geometría-de-la-mano)
        - [Firma](#firma)
        - [Elección de elemento biométrico](#elección-de-elemento-biométrico)
        - [Amenazas](#amenazas-2)
        - [Contramedidas](#contramedidas-2)
      - [Autentificación Fuerte](#autentificación-fuerte)
    - [Gestión de Identidades](#gestión-de-identidades)
      - [Acceso Unificado (Single Sign On)](#acceso-unificado-single-sign-on)
    - [Modelos de Control de Acceso](#modelos-de-control-de-acceso)
    - [Técnicas de Control de Acceso](#técnicas-de-control-de-acceso)

### Introducción a los sistemas y metodologías de control de acceso

- El control de acceso es la habilidad de otorgar el acceso a un sistema o recurso que se desea controlar. Se implementa para asegurar la confidencialidad, integridad y disponibilidad.

- **Requisitos**:
  - Evitar proveer información sensible a sujetos no autorizados. (Confidencialidad)
  - Proveer información sensible a usuarios autorizados. (Disponibilidad)
  - Confiable. (integridad)
  - Escalable. (Duradero)
- **Tipos**:
  - Administrativos.
  - Físicos.
  - Técnicos.
- **Sujeto**: Es una entidad activa que solicita acceso a un objeto.
  - Ejemplos: usuarios, programas, procesos, computadoras, etc.
- **Objeto**: Es una entidad pasiva que contiene información o realiza una función.
  - Ejemplos: archivos, programas, documentos, impresoras, etc.
  - La transferencia de información desde un objeto a un sujeto se llama acceso.

### Identificación, Autentificación y Autorización

- **Identificación**: Es un mecanismo para diferenciar los sujetos.
- Por ejemplo: Nombre de usuario, numero de proceso, etc.

- **Autenticación**: Permite asegurar (con un determinado nivel de certeza) que el sujeto es quien dice ser.

- **Autorización**: Es el mecanismo utilizado para definir si el sujeto tiene o no acceso a determinados objetos.
  - Ejemplo: Listas de control de acceso, Control de acceso mandatorio, etc.

#### Otros conceptos

- **Trazabilidad (Accountability):** Habilidad para determinar las acciones individuales de un usuario dentro de un sistema. Esta soportado por logs de auditoría.

- **No repudio:** El sujeto no puede negar que realizó cierta acción. Por ejemplo: En el envío de mensajes el no repudio del origen el emisor no puede negar que envió el mensaje.

### Tipos o factores de autentificación

#### Autenticación basada en Secretos

- “Algo que uno conoce”
- Ejemplo: contraseñas, pin, etc.

##### Hashes

1. El sujeto configura sus credenciales por primera vez y se calcula y guarda el hash (“H1”) de la
contraseña en el servidor de autenticación.
2. El sujeto informa sus credenciales (ID + Contraseña).
3. El autenticador calcula el hash de la contraseña, obteniendo “H2”.
4. El autenticador busca el hash almacenado correspondiente al ID del usuario, obteniendo “H1”.
5. El autenticador compara “H1” con “H2”. Si son iguales, entonces la autenticación es correcta.

- Si un atacante pudiera robarse el almacenamiento de credenciales, obtendría hashes y no las contraseñas en texto plano (al menos inicialmente).
  - No es necesario y no se sugiere almacenar la contraseña.

##### Amenazas

- Ataques de diccionario / fuerza bruta.
  - Probar opciones de contraseñas hasta dar con la correcta.
- Password Spray.
  - Probar múltiples usuarios con la misma contraseñas débiles o default.
- Análisis de tráfico de red.
  - Buscar credenciales observando los protocolos.
- “Spoofing” del dispositivo autenticado.
  - Engañar al sujeto para que se autentique en un objeto falso.
- Intentar obtener acceso a los mecanismos de autenticación basado en secretos, por ejemplo:
  - Archivo “/etc/shadow” de Linux.
  - SAM (Security Access Manager) de Windows.
  - Base de datos de usuarios, por ejemplo, Directorio IDM

##### Contramedidas

- No utilizar palabras de diccionario.
- No utilizar contraseñas triviales.
- Bloqueo de la cuenta tras ciertos intentos fallidos.
- Implementar retraso luego de ciertos intentos fallidos.
- Implementación de captchas.
- Forzar el uso de contraseñas con números, mayúsculas, minúsculas, establecer longitud mínima, etc.
- Forzar el cambio de contraseña periódico.
- No permitir el uso de credenciales anteriores

#### Autenticación basada en la posesión de elementos

- “Algo que uno tiene”
- Ejemplo: tarjeta, token, llaves, etc.

##### Tokens

- Son dispositivos generadores de contraseñas que un sujeto lleva con él.
- ***Existen 4 tipos de Tokens:***
  - **Tokens estáticos**.
    - Pueden requerir una contraseña.
    - Pueden almacenar una clave, credenciales de logon encriptadas, etc.
    - Son utilizados principalmente como técnica de identificación en lugar de autenticación.
      - Ejemplos:
        - Dispositivo USB.
        - Tarjeta inteligente.
  - **Tokens sincrónicos basados en tiempo.**
    - Los dispositivos y el servidor tienen relojes que miden el tiempo transcurrido desde la inicialización.
    - Cada cierto tiempo la clave generada se muestra en la pantalla del dispositivo. El usuario ingresa su clave en el sistema al cual se quiere autenticar.
    - Como el servidor esta sincronizado con el dispositivo, el la clave generada por  el servidor debe coincidir con la clave generada por el dispositivo para que el usuario sea aceptado.
  - **Tokens sincronicos basados en eventos.**
    - Las claves se generan en el dispositivo debido a la ocurrencia de un evento.
      - Ejemplo: Botón presionado en el dispositivo.
    - El usuario debe ingresar la clave en el sistema al cual se quiere autenticar.
    - El servidor compara la clave con un listado de claves que tiene.
      - Si esta, el usuario se autentica y esa clave se elimina.
      - Si no está, el usuario no se autentica.
  - **Tokens asincronicos basados en desafío respuesta.**
    - El servidor de tokens genera una cadena de dígitos aleatoria (desafío).
    - El sujeto ingresa esa cadena en el dispositivo, la cual le aplica un algoritmo y genera la clave (respuesta).
    - El resultado de esa función es enviado nuevamente al servidor de token, quien realiza la misma operación. Si el resultado es igual, el usuario es autenticado.
  - **Pueden ser dispositivos físicos o “software Tokens”**
    - Hay distintas implementaciones que permiten agregar un segundo factor de autenticación sin una alta inversión de hardware y aumenta la seguridad del sistema.
      - Ejemplo: JWT (Json Web Token).

##### Amenazas

- **Tokens estáticos**.
  - Robo del token físico.
  - Clonado del token.
  - Ataques a la tarjeta de coordenadas mediante phishing:
  - Se le solicita al usuario que complete todos los casilleros de la tarjeta de coordenadas en un sistema que tiene el atacante.
  - Se solicita foto de la tarjeta.

- **Software Tokens**
  - Mala implementación.
  - Falta de firma.
  - Manipulación de los campos que generan el Token

##### Contramedidas

- **Tokens estáticos**.
  - Capacitación.
  - Denuncia del token.
  - Autenticación Fuerte.

- **Software Tokens**
  - Utilización de firma para los tokens.
  - Agregar en el JWT solo los datos necesarios.
  - Validar en backend los datos del JWT

#### Autenticación basada en elementos biométricos

- “Algo que uno es”
  - Ejemplo: huella, voz, etc.
- Los sistemas biométricos se basan en características físicas del sujeto a identificar (Huellas digitales, Reconocimiento retina/iris, geometría de mano) o en patrones de conducta/comportamiento (registro vocal, firma a mano alzada).
- Extracción de ciertas características de la muestra.
- Comparación de ciertas características con las almacenadas en la base de datos.
- Finalmente, la decisión de si el usuario es válido o no

##### Huella Dactilar

- Proceso
  - Toma de una imagen.
  - Procesamiento y guardado en una base de datos.
  - Extracción de puntos característicos.
    - Arcos, bucles, remolinos.
  - Comparación contra la base de datos.
    - Basada en minucias.
    - Basada en patrones.

##### Retina

- Proceso
  - Se mira a través de binoculares a un punto (existen dispositivos de mayor distancia).
  - Se escanea la retina con radiación infrarroja de baja intensidad en forma de espiral.
  - Se detectan nodos y ramas del área retinal.
  - Se comparan con la base de datos.

##### Iris

- Proceso
  - Se captura una imagen del iris en blanco y negro.
  - Se somete a deformaciones pupilares.
  - Se extraen patrones y realizan transformaciones matemáticas.
  - Esa muestra se denominada iriscode.

##### Geometría de la Mano

- Proceso
  - Se sitúa la mano sobre un dispositivo lector con guías que marcan la posición correcta.
  - Se toma una imagen superior y otra lateral, de las que se extraen los datos.
  - Se transforman los datos en un modelo matemático que se contrasta contra una base de patrones.

##### Firma

- Estamos acostumbradas a firmar para identificarnos:
  - Alta aceptación social.
- Existen dos líneas de investigación.
  - Reconocimiento de firma estática (offline).
    - Se parte de firmas realizadas previamente.
    - Se extraen las características extraídas de la firma (geometría en 2D).
- Reconocimiento de firma dinámica (online).
  - La información se adquiere durante el firmado.
    - La información dinámica es más difícil de falsificar.
    - Requiere dispositivos digitalizadores.
    - Se dispone de información temporal (duración, posición, velocidad).

##### Elección de elemento biométrico

- Además de los costos hay ciertos puntos críticos a determinar a la hora de elegir un sistema biométrico como método de control de acceso:
  - Aceptación del usuario.
  - Tiempo de registración (la toma de la muestra inicial).
  - Tiempo de ingreso.
  - Precisión.
  - Facilidad de implementación.
  - Tamaño y manejo de las muestras.

##### Amenazas

- Dispositivos muy sensibles.
  - Error tipo 1: Tasa de falsos rechazos.
- Dispositivos poco sensibles.
  - Error tipo 2: Tasa de falsas aceptaciones.

##### Contramedidas

- Contar con dispositivos bien configurados.
  - El punto CER es usado como estándar para evaluar la performance de los dispositivos biométricos.

![CER Rate](../img/Imagen2.png)

#### Autentificación Fuerte

- Aquellos sistemas que requieren la autenticación de 2 factores diferentes de forma conjunta.
  - Ejemplo: Contraseña + Tarjeta de coordenadas
- Se considera fuerte porque las debilidades de un factor son mitigadas por el segundo factor.

### Gestión de Identidades

- Tecnologías usadas para gestionar la información de un sujeto.
- Centralización de la administración de usuarios/ contraseñas.
- Ejemplos de proveedores:
  - CyberArk, ForgeRock, IBM, Microsoft, Okta, Oracle.
- Ventajas:
  - Sincronización y utilización de contraseñas fuertes.
  - ABM de usuarios automatizada.
  - Workflows de aprobación.
  - Disminución de costos de administración.
- Desventajas:
  - Punto único de entrada.
  - Tiempo de desarrollo para sincronizar las aplicaciones.

#### Acceso Unificado (Single Sign On)

- Logueo único para diferentes sistemas.
  - Ejemplos de protocolos: Kerberos, Oauth, SAML, OpenID, LDAP.
- Ventajas:
  - Facilidad de administración.
  - Uso de contraseñas fuertes.
- Desventajas:
  - Punto único de entrada.
  - Difícil de implantar y operar.
  - Al dejar la PC desbloqueada se puede acceder a cualquier sistema

### Modelos de Control de Acceso

- **Control de Acceso Discrecional (DAC)**
  - Cada objeto tiene un dueño.
  - Normalmente se implementa con Listas de Control de Acceso. Resumen para entender la manipulación de archivos en los SO [aquí](https://github.com/KevinQ73/TPs-SO/blob/main/2do%20Parcial/File%20System.md#manipulaci%C3%B3n-de-archivos)

- **Control de Acceso Obligatorio (MAC)**
  - El sistema impone sus reglas.
  - Se implementa con el uso de etiquetas a recursos y personas
  - Las dividen en categorias
  - Se suele implementar en el ámbito militar, bancario, SE linux o Discord.
  - Si el recurso solicitado por la persona pertenecen a la misma categoria, se le da acceso, sino, nein.

- **Control de Acceso Basado en Roles (RBAC)**
  - Asignación indirecta de permisos.
  - Se basa en una descripción de puesto que ocupa en vez de la identidad del sujeto.
  - Facilita la implementación de Segregación de Funciones.

### Técnicas de Control de Acceso

- Dependiente del Contexto (CBAC)
  - El sistema toma su decisión de acceso basado en el estado de una determinada cantidad de variables que conforman el contexto.
    - Ejemplos: Control de Acceso de Red (NAC), Firewall de Red, Geolocalización.

- Dependiente del Contenido
  - Las decisiones de acceso se basan en la sensibilidad del dato y su contenido.
  - Los cambios en el contenido pueden provocar cambios en las decisiones de acceso. No obstante, las reglas de acceso definidas permanecen constantes.
    - Ejemplos: Firewall a nivel de Aplicación, Filtros Antispam, Antivirus.
