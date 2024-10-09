**Objetivo:**
Mostrar cómo un servicio escucha en un puerto específico y cómo podemos interactuar con él a través de nuestro navegador web.

**Pasos a Seguir:**

---

### **1. Instalar Python (si aún no está instalado):**

- **Descarga Python:**
  - Ve al sitio oficial de Python: [https://www.python.org/downloads/windows/](https://www.python.org/downloads/windows/)
  - Descarga la última versión de Python para Windows.

- **Instalación:**
  - Ejecuta el instalador descargado.
  - **Importante:** Marca la opción "Add Python to PATH" (Agregar Python al PATH) durante la instalación para poder usar Python desde la línea de comandos.
  - Completa la instalación siguiendo las instrucciones en pantalla.

### **2. Abrir la Línea de Comandos:**

- Presiona `Win + R`, escribe `cmd` y presiona Enter para abrir la línea de comandos.

### **3. Navegar a un Directorio Específico (Opcional):**

- Puedes crear una carpeta en tu escritorio llamada `servidor` y navegar hasta ella con el siguiente comando:

  ```cmd
  cd %USERPROFILE%\Desktop\servidor
  ```

  - Si la carpeta no existe, créala primero:

    ```cmd
    mkdir %USERPROFILE%\Desktop\servidor
    cd %USERPROFILE%\Desktop\servidor
    ```

### **4. Iniciar un Servidor Web Simple:**

- En la línea de comandos, ejecuta:

  ```cmd
  python -m http.server 8000
  ```

  - Este comando inicia un servidor HTTP simple que escucha en el puerto `8000`.

### **5. Explicar lo que Está Sucediendo:**

- **Servicio:** El servidor web que acabas de iniciar es un **servicio** que está corriendo en tu máquina local.
- **Puerto:** El servicio está escuchando en el **puerto** `8000`. Los puertos actúan como canales de comunicación específicos para diferentes servicios en tu computadora.

### **6. Acceder al Servicio desde el Navegador Web:**

- Abre tu navegador web preferido (Chrome, Firefox, Edge, etc.).
- En la barra de direcciones, escribe:

  ```
  http://localhost:8000
  ```

  o

  ```
  http://127.0.0.1:8000
  ```

- Deberías ver una página que muestra el contenido del directorio `servidor`. Si no hay archivos, aparecerá vacío.

### **7. Interactuar con el Servicio:**

- **Añadir Archivos:**
  - Coloca algunos archivos (pueden ser documentos de texto o imágenes) dentro de la carpeta `servidor`.
  - Refresca la página en el navegador y observa cómo aparecen los nuevos archivos listados.

- **Explicación:**
  - Esto demuestra cómo el navegador (cliente) se comunica con el servidor (servicio) a través del puerto `8000`.

### **8. Visualizar los Puertos en Uso:**

- Abre una **nueva** ventana de línea de comandos para no interrumpir el servidor.
- Ejecuta el siguiente comando para ver los puertos en uso:

  ```cmd
  netstat -a -n -o | findstr :8000
  ```

  - **Explicación:**
    - `netstat` muestra las conexiones de red y los puertos en uso.
    - Este comando filtra para mostrar solo las líneas que contienen `:8000`, es decir, el puerto que estamos usando.
    - Puedes ver el estado `LISTENING`, indicando que el puerto está abierto y esperando conexiones.

### **9. Detener el Servicio:**

- Para detener el servidor web, vuelve a la ventana de línea de comandos donde se está ejecutando y presiona `Ctrl + C`.

### **10. Conectar con Docker:**

- **Explicación:**
  - En Docker, los contenedores pueden ejecutar servicios que necesitan exponer puertos para comunicarse con otros contenedores o con el host.
  - Al entender cómo un servicio local escucha en un puerto y cómo interactuamos con él, los alumnos podrán comprender por qué es necesario mapear puertos al trabajar con Docker.

---

**Conceptos Clave para Compartir con tus Alumnos:**

- **Servicio:**
  - Un programa o proceso que corre en segundo plano y ofrece alguna funcionalidad, como servir páginas web, gestionar bases de datos, etc.
  - En nuestro ejemplo, el servidor HTTP simple es el servicio.

- **Puerto:**
  - Un número que identifica un canal de comunicación específico en una máquina.
  - Los puertos permiten que múltiples servicios operen simultáneamente sin interferir entre sí.
  - El puerto `8000` es donde nuestro servicio está escuchando las solicitudes.

- **Localhost / 127.0.0.1:**
  - Se refieren a la dirección de loopback de la propia máquina.
  - Usamos esta dirección para acceder a servicios que corren localmente.

- **Netstat:**
  - Una herramienta de línea de comandos que muestra conexiones de red activas y puertos en uso.
  - Útil para diagnosticar problemas de red y entender qué servicios están corriendo.

---

**Beneficios de este Ejemplo:**

- **Práctico y Visual:**
  - Los alumnos ven en tiempo real cómo se establece la comunicación entre el cliente y el servicio.
- **Sencillo:**
  - No requiere instalaciones complejas ni conocimientos avanzados de la terminal.
- **Relevante para Docker:**
  - Sienta las bases para entender cómo se manejan los puertos y servicios dentro de contenedores.

---

**Consejos Adicionales:**

- **Explorar Puertos Comunes:**
  - Menciona que ciertos servicios utilizan puertos estándar (HTTP usa el puerto 80, HTTPS el 443, etc.).
  - Puedes cambiar el puerto en el comando del servidor para mostrar cómo afecta a la forma de acceder al servicio.

- **Seguridad y Puertos:**
  - Introduce brevemente la idea de que abrir puertos puede tener implicaciones de seguridad.
  - En entornos de producción, es importante gestionar qué puertos están expuestos.

