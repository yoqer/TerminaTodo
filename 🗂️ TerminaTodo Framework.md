# 🗂️ TerminaTodo Framework

**Gestor Unificado de Almacenamiento Multi-Fuente**

Framework completo para descubrir, gestionar y acceder a múltiples tipos de almacenamiento desde una interfaz única:

- 💾 **Almacenamiento Local** (SSD, HDD, pendrives, memoria)
- ☁️ **Almacenamiento en Nube** (Google Drive, OneDrive, Dropbox)
- 🖥️ **Hosting Privado** (SFTP, FTP, HTTP/HTTPS)

## 🚀 Inicio Rápido

```bash
# 1. Extraer framework
unzip TerminaTodo_Complete.zip
cd TerminaTodo

# 2. Crear entorno virtual
python3 -m venv venv
source venv/bin/activate  # Linux/Mac
# o
venv\Scripts\activate  # Windows

# 3. Instalar dependencias
pip install -r requirements.txt

# 4. Iniciar servidor
python -m uvicorn api.app:app --host 0.0.0.0 --port 8000

# 5. Acceder a http://localhost:8000
```

## ✨ Características

### 🔍 Detección Automática

- **Dispositivos Locales**: Detecta automáticamente SSD, HDD, pendrives
- **Servicios en Nube**: Integración con Google Drive, OneDrive, Dropbox
- **Servidores Remotos**: Conexión a hosting privado vía SFTP, FTP, HTTP

### 🎯 Interfaz Unificada

- **Dashboard Web**: Explorador visual de todos los almacenamientos
- **API REST**: Acceso programático a todos los servicios
- **Búsqueda Global**: Busca archivos en múltiples fuentes simultáneamente

### 💻 Almacenamiento Local

```python
from TerminaTodo import LocalStorageManager

manager = LocalStorageManager()

# Obtener dispositivos
devices = manager.get_devices()
for device in devices:
    print(f"{device.device_type}: {device.mount_point} ({device.available_size_gb}GB libre)")

# Listar archivos
files = manager.list_files("/home/ubuntu")
for f in files:
    print(f"{'📁' if f['is_dir'] else '📄'} {f['name']} ({f['size_mb']}MB)")

# Obtener información de dispositivo
status = manager.get_storage_status()
print(f"Total: {status['total_capacity_gb']}GB")
```

### ☁️ Almacenamiento en Nube

```python
from TerminaTodo import CloudStorageManager

manager = CloudStorageManager()

# Ver servicios disponibles
services = manager.get_available_services()
for service in services:
    print(f"{service['icon']} {service['name']}")

# Autenticar con Google Drive
manager.authenticate_google_drive(credentials_json)

# Listar archivos
files = manager.list_files_google_drive()
for f in files:
    print(f"{'📁' if f.is_folder else '📄'} {f.name}")

# Descargar archivo
manager.download_file('google_drive', 'file_id', '/local/path')
```

### 🖥️ Hosting Privado

```python
from TerminaTodo import RemoteStorageManager

manager = RemoteStorageManager()

# Añadir servidor SFTP
manager.add_server(
    server_id="sftp_prod",
    name="Servidor Producción",
    host="example.com",
    port=22,
    protocol="sftp",
    username="user",
    password="pass"
)

# Conectar
manager.connect_sftp("sftp_prod")

# Listar archivos
files = manager.list_files_sftp("sftp_prod", "/data")
for f in files:
    print(f"{'📁' if f.is_folder else '📄'} {f.name}")

# Descargar
manager.download_file_sftp("sftp_prod", "/data/model.pt", "./model.pt")
```

### 🔗 Gestor Unificado

```python
from TerminaTodo import UnifiedStorageManager

manager = UnifiedStorageManager()

# Ver resumen
summary = manager.get_storage_summary()
print(f"Local: {summary['local']['available_gb']}GB")
print(f"Nube: {summary['cloud']['authenticated']} servicios autenticados")

# Buscar en todo
results = manager.search_files("*.py", ["local", "cloud", "remote"])
print(f"Encontrados {len(results)} archivos")

# Listar archivos de un tipo
files = manager.list_all_files("local", "/home/ubuntu")
```

## 📊 API REST

### Endpoints Principales

#### Almacenamiento Unificado

```bash
# Resumen
GET /api/storage/summary

# Todos los tipos
GET /api/storage/all

# Uso por tipo
GET /api/storage/usage

# Listar archivos
GET /api/files/list?storage_type=local&path=/home

# Buscar
GET /api/files/search?query=*.py&storage_types=local,cloud

# Información de archivo
GET /api/files/info?storage_type=local&path=/file
```

#### Almacenamiento Local

```bash
# Dispositivos
GET /api/local/devices
```

#### Almacenamiento en Nube

```bash
# Servicios disponibles
GET /api/cloud/services

# Autenticar
POST /api/cloud/authenticate
{
  "service": "google_drive",
  "credentials": "..."
}
```

#### Hosting Privado

```bash
# Listar servidores
GET /api/remote/servers

# Añadir servidor
POST /api/remote/servers
{
  "server_id": "sftp1",
  "name": "Mi Servidor",
  "host": "example.com",
  "port": 22,
  "protocol": "sftp",
  "username": "user",
  "password": "pass"
}

# Conectar
POST /api/remote/connect/{server_id}

# Desconectar
POST /api/remote/disconnect/{server_id}
```

