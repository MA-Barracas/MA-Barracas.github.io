# 🌟 **Docker Cheatsheet Total** 🌟

¡La guía definitiva para dominar Docker en un solo lugar!

---

## 📦 **Instalación y Configuración**

### **Instalar Docker**
```bash
# En Ubuntu
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io

# En macOS (con Homebrew)
brew install --cask docker
```

### **Comprobar la Instalación**
```bash
docker --version
```

---

## 🚀 **Comandos Básicos de Docker**

### **Imágenes**
```bash
# Buscar una imagen en Docker Hub
docker search <nombre_imagen>

# Descargar una imagen
docker pull <nombre_imagen>

# Listar imágenes locales
docker images

# Eliminar una imagen
docker rmi <nombre_imagen> [--force]
```

### **Contenedores**
```bash
# Crear y ejecutar un contenedor
docker run -it --name <nombre_contenedor> <nombre_imagen>

# Listar contenedores activos
docker ps

# Listar todos los contenedores
docker ps -a

# Detener un contenedor
docker stop <nombre_contenedor>

# Eliminar un contenedor
docker rm <nombre_contenedor> [--force]
```

---

## 🛠️ **Gestión Avanzada**

### **Volúmenes y Redes**
```bash
# Crear un volumen
docker volume create <nombre_volumen>

# Listar volúmenes
docker volume ls

# Conectar un contenedor a una red
docker network connect <nombre_red> <nombre_contenedor>

# Crear una red personalizada
docker network create <nombre_red>
```

### **Dockerfile**
```Dockerfile
# Ejemplo de Dockerfile básico
FROM ubuntu:20.04
RUN apt-get update && apt-get install -y curl
COPY . /app
WORKDIR /app
CMD ["bash"]
```

```bash
# Construir una imagen desde un Dockerfile
docker build -t <nombre_imagen> .
```

---

## 📊 **Monitoreo y Logs**

### **Logs de Contenedores**
```bash
# Ver los logs de un contenedor en tiempo real
docker logs -f <nombre_contenedor>
```

### **Estadísticas en Vivo**
```bash
# Ver estadísticas de recursos
docker stats
```

---

## 🏗️ **Docker Compose**

### **Archivo `docker-compose.yml` Básico**
```yaml
version: '3'
services:
  web:
    image: nginx
    ports:
      - "8080:80"
  db:
    image: mysql:5.7
    environment:
      MYSQL_ROOT_PASSWORD: ejemplo
```

### **Comandos de Docker Compose**
```bash
# Levantar los servicios definidos en docker-compose.yml
docker-compose up -d

# Detener los servicios
docker-compose down

# Verificar los servicios en ejecución
docker-compose ps
```

---

## 🛡️ **Buenas Prácticas**

- **Mantén las imágenes pequeñas:** Usa imágenes base ligeras como `alpine`.
- **Evita instalar paquetes innecesarios:** Minimiza el tamaño de la imagen.
- **Utiliza `.dockerignore`:** Excluye archivos no esenciales del contexto de construcción.
- **Etiquetas y versiones:** Etiqueta tus imágenes con versiones claras y descriptivas.

---

## 🎓 **Recursos Adicionales**

- **[Documentación Oficial de Docker](https://docs.docker.com)**
- **[Docker Hub](https://hub.docker.com)**
- **[Play with Docker](https://labs.play-with-docker.com):** Practica con Docker en tu navegador.

---

### **🚢 Docker es el ancla que asegura tus aplicaciones en cualquier puerto. ¡Domínalo! 🚢**

