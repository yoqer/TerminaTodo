# TerminaTodo v2.0

**Gestor Unificado de Almacenamiento Multi-Fuente**

Sincronización automática, caché inteligente y compresión para SSD, HDD, pendrives, Google Drive, OneDrive, Dropbox y servidores remotos.

## 🚀 Instalación Rápida

```bash
# Base
pip install terminatodo

# Con librerías ML
pip install terminatodo[pytorch,tensorflow,nlp]

# Todas las librerías
pip install terminatodo[all]
```

## 💡 Uso Rápido

```python
from terminatodo import UnifiedStorageManager

storage = UnifiedStorageManager()

# Detectar almacenamiento
summary = storage.get_storage_summary()

# Buscar archivos
files = storage.search_files("*.csv", ["local", "cloud"])

# Sincronizar
from terminatodo.sync.sync_manager import SyncManager
sync = SyncManager()
sync.add_job("backup", "local", "/data", "cloud", "/Drive", bidirectional=True)
```

## ✨ Características

- 🔍 Detección automática de almacenamiento
- 🔄 Sincronización bidireccional
- 💾 Caché inteligente con TTL
- 👁️ Monitoreo en tiempo real
- 📦 Compresión automática
- 🌍 Soporte multilingüe
- 🔗 Compatible con TenMinaTorch, TenMiNaTor, Terminato, TERMINATOR

## 📦 Requisitos Opcionales

```bash
# Almacenamiento en Nube
pip install terminatodo[cloud]

# Deep Learning
pip install terminatodo[pytorch]
pip install terminatodo[tensorflow]

# NLP
pip install terminatodo[nlp]

# Visión
pip install terminatodo[vision]

# Audio
pip install terminatodo[audio]

# Frameworks
pip install terminatodo[tenminatorch]
pip install terminatodo[teminador]
pip install terminatodo[terminato]
pip install terminatodo[terminator]
```

## 📚 Documentación

- README.md - Este archivo
- MANUAL_USUARIO.md - Guía completa
- EJEMPLOS_CODIGO_COMBINACIONES.md - 20 ejemplos prácticos
- MANUAL_ERRORES_SOLUCIONES.md - Troubleshooting

## 🔗 Enlaces

- **GitHub:** https://github.com/yoqer/TerminaTodo
- **PyPI:** https://pypi.org/project/terminatodo/
- **Issues:** https://github.com/yoqer/TerminaTodo/issues

## 📄 Licencia

MIT License - Ver LICENSE para detalles

---

**TerminaTodo v2.0** - Gestor Unificado de Almacenamiento