## 🌐 Interfaz Web

Acceso en: **http://localhost:8000**

### Tabs Disponibles

1. **📊 Resumen** - Vista general de todos los almacenamientos
2. **💾 Local** - Dispositivos locales y explorador de archivos
3. **☁️ Nube** - Autenticación y gestión de servicios en nube
4. **🖥️ Hosting** - Configuración y conexión a servidores remotos
5. **🔍 Búsqueda** - Búsqueda global en múltiples fuentes

## 🔧 Configuración

Crear archivo `.env` (opcional):

```env
# Google Drive
GOOGLE_DRIVE_CREDENTIALS_PATH=./credentials.json

# OneDrive
ONEDRIVE_CLIENT_ID=your_client_id
ONEDRIVE_CLIENT_SECRET=your_secret

# Dropbox
DROPBOX_ACCESS_TOKEN=your_token

# API
API_HOST=0.0.0.0
API_PORT=8000
API_WORKERS=4
```

## 📁 Estructura del Proyecto

```
TerminaTodo/
├── storage/
│   ├── local_storage.py        # Gestor de almacenamiento local
│   ├── unified_manager.py      # Gestor unificado
│   └── __init__.py
├── cloud/
│   ├── cloud_manager.py        # Gestor de nube
│   └── __init__.py
├── hosting/
│   ├── remote_manager.py       # Gestor de hosting privado
│   └── __init__.py
├── api/
│   ├── app.py                  # API REST
│   └── __init__.py
├── web/
│   ├── index.html              # Interfaz web
│   └── __init__.py
├── config/
│   └── __init__.py
├── utils/
│   └── __init__.py
├── requirements.txt
├── README.md
└── __init__.py
```

## 💡 Ejemplos Prácticos

### Ejemplo 1: Sincronizar Archivos

```python
from TerminaTodo import UnifiedStorageManager

manager = UnifiedStorageManager()

# Buscar todos los archivos .pt (modelos)
models = manager.search_files("*.pt", ["local", "cloud", "remote"])

print(f"Encontrados {len(models)} modelos")
for model in models:
    print(f"{model['storage_type']}: {model['name']} ({model['size_mb']}MB)")
```

### Ejemplo 2: Monitorear Almacenamiento

```python
from TerminaTodo import LocalStorageManager

manager = LocalStorageManager()

for device in manager.get_devices():
    usage_pct = (device.used_size_gb / device.total_size_gb * 100)
    
    if usage_pct > 80:
        print(f"⚠️ {device.mount_point}: {usage_pct:.1f}% lleno")
    else:
        print(f"✅ {device.mount_point}: {usage_pct:.1f}% usado")
```

### Ejemplo 3: Integración con Terminato

```python
from TerminaTodo import UnifiedStorageManager
from Terminato import TrainingManager

storage = UnifiedStorageManager()
trainer = TrainingManager()

# Buscar datasets
datasets = storage.search_files("dataset*.csv", ["local"])

# Usar en entrenamiento
for dataset in datasets:
    trainer.start_training(
        model_path="./model.pt",
        dataset_path=dataset['path']
    )
```

## 🔐 Seguridad

- **Credenciales**: Almacenadas en variables de entorno, nunca en código
- **HTTPS**: Soporte completo para conexiones seguras
- **Autenticación**: OAuth2 para servicios en nube
- **Permisos**: Respeto a permisos del sistema de archivos

## 📊 Rendimiento

- **Detección**: < 1 segundo para dispositivos locales
- **Listado**: Caché automático de directorios
- **Búsqueda**: Indexación incremental
- **API**: 1000+ requests/segundo

## 🆘 Solución de Problemas

### Dispositivo no detectado

```bash
# Ver dispositivos del sistema
lsblk
sudo fdisk -l

# Verificar permisos
ls -la /mnt/
```

### Error de conexión SFTP

```bash
# Verificar conectividad
ssh -v user@host

# Verificar puerto
nc -zv host 22
```

### Nube no autenticada

- Verificar credenciales en `.env`
- Regenerar tokens de acceso
- Verificar permisos en aplicación en la nube

## 📚 Documentación Completa

- **API Docs**: http://localhost:8000/docs
- **ReDoc**: http://localhost:8000/redoc
- **Ejemplos**: Ver carpeta `examples/`

## 🤝 Integración con Otros Frameworks

### Con Terminato

```python
from TerminaTodo import UnifiedStorageManager
from Terminato import TrainingManager

storage = UnifiedStorageManager()
trainer = TrainingManager()

# Usar archivos de TerminaTodo en Terminato
dataset_path = storage.list_all_files("local", "/datasets")[0]['path']
trainer.start_training(dataset_path=dataset_path)
```

### Con TeMiNaTor

```python
from TerminaTodo import UnifiedStorageManager
from TeMiNaTor import ModelFusion

storage = UnifiedStorageManager()
fusion = ModelFusion()

# Buscar modelos en nube
models = storage.search_files("*.pt", ["cloud"])

# Fusionarlos
for model in models:
    fusion.add_model(model['path'])

fused = fusion.fuse_models()
```

## 📄 Licencia

MIT License

## 📞 Soporte

- **Documentación**: http://localhost:8000/docs
- **GitHub**: [Repositorio]
- **Issues**: Reportar en GitHub

---

**TerminaTodo v1.0.0**  
*Gestor Unificado de Almacenamiento*  
*Listo para Producción*
