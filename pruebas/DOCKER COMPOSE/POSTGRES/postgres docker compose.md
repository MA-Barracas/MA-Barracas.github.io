
### **Visión general de Docker Compose**

Docker Compose es una herramienta que permite definir y ejecutar múltiples contenedores Docker de forma coordinada. En lugar de iniciar manualmente cada contenedor por separado, con Compose puedes describir un entorno completo (con varios servicios) en un archivo YAML. Después, un simple comando (`docker-compose up`) puede iniciar todos esos contenedores con las configuraciones específicas que hayas definido.

Este archivo `docker-compose.yml` tiene tres elementos clave:
1. **Servicios**: Define los contenedores que deseas ejecutar.
2. **Redes**: Gestiona cómo los servicios se comunican entre sí.
3. **Volúmenes**: Administra el almacenamiento persistente que utilizarán los contenedores.

### **Estructura general del archivo**

Tu archivo tiene las siguientes secciones:

1. **version**: Define la versión del formato de Docker Compose que estás usando.
2. **services**: Contiene los contenedores que quieres desplegar (en este caso, `postgres` y `pgadmin`).
3. **networks**: Define la red que los servicios usarán para comunicarse entre sí.
4. **volumes**: Especifica el almacenamiento persistente para los servicios, asegurando que los datos sobrevivan a los reinicios de los contenedores.

Ahora vamos a desglosarlo parte por parte.

---

### **1. Definición de la versión de Docker Compose**

```yaml
version: '3.5'
```

#### ¿Qué significa esto?

Este bloque establece la **versión** del archivo Docker Compose. Docker Compose ha evolucionado a lo largo del tiempo, y cada versión puede admitir diferentes características y opciones de configuración. La versión `3.5` corresponde a una de las versiones más avanzadas de la tercera serie, introduciendo mejoras en la orquestación de contenedores.

#### ¿Por qué se usa aquí?

Esta versión es lo suficientemente moderna para admitir redes, volúmenes y la mayoría de las funcionalidades necesarias para proyectos complejos, como el que tienes aquí con PostgreSQL y PgAdmin. Dependiendo de tu versión de Docker y Docker Compose, es recomendable usar versiones compatibles con las funcionalidades que necesitas.

---

### **2. Servicios: Definición de contenedores**

#### a) Servicio de PostgreSQL

```yaml
services:
  postgres:
    container_name: postgres_container
    image: postgres:12.1
    environment:
      POSTGRES_USER: ${POSTGRES_USER:-postgres}
      POSTGRES_PASSWORD: ${POSTGRES_PASSWORD:-Welcome01}
      PGDATA: /data/postgres
    volumes:
       - postgres:/data/postgres
    ports:
      - "5432:5432"
    networks:
      - postgres
    restart: unless-stopped
```

Aquí estamos definiendo un **servicio** llamado `postgres`. Un "servicio" en Docker Compose representa un contenedor que será gestionado por Compose. Vamos a analizar cada sección:

- **container_name: postgres_container**  
  Esto define el nombre del contenedor que será creado. Si no lo defines, Docker asignará un nombre por defecto. Aquí, estás especificando `postgres_container` para mantener un nombre fácilmente identificable.

- **image: postgres:12.1**  
  El campo `image` define la imagen de Docker que el contenedor va a utilizar. En este caso, estás usando la imagen oficial de PostgreSQL en su versión `12.1`. Docker Compose descargará esta imagen desde Docker Hub (si no está ya en la máquina) y la utilizará para crear el contenedor. PostgreSQL es un sistema de gestión de bases de datos relacional, y la imagen incluye todo lo necesario para que un contenedor funcione como una base de datos PostgreSQL.

- **environment**  
  Las variables de entorno permiten configurar el comportamiento del contenedor sin necesidad de modificar la imagen subyacente. Aquí defines varias variables clave para PostgreSQL:
  - **POSTGRES_USER**: El nombre de usuario que se utilizará para acceder a la base de datos. El valor puede ser establecido desde una variable de entorno externa o, si no se proporciona, el valor predeterminado será `postgres`.
  - **POSTGRES_PASSWORD**: La contraseña para el usuario definido anteriormente. En este caso, has establecido la contraseña por defecto como `Welcome01`. Es esencial para acceder a la base de datos de manera segura.
  - **PGDATA**: Define el directorio donde PostgreSQL almacenará sus datos. Esta es una opción importante porque permite que los datos de la base de datos se almacenen de manera persistente fuera del contenedor.

- **volumes**  
  Los volúmenes son fundamentales para el almacenamiento persistente. El contenedor puede ser destruido o reiniciado, pero los datos en el volumen persisten.
  - **postgres:/data/postgres**: Este volumen asegura que los datos de la base de datos PostgreSQL se almacenen en `/data/postgres` dentro del contenedor, pero el almacenamiento real está en un volumen gestionado por Docker llamado `postgres`. Este es un almacenamiento externo al contenedor que permite la persistencia de los datos.

- **ports**  
  Define cómo los puertos del contenedor se exponen a la máquina host (donde Docker está ejecutándose).
  - **"5432:5432"**: El contenedor expone el puerto `5432`, que es el puerto estándar para PostgreSQL. Esto significa que cualquier conexión a PostgreSQL en la máquina host puede hacerse a través del puerto `5432`, y será dirigida al puerto `5432` del contenedor.

- **networks**  
  Aquí defines que el servicio `postgres` estará conectado a la red llamada `postgres`. Las redes en Docker Compose permiten que los contenedores se comuniquen entre sí de manera privada y segura sin necesidad de exponer sus puertos fuera del host.

