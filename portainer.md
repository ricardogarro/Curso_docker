# 🚀 Instalación de Portainer Community Edition (CE)

## ✅ Requisitos Previos
- Docker instalado y funcionando correctamente.
- Acceso de superusuario o permisos para ejecutar `docker`.

---

## 📦 Crear volumen persistente para Portainer
```bash
docker volume create portainer_data
```

## 🐳 Ejecutar Portainer con Docker

```
docker run -d \
  -p 9000:9000 \
  -p 9443:9443 \
  --name=portainer \
  --restart=always \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -v portainer_data:/data \
  portainer/portainer-ce:latest
```

 ## 🌐 Acceso a la interfaz web

 Abrí tu navegador en:
```
 http://TU_IP_O_DOMINIO:9000

```
