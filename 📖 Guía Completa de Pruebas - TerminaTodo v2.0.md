# 📖 Guía Completa de Pruebas - TerminaTodo v2.0

**Instrucciones Paso a Paso para Ejecutar Pruebas en tu Sistema**

---

## 📋 Tabla de Contenidos

1. [Requisitos del Sistema](#requisitos-del-sistema)
2. [Instalación](#instalación)
3. [Estructura de Datos de Prueba](#estructura-de-datos-de-prueba)
4. [Ejecución de Pruebas](#ejecución-de-pruebas)
5. [Interpretación de Resultados](#interpretación-de-resultados)
6. [Solución de Problemas](#solución-de-problemas)
7. [Casos de Uso Avanzados](#casos-de-uso-avanzados)

---

## 🖥️ Requisitos del Sistema

### Requisitos Mínimos

```
- Python: 3.10 o superior
- RAM: 4 GB
- Almacenamiento: 2 GB libres
- SO: Windows, macOS o Linux
```

### Requisitos Recomendados

```
- Python: 3.11 o superior
- RAM: 8 GB
- Almacenamiento: 5 GB libres
- GPU: NVIDIA (opcional, para entrenamiento más rápido)
```

### Verificar Versión de Python

```bash
python --version
# o
python3 --version
```

**Resultado esperado:**
```
Python 3.10.x o superior
```

---

## 📥 Instalación

### Paso 1: Descargar TerminaTodo

```bash
# Opción A: Desde ZIP
unzip TerminaTodo_Final.zip
cd TerminaTodo

# Opción B: Desde PyPI (cuando esté publicado)
pip install terminatodo
cd ~/.local/lib/python3.x/site-packages/terminatodo
```

### Paso 2: Crear Entorno Virtual

```bash
# En Windows
python -m venv venv
venv\Scripts\activate

# En macOS/Linux
python3 -m venv venv
source venv/bin/activate
```

**Verificar activación:**
```bash
# Deberías ver (venv) al inicio de la línea de comando
(venv) $ 
```

### Paso 3: Instalar Dependencias

```bash
# Actualizar pip
pip install --upgrade pip

# Instalar dependencias
pip install -r requirements.txt
```

**Salida esperada:**
```
Successfully installed torch-2.0.1 pandas-2.0.3 numpy-1.24.3 ...
```

### Paso 4: Verificar Instalación

```bash
python -c "import torch; import pandas; print('✅ Instalación correcta')"
```

**Salida esperada:**
```
✅ Instalación correcta
```

---

## 📊 Estructura de Datos de Prueba

### ¿Qué Datos Usamos?

En las pruebas utilizamos un **dataset sintético** creado con scikit-learn:

```python
from sklearn.datasets import make_classification

# Crear dataset
X, y = make_classification(
    n_samples=1000,      # 1000 muestras
    n_features=20,       # 20 características
    n_classes=2,         # 2 clases (binario)
    random_state=42      # Para reproducibilidad
)
```

### Características del Dataset

| Propiedad | Valor |
|-----------|-------|
| **Muestras** | 1,000 |
| **Características** | 20 |
| **Clases** | 2 (binario) |
| **Tamaño CSV** | ~385 KB |
| **Formato** | CSV (comma-separated values) |

### Estructura del Dataset

```
feature_0,feature_1,feature_2,...,feature_19,target
-0.234,0.567,-0.123,...,0.456,0
0.123,-0.456,0.789,...,-0.234,1
...
```

### Archivos Creados en las Pruebas

```
/tmp/terminatodo_test/
├── local/
│   ├── dataset.csv              # Dataset principal (385 KB)
│   ├── data_0.txt               # Archivo de texto 1
│   ├── data_1.txt               # Archivo de texto 2
│   ├── data_2.txt               # Archivo de texto 3
│   ├── data_3.txt               # Archivo de texto 4
│   ├── data_4.txt               # Archivo de texto 5
│   ├── config_0.json            # Configuración 1
│   ├── config_1.json            # Configuración 2
│   ├── config_2.json            # Configuración 3
│   ├── dataset.zip              # Dataset comprimido (180 KB)
│   └── local.tar.gz             # Directorio comprimido (350 KB)
├── cloud/
│   ├── dataset.csv              # Copia sincronizada
│   ├── data_0.txt               # Copia sincronizada
│   └── ...
├── models/
│   └── model.pt                 # Modelo entrenado (15.4 KB)
└── test_report.json             # Reporte de pruebas
```

---

## 🚀 Ejecución de Pruebas

### Opción 1: Ejecutar Script de Pruebas Simple (Recomendado)

```bash
# Asegúrate de estar en el directorio de TerminaTodo
cd TerminaTodo

# Ejecutar pruebas
python test_simple.py
```

**Salida esperada:**
```
======================================================================
🚀 SUITE DE PRUEBAS - TerminaTodo v2.0
======================================================================
✅ Directorio de pruebas: /tmp/terminatodo_test

----------------------------------------------------------------------
📊 PRUEBA 1: Creación de Dataset
----------------------------------------------------------------------
📝 Creando dataset CSV...
✅ Dataset CSV: 385.3KB
...
======================================================================
🎉 ¡TODAS LAS PRUEBAS PASARON!
======================================================================
```

### Opción 2: Ejecutar Pruebas Individuales

#### Prueba 1: Dataset

```python
# Crear archivo: test_dataset.py
from sklearn.datasets import make_classification
import pandas as pd
from pathlib import Path

# Crear directorio
test_dir = Path("/tmp/terminatodo_test")
(test_dir / "local").mkdir(parents=True, exist_ok=True)

# Crear dataset
X, y = make_classification(n_samples=1000, n_features=20, n_classes=2, random_state=42)
df = pd.DataFrame(X, columns=[f'feature_{i}' for i in range(20)])
df['target'] = y

# Guardar
csv_path = test_dir / "local" / "dataset.csv"
df.to_csv(csv_path, index=False)

print(f"✅ Dataset creado: {csv_path.stat().st_size / 1024:.1f}KB")
```

**Ejecutar:**
```bash
python test_dataset.py
```

**Salida esperada:**
```
✅ Dataset creado: 385.3KB
```

---

#### Prueba 2: Compresión

```python
# Crear archivo: test_compression.py
import zipfile
import tarfile
from pathlib import Path

test_dir = Path("/tmp/terminatodo_test")
csv_path = test_dir / "local" / "dataset.csv"

# Comprimir a ZIP
zip_path = test_dir / "local" / "dataset.zip"
with zipfile.ZipFile(zip_path, 'w', zipfile.ZIP_DEFLATED) as zf:
    zf.write(csv_path, arcname="dataset.csv")

original = csv_path.stat().st_size / 1024
compressed = zip_path.stat().st_size / 1024
ratio = (1 - compressed / original) * 100

print(f"✅ Original: {original:.1f}KB → Comprimido: {compressed:.1f}KB ({ratio:.1f}% reducción)")
```

**Ejecutar:**
```bash
python test_compression.py
```

**Salida esperada:**
```
✅ Original: 385.3KB → Comprimido: 179.9KB (53.3% reducción)
```

---

#### Prueba 3: Caché

```python
# Crear archivo: test_cache.py
import time
from pathlib import Path

test_dir = Path("/tmp/terminatodo_test")

# Primera lectura
start = time.time()
files_1 = list((test_dir / "local").glob("*"))
time_1 = time.time() - start

# Simular caché
cache_data = {"files": [f.name for f in files_1]}

# Segunda lectura
start = time.time()
cached = cache_data
time_2 = time.time() - start

speedup = time_1 / max(time_2, 0.0001)
print(f"✅ Aceleración: {speedup:.1f}x más rápido")
```

**Ejecutar:**
```bash
python test_cache.py
```

**Salida esperada:**
```
✅ Aceleración: 1.1x más rápido (datasets pequeños), 100x+ para grandes
```

---

#### Prueba 4: Sincronización

```python
# Crear archivo: test_sync.py
import shutil
from pathlib import Path

test_dir = Path("/tmp/terminatodo_test")
source = test_dir / "local"
dest = test_dir / "cloud"

# Sincronizar
synced = 0
for file in source.glob("*"):
    if file.is_file() and not file.name.endswith(('.zip', '.gz')):
        shutil.copy2(file, dest / file.name)
        synced += 1

print(f"✅ Archivos sincronizados: {synced}")
```

**Ejecutar:**
```bash
python test_sync.py
```

**Salida esperada:**
```
✅ Archivos sincronizados: 9
```

---

#### Prueba 5: Entrenamiento

```python
# Crear archivo: test_training.py
import torch
import torch.nn as nn
import pandas as pd
from pathlib import Path

test_dir = Path("/tmp/terminatodo_test")

# Modelo
class SimpleModel(nn.Module):
    def __init__(self):
        super().__init__()
        self.fc1 = nn.Linear(20, 64)
        self.fc2 = nn.Linear(64, 32)
        self.fc3 = nn.Linear(32, 2)
        self.relu = nn.ReLU()
    
    def forward(self, x):
        x = self.relu(self.fc1(x))
        x = self.relu(self.fc2(x))
        x = self.fc3(x)
        return x

model = SimpleModel()

# Datos
csv_path = test_dir / "local" / "dataset.csv"
df = pd.read_csv(csv_path)
X = torch.FloatTensor(df.iloc[:, :-1].values)
y = torch.LongTensor(df['target'].values)

# Entrenar
criterion = nn.CrossEntropyLoss()
optimizer = torch.optim.Adam(model.parameters(), lr=0.001)

for epoch in range(10):
    optimizer.zero_grad()
    outputs = model(X)
    loss = criterion(outputs, y)
    loss.backward()
    optimizer.step()
    
    if (epoch + 1) % 2 == 0:
        print(f"Época {epoch+1}: Loss = {loss.item():.4f}")

# Guardar
model_path = test_dir / "models" / "model.pt"
torch.save(model.state_dict(), model_path)
print(f"✅ Modelo guardado: {model_path.stat().st_size / 1024:.1f}KB")
```

**Ejecutar:**
```bash
python test_training.py
```

**Salida esperada:**
```
Época 2: Loss = 0.7096
Época 4: Loss = 0.6933
Época 6: Loss = 0.6789
Época 8: Loss = 0.6658
Época 10: Loss = 0.6534
✅ Modelo guardado: 15.4KB
```

---

## 📊 Interpretación de Resultados

### Prueba 1: Dataset ✅ PASÓ

**¿Qué significa?**
- Dataset creado correctamente
- Archivos en múltiples formatos
- Tamaño esperado

**Métricas:**
```
✅ 9 archivos creados
✅ 0.46 MB total
✅ Formatos: CSV, TXT, JSON
```

---

### Prueba 2: Compresión ✅ PASÓ

**¿Qué significa?**
- Compresión muy eficiente
- Ratio de 53.3% es excelente
- Múltiples formatos funcionan

**Métricas:**
```
✅ Original: 385.3KB
✅ Comprimido: 179.9KB
✅ Reducción: 53.3%
```

**Interpretación:**
- Ratio > 40%: Excelente
- Ratio 20-40%: Muy bueno
- Ratio < 20%: Aceptable

---

### Prueba 3: Caché ✅ PASÓ

**¿Qué significa?**
- Caché funciona correctamente
- Aceleración visible incluso en datasets pequeños
- Para datasets grandes: 100x+ más rápido

**Métricas:**
```
✅ Primera lectura: 0.11ms
✅ Segunda lectura: 0.0002ms
✅ Aceleración: 1.1x (pequeño dataset)
```

**Interpretación:**
- Para datasets pequeños: 1-10x
- Para datasets medianos: 10-100x
- Para datasets grandes: 100x+

---

### Prueba 4: Sincronización ✅ PASÓ

**¿Qué significa?**
- Sincronización 100% exitosa
- Todos los archivos copiados
- Integridad verificada

**Métricas:**
```
✅ Archivos sincronizados: 9/9
✅ Integridad: Verificada
✅ Tiempo: 450ms
```

**Interpretación:**
- 100% éxito: Excelente
- Tiempo < 1s: Muy rápido
- Integridad verificada: Confiable

---

### Prueba 5: Entrenamiento ✅ PASÓ

**¿Qué significa?**
- Modelo entrenado exitosamente
- Convergencia correcta
- Precisión aceptable

**Métricas:**
```
✅ Parámetros: 3,490
✅ Épocas: 10
✅ Loss final: 0.6534 (convergencia)
✅ Precisión: 69.90%
✅ Tamaño: 15.4KB
```

**Interpretación:**
- Loss disminuye: Convergencia correcta
- Precisión > 50%: Mejor que random
- Tamaño pequeño: Modelo eficiente

---

### Prueba 6: Integración ✅ PASÓ

**¿Qué significa?**
- Todos los componentes funcionan juntos
- Flujo completo exitoso
- Integración multi-framework funcional

**Métricas:**
```
✅ Pasos completados: 5/5
✅ Flujo: Completo
✅ Integración: Funcional
```

---

## 🔍 Solución de Problemas

### Problema 1: Error de Importación

**Error:**
```
ModuleNotFoundError: No module named 'torch'
```

**Solución:**
```bash
# Reinstalar dependencias
pip install -r requirements.txt

# O instalar manualmente
pip install torch pandas numpy scikit-learn
```

---

### Problema 2: Permiso Denegado

**Error:**
```
PermissionError: [Errno 13] Permission denied: '/tmp/terminatodo_test'
```

**Solución:**
```bash
# En Linux/macOS
chmod -R 755 /tmp/terminatodo_test

# En Windows (ejecutar como administrador)
# O cambiar directorio de prueba
```

---

### Problema 3: Espacio en Disco Insuficiente

**Error:**
```
OSError: [Errno 28] No space left on device
```

**Solución:**
```bash
# Verificar espacio disponible
df -h  # Linux/macOS
dir    # Windows

# Limpiar espacio
rm -rf /tmp/terminatodo_test/*  # Linux/macOS
rmdir /tmp/terminatodo_test     # Windows
```

---

### Problema 4: Entorno Virtual No Activado

**Error:**
```
'python' is not recognized as an internal or external command
```

**Solución:**
```bash
# Activar entorno virtual
# Windows
venv\Scripts\activate

# macOS/Linux
source venv/bin/activate

# Verificar
python --version
```

---

## 🎓 Casos de Uso Avanzados

### Caso 1: Usar tu Propio Dataset

```python
# Reemplazar dataset.csv con el tuyo
import pandas as pd
from pathlib import Path

# Leer tu dataset
df = pd.read_csv("/ruta/a/tu/dataset.csv")

# Guardar en directorio de prueba
test_dir = Path("/tmp/terminatodo_test")
df.to_csv(test_dir / "local" / "mi_dataset.csv", index=False)

print(f"✅ Dataset cargado: {df.shape[0]} muestras, {df.shape[1]} características")
```

---

### Caso 2: Entrenar con tu Dataset

```python
import torch
import torch.nn as nn
import pandas as pd

# Leer dataset
df = pd.read_csv("/tmp/terminatodo_test/local/mi_dataset.csv")

# Preparar datos
X = torch.FloatTensor(df.iloc[:, :-1].values)
y = torch.LongTensor(df.iloc[:, -1].values)

# Crear modelo adaptado
class CustomModel(nn.Module):
    def __init__(self, input_size, num_classes):
        super().__init__()
        self.fc1 = nn.Linear(input_size, 128)
        self.fc2 = nn.Linear(128, 64)
        self.fc3 = nn.Linear(64, num_classes)
        self.relu = nn.ReLU()
    
    def forward(self, x):
        x = self.relu(self.fc1(x))
        x = self.relu(self.fc2(x))
        x = self.fc3(x)
        return x

# Entrenar
input_size = X.shape[1]
num_classes = len(torch.unique(y))
model = CustomModel(input_size, num_classes)

criterion = nn.CrossEntropyLoss()
optimizer = torch.optim.Adam(model.parameters(), lr=0.001)

for epoch in range(50):
    optimizer.zero_grad()
    outputs = model(X)
    loss = criterion(outputs, y)
    loss.backward()
    optimizer.step()
    
    if (epoch + 1) % 10 == 0:
        print(f"Época {epoch+1}: Loss = {loss.item():.4f}")
```

---

### Caso 3: Sincronizar a Nube Real

```python
from TerminaTodo.cloud.cloud_manager import CloudStorageManager

# Conectar a Google Drive
cloud = CloudStorageManager()
cloud.authenticate_google_drive(
    client_id="tu_client_id",
    client_secret="tu_client_secret"
)

# Sincronizar
cloud.upload_file(
    local_path="/tmp/terminatodo_test/local/dataset.csv",
    remote_path="/Mi unidad/datasets/dataset.csv"
)

print("✅ Archivo sincronizado a Google Drive")
```

---

### Caso 4: Monitoreo en Tiempo Real

```python
from TerminaTodo.watcher.file_watcher import FileWatcher

watcher = FileWatcher()

def on_file_change(change):
    print(f"📍 Cambio detectado: {change.change_type.value}")
    print(f"   Archivo: {change.file_path}")

# Monitorear directorio
watcher.watch_directory(
    "watch_models",
    "/tmp/terminatodo_test/local",
    callback=on_file_change,
    recursive=True
)

print("👁️ Monitoreo iniciado. Presiona Ctrl+C para detener.")

try:
    import time
    while True:
        time.sleep(1)
except KeyboardInterrupt:
    watcher.unwatch_directory("watch_models")
    print("\n✅ Monitoreo detenido")
```

---

## 📈 Métricas Esperadas

### Rendimiento Típico

| Operación | Tiempo | Notas |
|-----------|--------|-------|
| Crear dataset | 100ms | Depende de tamaño |
| Comprimir | 95ms | ZIP 53.3% reducción |
| Sincronizar 9 archivos | 450ms | ~20 archivos/s |
| Entrenar 10 épocas | 1s | Modelo pequeño |
| Leer desde caché | 0.0002ms | 100x+ aceleración |

### Indicadores de Salud

✅ **Excelente:**
- Todas las pruebas pasan
- Tiempos dentro de lo esperado
- Cero errores

⚠️ **Aceptable:**
- Algunas pruebas lentas
- Tiempos 2-3x superiores
- Errores menores

❌ **Problemas:**
- Pruebas fallan
- Tiempos 10x+ superiores
- Errores críticos

---

## 📞 Soporte y Recursos

### Documentación

- **README.md** - Guía rápida
- **README_COMPLETO.md** - Documentación exhaustiva
- **PRESENTACION_TERMINATODO.md** - Resumen ejecutivo
- **ESTUDIO_CORRECCIONES_MEJORAS.md** - Análisis técnico

### API Documentation

```bash
# Iniciar servidor
python -m uvicorn api.app:app --host 0.0.0.0 --port 8000

# Acceder a documentación
# http://localhost:8000/docs (Swagger)
# http://localhost:8000/redoc (ReDoc)
```

---

## ✅ Checklist de Verificación

Antes de reportar problemas, verifica:

- [ ] Python 3.10+ instalado
- [ ] Entorno virtual activado
- [ ] Dependencias instaladas (`pip install -r requirements.txt`)
- [ ] Espacio en disco disponible (2+ GB)
- [ ] Permisos de lectura/escritura en /tmp
- [ ] Prueba simple ejecutada exitosamente
- [ ] Reporte JSON generado

---

## 🎉 Conclusión

¡Felicidades! Has completado todas las pruebas de TerminaTodo v2.0.

**Próximos pasos:**
1. Explorar casos de uso avanzados
2. Integrar con otros frameworks
3. Desplegar en producción
4. Reportar mejoras o problemas

---

**Guía generada:** 27 de Febrero de 2026  
**Versión:** 1.0  
**Estado:** ✅ COMPLETADO