- **restart: unless-stopped**  
  Esta política de reinicio asegura que si el contenedor falla, Docker intentará reiniciarlo automáticamente, a menos que haya sido explícitamente detenido. Es útil para garantizar la alta disponibilidad de servicios críticos como bases de datos.

---

#### b) Servicio de PgAdmin

```yaml
  pgadmin:
    container_name: pgadmin_container
    image: dpage/pgadmin4:4.16
    environment:
      PGADMIN_DEFAULT_EMAIL: ${PGADMIN_DEFAULT_EMAIL:-pgadmin4@pgadmin.org}
      PGADMIN_DEFAULT_PASSWORD: ${PGADMIN_DEFAULT_PASSWORD:-admin}
    volumes:
       - pgadmin:/root/.pgadmin
    ports:
      - "${PGADMIN_PORT:-5050}:80"
    networks:
      - postgres
    restart: unless-stopped
```

Este segundo servicio es para **PgAdmin**, que es una herramienta web para gestionar bases de datos PostgreSQL.

- **container_name: pgadmin_container**  
  Asigna un nombre al contenedor, `pgadmin_container`, que facilita su identificación.

- **image: dpage/pgadmin4:4.16**  
  Estás utilizando la imagen de Docker `dpage/pgadmin4`, en su versión `4.16`. Esta imagen contiene una versión preconfigurada de PgAdmin, que te permite conectarte y administrar visualmente tus bases de datos PostgreSQL a través de un navegador web.

- **environment**  
  Al igual que con PostgreSQL, defines algunas variables de entorno que configuran el comportamiento del contenedor:
  - **PGADMIN_DEFAULT_EMAIL**: El correo electrónico por defecto para el usuario administrador de PgAdmin. Si no se proporciona otro valor, se utilizará `pgadmin4@pgadmin.org`.
  - **PGADMIN_DEFAULT_PASSWORD**: La contraseña por defecto para el usuario administrador. Aquí has establecido `admin` como valor predeterminado.

- **volumes**  
  Al igual que con PostgreSQL, aquí defines un volumen que asegura que las configuraciones y sesiones de PgAdmin persistan.
  - **pgadmin:/root/.pgadmin**: Aquí se almacena la configuración de PgAdmin en un volumen gestionado por Docker, bajo el directorio `/root/.pgadmin`.

- **ports**  
  PgAdmin es una herramienta basada en web, por lo que necesita exponer un puerto para acceder desde un navegador.
  - **"${PGADMIN_PORT:-5050}:80"**: Estás mapeando el puerto `5050` en el host al puerto `80` en el contenedor. El puerto `80` es el puerto por defecto para servidores web, y `5050` es el puerto en el que accederás a PgAdmin en tu máquina host.

- **networks**  
  Este contenedor también está conectado a la red `postgres`, lo que le permite comunicarse directamente con el servicio de PostgreSQL. Esto es importante para que PgAdmin pueda gestionar la base de datos PostgreSQL sin exponer su puerto públicamente.

- **restart: unless-stopped**  
  Al igual que el servicio de PostgreSQL, PgAdmin también se reiniciará automáticamente si falla.

---

### **3. Redes: Comunicación entre servicios**

```yaml
networks:
  postgres:
    driver: bridge
```

Las redes en Docker Compose permiten que los contenedores se comuniquen de manera aislada y segura. Aquí defines una red llamada `postgres`, que utiliza el **driver `bridge`**, que es el driver por defecto en Docker. Este tipo de red permite que los contenedores conectados a ella se comuniquen entre sí, mientras que están aislados del resto de los contenedores en el sistema.

En este caso, tanto `postgres`

 como `pgadmin` están en la misma red, lo que permite que PgAdmin se conecte directamente a PostgreSQL sin exponer el puerto `5432` a otros servicios fuera de esta red.

---

### **4. Volúmenes: Persistencia de datos**

```yaml
volumes:
  postgres:
  pgadmin:
```

Aquí defines dos volúmenes: `postgres` y `pgadmin`. Estos volúmenes proporcionan **almacenamiento persistente**, lo que significa que aunque los contenedores se detengan o reinicien, los datos almacenados en estos volúmenes se conservarán. Esto es especialmente importante para bases de datos (en el caso de `postgres`), ya que quieres asegurarte de que los datos no se pierdan cuando el contenedor de PostgreSQL se detiene o reinicia. Lo mismo ocurre con PgAdmin, donde se almacena la configuración y las preferencias de usuario.

---

### **Conclusión**

Este archivo Docker Compose está diseñado para configurar y ejecutar una base de datos PostgreSQL y una instancia de PgAdmin que permita gestionar la base de datos a través de una interfaz gráfica web. 

- **PostgreSQL** almacena los datos de manera persistente utilizando volúmenes y expone su puerto de manera segura.
- **PgAdmin** se comunica con PostgreSQL a través de una red interna, permitiendo la administración de la base de datos sin necesidad de exponer puertos adicionales.
- **Docker Compose** facilita la gestión de ambos servicios en conjunto, simplificando el proceso de levantarlos, gestionarlos y asegurando que estén configurados correctamente con volúmenes persistentes y redes internas. 

Docker Compose, en este caso, te permite manejar dos servicios que dependen entre sí (una base de datos y una interfaz de administración) de una manera coordinada, asegurando la persistencia de los datos y una comunicación efectiva entre ellos.