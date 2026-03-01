# 💻 Ejemplos de Código - Combinaciones de TerminaTodo v2.0

**Guía Práctica con Código Ejecutable para Todas las Combinaciones y Integraciones**

---

## 📋 Tabla de Contenidos

1. [Combinación 1: Base Mínima](#combinación-1-base-mínima)
2. [Combinación 2: Data Science Básica](#combinación-2-data-science-básica)
3. [Combinación 3: ML Completo](#combinación-3-ml-completo)
4. [Combinación 4: Pipeline Completo](#combinación-4-pipeline-completo)
5. [Combinación 5: Desarrollo](#combinación-5-desarrollo)
6. [Integración con Terminato](#integración-con-terminato)
7. [Integración con TenMiNaTor](#integración-con-teminador)
8. [Integración con TERMINATOR](#integración-con-terminator)
9. [Casos de Uso Avanzados](#casos-de-uso-avanzados)

---

## 🔧 Combinación 1: Base Mínima

### Instalación
```bash
pip install terminatodo
```

### Ejemplo 1.1: Detectar Almacenamiento Disponible

```python
#!/usr/bin/env python3
"""
Detectar y listar todos los almacenamientos disponibles
"""

from terminatodo import UnifiedStorageManager

def main():
    # Crear gestor unificado
    storage = UnifiedStorageManager()
    
    # Obtener resumen de almacenamiento
    summary = storage.get_storage_summary()
    
    print("=" * 70)
    print("📊 RESUMEN DE ALMACENAMIENTO")
    print("=" * 70)
    
    for storage_type, info in summary.items():
        print(f"\n{storage_type.upper()}:")
        print(f"  Disponible: {info.get('available', 0)} GB")
        print(f"  Usado: {info.get('used', 0)} GB")
        print(f"  Total: {info.get('total', 0)} GB")
        print(f"  Porcentaje: {info.get('usage_percent', 0):.1f}%")

if __name__ == "__main__":
    main()
```

### Ejemplo 1.2: Listar Archivos Locales

```python
#!/usr/bin/env python3
"""
Listar archivos en almacenamiento local
"""

from terminatodo import UnifiedStorageManager
from pathlib import Path

def main():
    storage = UnifiedStorageManager()
    
    # Listar archivos en directorio
    home = str(Path.home())
    files = storage.list_all_files("local", home)
    
    print(f"📁 Archivos en {home}:")
    print("-" * 70)
    
    for file in files[:10]:  # Primeros 10
        print(f"  📄 {file['name']}")
        print(f"     Tamaño: {file['size_mb']:.2f} MB")
        print(f"     Modificado: {file['modified']}")
        print()

if __name__ == "__main__":
    main()
```

### Ejemplo 1.3: Buscar Archivos Específicos

```python
#!/usr/bin/env python3
"""
Buscar archivos por patrón
"""

from terminatodo import UnifiedStorageManager

def main():
    storage = UnifiedStorageManager()
    
    # Buscar archivos CSV
    csv_files = storage.search_files("*.csv", ["local"])
    
    print(f"🔍 Archivos CSV encontrados: {len(csv_files)}")
    print("-" * 70)
    
    for file in csv_files:
        print(f"  📊 {file['name']}")
        print(f"     Ruta: {file['path']}")
        print(f"     Tamaño: {file['size_mb']:.2f} MB")
        print()

if __name__ == "__main__":
    main()
```

### Ejemplo 1.4: Comprimir Archivos

```python
#!/usr/bin/env python3
"""
Comprimir archivos automáticamente
"""

from terminatodo.compression.compressor import Compressor
from pathlib import Path

def main():
    compressor = Compressor()
    
    # Comprimir archivo
    file_path = Path.home() / "documents" / "data.txt"
    
    if file_path.exists():
        result = compressor.compress_file(
            str(file_path),
            format="zip",
            output_dir=str(Path.home() / "compressed")
        )
        
        print("✅ Compresión completada:")
        print(f"  Archivo original: {result['original_size_mb']:.2f} MB")
        print(f"  Archivo comprimido: {result['compressed_size_mb']:.2f} MB")
        print(f"  Ratio: {result['compression_ratio']:.1f}%")
        print(f"  Archivo: {result['output_path']}")
    else:
        print(f"❌ Archivo no encontrado: {file_path}")

if __name__ == "__main__":
    main()
```

### Ejemplo 1.5: Monitorear Cambios de Archivos

```python
#!/usr/bin/env python3
"""
Monitorear cambios en directorio
"""

from terminatodo.watcher.file_watcher import FileWatcher
import time

def on_file_created(event):
    print(f"✨ Archivo creado: {event.src_path}")

def on_file_modified(event):
    print(f"📝 Archivo modificado: {event.src_path}")

def on_file_deleted(event):
    print(f"🗑️  Archivo eliminado: {event.src_path}")

def main():
    watcher = FileWatcher()
    
    # Registrar callbacks
    watcher.on_created = on_file_created
    watcher.on_modified = on_file_modified
    watcher.on_deleted = on_file_deleted
    
    # Monitorear directorio
    watch_path = "/home/user/documents"
    watcher.start_watching(watch_path)
    
    print(f"👁️  Monitoreando {watch_path}...")
    print("Presiona Ctrl+C para detener")
    
    try:
        while True:
            time.sleep(1)
    except KeyboardInterrupt:
        print("\n⏹️  Monitoreo detenido")
        watcher.stop_watching()

if __name__ == "__main__":
    main()
```

---

## 📊 Combinación 2: Data Science Básica

### Instalación
```bash
pip install terminatodo[ml,cloud]
```

### Ejemplo 2.1: Cargar Dataset desde Almacenamiento

```python
#!/usr/bin/env python3
"""
Cargar dataset desde múltiples fuentes
"""

from terminatodo import UnifiedStorageManager
from terminatodo.config.optional_deps import DependencyGroup, require
import pandas as pd

def main():
    # Requerir ML
    require(DependencyGroup.ML)
    
    storage = UnifiedStorageManager()
    
    # Buscar archivos CSV
    csv_files = storage.search_files("*.csv", ["local", "cloud"])
    
    print(f"📊 Datasets encontrados: {len(csv_files)}")
    print("-" * 70)
    
    for file in csv_files:
        try:
            # Cargar con pandas
            df = pd.read_csv(file['path'])
            
            print(f"\n📈 {file['name']}")
            print(f"   Filas: {len(df)}")
            print(f"   Columnas: {len(df.columns)}")
            print(f"   Memoria: {df.memory_usage(deep=True).sum() / 1024**2:.2f} MB")
            print(f"   Primeras filas:")
            print(df.head(3).to_string(index=False))
            
        except Exception as e:
            print(f"❌ Error cargando {file['name']}: {e}")

if __name__ == "__main__":
    main()
```

### Ejemplo 2.2: Análisis Exploratorio de Datos

```python
#!/usr/bin/env python3
"""
Análisis exploratorio de datos con pandas
"""

from terminatodo import UnifiedStorageManager
from terminatodo.config.optional_deps import require, DependencyGroup
import pandas as pd
import numpy as np

def main():
    require(DependencyGroup.ML)
    
    storage = UnifiedStorageManager()
    
    # Buscar dataset
    datasets = storage.search_files("*.csv", ["local"])
    
    if not datasets:
        print("❌ No hay datasets disponibles")
        return
    
    # Usar primer dataset
    df = pd.read_csv(datasets[0]['path'])
    
    print("=" * 70)
    print(f"📊 ANÁLISIS: {datasets[0]['name']}")
    print("=" * 70)
    
    # Información general
    print("\n📋 INFORMACIÓN GENERAL:")
    print(f"  Forma: {df.shape}")
    print(f"  Memoria: {df.memory_usage(deep=True).sum() / 1024**2:.2f} MB")
    print(f"  Tipos de datos:\n{df.dtypes}")
    
    # Estadísticas
    print("\n📈 ESTADÍSTICAS:")
    print(df.describe())
    
    # Valores faltantes
    print("\n❓ VALORES FALTANTES:")
    missing = df.isnull().sum()
    if missing.sum() > 0:
        print(missing[missing > 0])
    else:
        print("  Sin valores faltantes")
    
    # Correlaciones
    print("\n🔗 CORRELACIONES:")
    numeric_cols = df.select_dtypes(include=[np.number]).columns
    if len(numeric_cols) > 1:
        corr = df[numeric_cols].corr()
        print(corr.iloc[:5, :5])  # Primeras 5x5

if __name__ == "__main__":
    main()
```

### Ejemplo 2.3: Sincronizar Datasets entre Local y Cloud

```python
#!/usr/bin/env python3
"""
Sincronizar datasets entre almacenamiento local y cloud
"""

from terminatodo.sync.sync_manager import SyncManager
from terminatodo.config.optional_deps import require, DependencyGroup

def main():
    require(DependencyGroup.CLOUD)
    
    sync = SyncManager()
    
    # Crear trabajo de sincronización
    job_id = "datasets_sync"
    
    sync.add_job(
        job_id=job_id,
        source_type="local",
        source_path="/home/user/datasets",
        dest_type="cloud",
        dest_path="/My Drive/Backups/datasets",
        bidirectional=True,
        interval=3600,  # Cada hora
        compress=True
    )
    
    print("🔄 Sincronización configurada:")
    print(f"  ID: {job_id}")
    print(f"  Origen: /home/user/datasets (local)")
    print(f"  Destino: /My Drive/Backups/datasets (cloud)")
    print(f"  Intervalo: 3600 segundos (1 hora)")
    print(f"  Compresión: Habilitada")
    
    # Sincronizar ahora
    print("\n⏳ Sincronizando...")
    success = sync.sync_now(job_id)
    
    if success:
        print("✅ Sincronización completada")
        status = sync.get_job_status(job_id)
        print(f"  Archivos sincronizados: {status.get('files_synced', 0)}")
        print(f"  Bytes transferidos: {status.get('bytes_transferred', 0)}")
    else:
        print("❌ Sincronización fallida")

if __name__ == "__main__":
    main()
```

---

## 🤖 Combinación 3: ML Completo

### Instalación
```bash
pip install terminatodo[ml,cloud,teminador]
```

### Ejemplo 3.1: Entrenar Modelo Simple

```python
#!/usr/bin/env python3
"""
Entrenar modelo de clasificación
"""

from terminatodo import UnifiedStorageManager
from terminatodo.config.optional_deps import require, DependencyGroup
import torch
import torch.nn as nn
import pandas as pd
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import StandardScaler

def main():
    require(DependencyGroup.ML)
    
    storage = UnifiedStorageManager()
    
    # Cargar dataset
    datasets = storage.search_files("*.csv", ["local"])
    if not datasets:
        print("❌ No hay datasets disponibles")
        return
    
    df = pd.read_csv(datasets[0]['path'])
    
    # Preparar datos
    X = df.drop('target', axis=1).values
    y = df['target'].values
    
    # Normalizar
    scaler = StandardScaler()
    X = scaler.fit_transform(X)
    
    # Split
    X_train, X_test, y_train, y_test = train_test_split(
        X, y, test_size=0.2, random_state=42
    )
    
    # Convertir a tensores
    X_train = torch.FloatTensor(X_train)
    y_train = torch.LongTensor(y_train)
    X_test = torch.FloatTensor(X_test)
    y_test = torch.LongTensor(y_test)
    
    # Crear modelo
    class SimpleModel(nn.Module):
        def __init__(self, input_size):
            super().__init__()
            self.fc1 = nn.Linear(input_size, 64)
            self.fc2 = nn.Linear(64, 32)
            self.fc3 = nn.Linear(32, 2)
            self.relu = nn.ReLU()
            self.dropout = nn.Dropout(0.3)
        
        def forward(self, x):
            x = self.relu(self.fc1(x))
            x = self.dropout(x)
            x = self.relu(self.fc2(x))
            x = self.dropout(x)
            x = self.fc3(x)
            return x
    
    model = SimpleModel(X_train.shape[1])
    criterion = nn.CrossEntropyLoss()
    optimizer = torch.optim.Adam(model.parameters(), lr=0.001)
    
    # Entrenar
    print("🤖 Entrenando modelo...")
    epochs = 50
    
    for epoch in range(epochs):
        # Forward
        outputs = model(X_train)
        loss = criterion(outputs, y_train)
        
        # Backward
        optimizer.zero_grad()
        loss.backward()
        optimizer.step()
        
        if (epoch + 1) % 10 == 0:
            print(f"  Época {epoch + 1}/{epochs}, Loss: {loss.item():.4f}")
    
    # Evaluar
    with torch.no_grad():
        outputs = model(X_test)
        _, predicted = torch.max(outputs, 1)
        accuracy = (predicted == y_test).float().mean()
    
    print(f"\n✅ Entrenamiento completado")
    print(f"  Precisión en test: {accuracy.item():.4f}")
    
    # Guardar modelo
    torch.save(model.state_dict(), "/tmp/model.pt")
    print(f"  Modelo guardado: /tmp/model.pt")

if __name__ == "__main__":
    main()
```

### Ejemplo 3.2: Fusionar Múltiples Modelos

```python
#!/usr/bin/env python3
"""
Fusionar múltiples modelos entrenados
"""

from teminador import TenMiNaTor
from terminatodo import UnifiedStorageManager
from terminatodo.config.optional_deps import require, DependencyGroup

def main():
    require(DependencyGroup.TEMINADOR)
    
    storage = UnifiedStorageManager()
    
    # Buscar modelos
    models = storage.search_files("*.pt", ["local"])
    
    if len(models) < 2:
        print("❌ Se necesitan al menos 2 modelos para fusionar")
        return
    
    print(f"🔗 Fusionando {len(models)} modelos...")
    
    # Crear fusionador
    fusion = TenMiNaTor(similarity_threshold=0.95)
    
    # Agregar modelos
    for i, model in enumerate(models[:3]):  # Máximo 3
        fusion.add_model(model['path'], f"model_{i}")
        print(f"  ✓ Agregado: {model['name']}")
    
    # Fusionar
    print("\n⏳ Fusionando...")
    fused_model = fusion.fuse_models(strategy="weighted")
    
    print("✅ Fusión completada")
    print(f"  Modelo fusionado: {fused_model}")
    
    # Guardar
    fusion.save_fused_model("/tmp/fused_model.pt")
    print(f"  Guardado en: /tmp/fused_model.pt")

if __name__ == "__main__":
    main()
```

### Ejemplo 3.3: Fine-tuning Dirigido

```python
#!/usr/bin/env python3
"""
Fine-tuning dirigido a capas específicas
"""

from teminador import DirectedFineTuner
from terminatodo import UnifiedStorageManager
from terminatodo.config.optional_deps import require, DependencyGroup
import torch
import pandas as pd

def main():
    require(DependencyGroup.TEMINADOR)
    require(DependencyGroup.ML)
    
    storage = UnifiedStorageManager()
    
    # Cargar modelo
    models = storage.search_files("*.pt", ["local"])
    if not models:
        print("❌ No hay modelos disponibles")
        return
    
    model_path = models[0]['path']
    
    # Cargar datos
    datasets = storage.search_files("*.csv", ["local"])
    if not datasets:
        print("❌ No hay datasets disponibles")
        return
    
    df = pd.read_csv(datasets[0]['path'])
    X = torch.FloatTensor(df.drop('target', axis=1).values)
    y = torch.LongTensor(df['target'].values)
    
    # Crear fine-tuner
    finetuner = DirectedFineTuner(
        model_path=model_path,
        learning_rate=0.001
    )
    
    # Congelar capas base
    finetuner.freeze_layer('fc1')
    finetuner.freeze_layer('fc2')
    
    print("🔧 Fine-tuning configurado:")
    print("  Capas congeladas: fc1, fc2")
    print("  Capas entrenables: fc3")
    print("  Learning rate: 0.001")
    
    # Entrenar
    print("\n⏳ Entrenando...")
    finetuner.train(X, y, max_epochs=50)
    
    print("✅ Fine-tuning completado")
    
    # Guardar
    finetuner.save("/tmp/finetuned_model.pt")
    print("  Modelo guardado: /tmp/finetuned_model.pt")

if __name__ == "__main__":
    main()
```

---

## 🚀 Combinación 4: Pipeline Completo

### Instalación
```bash
pip install terminatodo[ml,cloud,remote,teminador,terminato,terminator]
```

### Ejemplo 4.1: Pipeline End-to-End

```python
#!/usr/bin/env python3
"""
Pipeline completo: Datos → Entrenamiento → Fusión → Inferencia
"""

from terminatodo import UnifiedStorageManager
from teminador import TenMiNaTor
from terminato import Terminato
from terminator import TERMINATOR
from terminatodo.config.optional_deps import require, DependencyGroup
import pandas as pd
import torch

def main():
    require(DependencyGroup.ML)
    require(DependencyGroup.TEMINADOR)
    require(DependencyGroup.TERMINATO)
    require(DependencyGroup.TERMINATOR)
    
    storage = UnifiedStorageManager()
    
    # PASO 1: Buscar datos
    print("=" * 70)
    print("PASO 1: 📊 Buscando datos...")
    print("=" * 70)
    
    datasets = storage.search_files("*.csv", ["local", "cloud"])
    print(f"✓ Encontrados {len(datasets)} datasets")
    
    # PASO 2: Entrenar modelos
    print("\n" + "=" * 70)
    print("PASO 2: 🤖 Entrenando modelos...")
    print("=" * 70)
    
    trainer = Terminato()
    trained_models = []
    
    for dataset in datasets[:2]:  # Máximo 2
        print(f"\n  Entrenando con {dataset['name']}...")
        
        session = trainer.create_training_session(
            model_name="SimpleModel",
            dataset_path=dataset['path'],
            epochs=10
        )
        
        trainer.start_training(session)
        trained_models.append(session['model_path'])
        print(f"  ✓ Modelo entrenado: {session['model_path']}")
    
    # PASO 3: Fusionar modelos
    print("\n" + "=" * 70)
    print("PASO 3: 🔗 Fusionando modelos...")
    print("=" * 70)
    
    if len(trained_models) > 1:
        fusion = TenMiNaTor()
        
        for i, model_path in enumerate(trained_models):
            fusion.add_model(model_path, f"model_{i}")
            print(f"  ✓ Agregado modelo {i}")
        
        fused_model = fusion.fuse_models(strategy="weighted")
        print(f"✓ Modelo fusionado: {fused_model}")
    else:
        fused_model = trained_models[0]
        print("✓ Solo 1 modelo, usando directamente")
    
    # PASO 4: Inferencia dirigida
    print("\n" + "=" * 70)
    print("PASO 4: 🎯 Realizando inferencia dirigida...")
    print("=" * 70)
    
    inference = TERMINATOR()
    
    # Cargar datos de prueba
    test_data = torch.randn(10, 20)  # 10 muestras, 20 características
    
    results = inference.infer(
        model_path=fused_model,
        input_data=test_data,
        steering_strength=0.5
    )
    
    print(f"✓ Inferencia completada")
    print(f"  Predicciones: {results['predictions']}")
    print(f"  Confianza: {results['confidence']:.4f}")
    
    # RESUMEN
    print("\n" + "=" * 70)
    print("✅ PIPELINE COMPLETADO")
    print("=" * 70)
    print(f"  Datasets procesados: {len(datasets)}")
    print(f"  Modelos entrenados: {len(trained_models)}")
    print(f"  Modelo final: {fused_model}")

if __name__ == "__main__":
    main()
```

### Ejemplo 4.2: Sincronización Multi-Fuente

```python
#!/usr/bin/env python3
"""
Sincronizar datos desde múltiples fuentes
"""

from terminatodo.sync.sync_manager import SyncManager
from terminatodo.config.optional_deps import require, DependencyGroup

def main():
    require(DependencyGroup.CLOUD)
    require(DependencyGroup.REMOTE)
    
    sync = SyncManager()
    
    # Sincronización 1: Local ↔ Cloud
    sync.add_job(
        job_id="local_to_cloud",
        source_type="local",
        source_path="/home/user/data",
        dest_type="cloud",
        dest_path="/My Drive/Data",
        bidirectional=True,
        interval=3600
    )
    
    # Sincronización 2: Cloud → Remote
    sync.add_job(
        job_id="cloud_to_remote",
        source_type="cloud",
        source_path="/My Drive/Data",
        dest_type="remote",
        dest_path="/backups/data",
        interval=86400  # Diariamente
    )
    
    # Sincronización 3: Remote → Local (Backup)
    sync.add_job(
        job_id="remote_to_local",
        source_type="remote",
        source_path="/backups/data",
        dest_type="local",
        dest_path="/home/user/backups",
        interval=604800  # Semanalmente
    )
    
    print("🔄 Trabajos de sincronización configurados:")
    print("\n  1️⃣  Local ↔ Cloud (cada hora)")
    print("  2️⃣  Cloud → Remote (diariamente)")
    print("  3️⃣  Remote → Local (semanalmente)")
    
    # Ejecutar todas las sincronizaciones
    print("\n⏳ Ejecutando sincronizaciones...")
    
    for job_id in ["local_to_cloud", "cloud_to_remote", "remote_to_local"]:
        success = sync.sync_now(job_id)
        status = "✅" if success else "❌"
        print(f"  {status} {job_id}")

if __name__ == "__main__":
    main()
```

---

## 🛠️ Combinación 5: Desarrollo

### Instalación
```bash
pip install terminatodo[all,dev]
```

### Ejemplo 5.1: Testing con Pytest

```python
#!/usr/bin/env python3
"""
Tests para TerminaTodo
"""

import pytest
from terminatodo import UnifiedStorageManager
from terminatodo.sync.sync_manager import SyncManager
from terminatodo.cache.cache_manager import CacheManager
from pathlib import Path

class TestStorageManager:
    """Tests para gestor de almacenamiento"""
    
    def setup_method(self):
        """Preparar antes de cada test"""
        self.storage = UnifiedStorageManager()
    
    def test_get_storage_summary(self):
        """Probar obtener resumen de almacenamiento"""
        summary = self.storage.get_storage_summary()
        assert summary is not None
        assert 'local' in summary
    
    def test_search_files(self):
        """Probar búsqueda de archivos"""
        files = self.storage.search_files("*.py", ["local"])
        assert isinstance(files, list)
    
    def test_list_all_files(self):
        """Probar listar archivos"""
        home = str(Path.home())
        files = self.storage.list_all_files("local", home, batch_size=10)
        assert isinstance(files, list)

class TestSyncManager:
    """Tests para gestor de sincronización"""
    
    def setup_method(self):
        """Preparar antes de cada test"""
        self.sync = SyncManager()
    
    def test_add_job(self):
        """Probar agregar trabajo"""
        self.sync.add_job(
            job_id="test_job",
            source_type="local",
            source_path="/tmp",
            dest_type="local",
            dest_path="/tmp/backup"
        )
        
        jobs = self.sync.get_all_jobs()
        assert "test_job" in jobs
    
    def test_job_status(self):
        """Probar estado del trabajo"""
        self.sync.add_job(
            job_id="test_job",
            source_type="local",
            source_path="/tmp",
            dest_type="local",
            dest_path="/tmp/backup"
        )
        
        status = self.sync.get_job_status("test_job")
        assert status is not None

class TestCacheManager:
    """Tests para gestor de caché"""
    
    def setup_method(self):
        """Preparar antes de cada test"""
        self.cache = CacheManager()
    
    def test_set_get(self):
        """Probar set y get"""
        self.cache.set("key", "value")
        assert self.cache.get("key") == "value"
    
    def test_ttl(self):
        """Probar TTL"""
        self.cache.set("key", "value", ttl=1)
        assert self.cache.get("key") == "value"
        
        import time
        time.sleep(2)
        assert self.cache.get("key") is None

if __name__ == "__main__":
    pytest.main([__file__, "-v"])
```

### Ejemplo 5.2: Verificación de Código

```bash
#!/bin/bash
# Script para verificar calidad de código

echo "🔍 Verificando código..."

# Black - Formateo
echo "  Formateando con Black..."
black terminatodo/

# Flake8 - Estilo
echo "  Verificando estilo con Flake8..."
flake8 terminatodo/ --max-line-length=100

# MyPy - Type checking
echo "  Verificando tipos con MyPy..."
mypy terminatodo/

# Pytest - Tests
echo "  Ejecutando tests..."
pytest tests/ -v --cov=terminatodo

echo "✅ Verificación completada"
```

---

## 🔗 Integración con Terminato

### Ejemplo: Entrenar con Datasets de TerminaTodo

```python
#!/usr/bin/env python3
"""
Usar TerminaTodo para gestionar datos y Terminato para entrenar
"""

from terminatodo import UnifiedStorageManager
from terminato import Terminato
from terminatodo.config.optional_deps import require, DependencyGroup

def main():
    require(DependencyGroup.TERMINATO)
    
    storage = UnifiedStorageManager()
    trainer = Terminato()
    
    # Buscar datasets
    datasets = storage.search_files("*.csv", ["local", "cloud"])
    
    print(f"📊 Encontrados {len(datasets)} datasets")
    print("=" * 70)
    
    # Entrenar con cada dataset
    for dataset in datasets:
        print(f"\n🤖 Entrenando con: {dataset['name']}")
        
        # Crear sesión
        session = trainer.create_training_session(
            model_name="ResNet50",
            dataset_path=dataset['path'],
            epochs=50,
            batch_size=32,
            learning_rate=0.001,
            gpu_id=0
        )
        
        # Iniciar entrenamiento
        trainer.start_training(session)
        
        # Monitorear progreso
        while trainer.is_training(session['id']):
            status = trainer.get_training_status(session['id'])
            print(f"  Época {status['epoch']}/{status['total_epochs']}")
            print(f"  Loss: {status['loss']:.4f}")
            print(f"  Precisión: {status['accuracy']:.4f}")
        
        # Guardar resultado
        print(f"✅ Entrenamiento completado")
        print(f"  Modelo: {session['model_path']}")
        print(f"  Precisión final: {status['accuracy']:.4f}")

if __name__ == "__main__":
    main()
```

---

## 🔗 Integración con TenMiNaTor

### Ejemplo: Fusión y Fine-tuning

```python
#!/usr/bin/env python3
"""
Usar TerminaTodo para gestionar modelos y TenMiNaTor para fusionar
"""

from terminatodo import UnifiedStorageManager
from teminador import TenMiNaTor, DirectedFineTuner
from terminatodo.config.optional_deps import require, DependencyGroup

def main():
    require(DependencyGroup.TEMINADOR)
    
    storage = UnifiedStorageManager()
    
    # PASO 1: Buscar modelos
    print("=" * 70)
    print("PASO 1: 🔍 Buscando modelos...")
    print("=" * 70)
    
    models = storage.search_files("*.pt", ["local", "cloud"])
    print(f"✓ Encontrados {len(models)} modelos")
    
    # PASO 2: Fusionar
    print("\n" + "=" * 70)
    print("PASO 2: 🔗 Fusionando modelos...")
    print("=" * 70)
    
    fusion = TenMiNaTor(similarity_threshold=0.95)
    
    for i, model in enumerate(models[:3]):
        fusion.add_model(model['path'], f"model_{i}")
        print(f"  ✓ {model['name']}")
    
    fused = fusion.fuse_models(strategy="weighted")
    print(f"✓ Modelo fusionado")
    
    # PASO 3: Fine-tuning
    print("\n" + "=" * 70)
    print("PASO 3: 🎯 Fine-tuning dirigido...")
    print("=" * 70)
    
    # Buscar dataset
    datasets = storage.search_files("*.csv", ["local"])
    
    if datasets:
        finetuner = DirectedFineTuner(
            model_path=fused,
            learning_rate=0.0001
        )
        
        # Congelar capas base
        finetuner.freeze_layer('layer1')
        finetuner.freeze_layer('layer2')
        
        print("  Capas congeladas: layer1, layer2")
        print("  Capas entrenables: layer3, layer4")
        
        # Entrenar
        import pandas as pd
        import torch
        
        df = pd.read_csv(datasets[0]['path'])
        X = torch.FloatTensor(df.drop('target', axis=1).values)
        y = torch.LongTensor(df['target'].values)
        
        finetuner.train(X, y, max_epochs=50)
        print("✓ Fine-tuning completado")
        
        # Guardar
        finetuner.save("/tmp/finetuned_fused.pt")
        print(f"✓ Modelo guardado: /tmp/finetuned_fused.pt")

if __name__ == "__main__":
    main()
```

---

## 🎯 Integración con TERMINATOR

### Ejemplo: Inferencia Dirigida

```python
#!/usr/bin/env python3
"""
Usar TerminaTodo para gestionar modelos y TERMINATOR para inferencia
"""

from terminatodo import UnifiedStorageManager
from terminator import TERMINATOR
from terminatodo.config.optional_deps import require, DependencyGroup
import torch

def main():
    require(DependencyGroup.TERMINATOR)
    
    storage = UnifiedStorageManager()
    inference = TERMINATOR()
    
    # Buscar modelo
    models = storage.search_files("*.pt", ["local"])
    
    if not models:
        print("❌ No hay modelos disponibles")
        return
    
    model_path = models[0]['path']
    
    print("=" * 70)
    print(f"🎯 INFERENCIA DIRIGIDA")
    print("=" * 70)
    print(f"Modelo: {models[0]['name']}")
    
    # Crear datos de entrada
    batch_size = 5
    input_features = 20
    input_data = torch.randn(batch_size, input_features)
    
    print(f"\nEntrada: {input_data.shape}")
    
    # INFERENCIA 1: Sin steering
    print("\n1️⃣  Inferencia normal:")
    results_normal = inference.infer(
        model_path=model_path,
        input_data=input_data
    )
    
    print(f"  Predicciones: {results_normal['predictions']}")
    print(f"  Confianza: {results_normal['confidence']:.4f}")
    
    # INFERENCIA 2: Con steering
    print("\n2️⃣  Inferencia con steering:")
    
    steering_vector = torch.randn(input_features) * 0.1
    
    results_steered = inference.infer(
        model_path=model_path,
        input_data=input_data,
        steering_vector=steering_vector,
        steering_strength=0.5
    )
    
    print(f"  Predicciones: {results_steered['predictions']}")
    print(f"  Confianza: {results_steered['confidence']:.4f}")
    
    # INFERENCIA 3: Generación de audio
    print("\n3️⃣  Generación de audio:")
    
    audio_result = inference.generate_audio(
        text="Hola, esto es una prueba",
        model_path=model_path,
        voice_id="default"
    )
    
    print(f"  Audio generado: {audio_result['audio_path']}")
    print(f"  Duración: {audio_result['duration']:.2f}s")
    
    # INFERENCIA 4: Modelo 3D animado
    print("\n4️⃣  Generación de modelo 3D:")
    
    animation_result = inference.generate_animation(
        model_path=model_path,
        input_data=input_data,
        animation_type="lip_sync"
    )
    
    print(f"  Modelo 3D: {animation_result['model_path']}")
    print(f"  Animación: {animation_result['animation_type']}")

if __name__ == "__main__":
    main()
```

---

## 🚀 Casos de Uso Avanzados

### Caso 1: Sistema de Backup Automático

```python
#!/usr/bin/env python3
"""
Sistema de backup automático con sincronización inteligente
"""

from terminatodo.sync.sync_manager import SyncManager
from terminatodo.cache.cache_manager import CacheManager
from terminatodo.compression.compressor import Compressor
import schedule
import time

def backup_job():
    """Trabajo de backup"""
    sync = SyncManager()
    
    # Sincronizar
    success = sync.sync_now("main_backup")
    
    if success:
        print(f"✅ Backup completado: {time.strftime('%Y-%m-%d %H:%M:%S')}")
    else:
        print(f"❌ Backup fallido: {time.strftime('%Y-%m-%d %H:%M:%S')}")

def cleanup_job():
    """Limpiar caché antiguo"""
    cache = CacheManager()
    cache.cleanup()
    print(f"🧹 Caché limpiado: {time.strftime('%Y-%m-%d %H:%M:%S')}")

def compress_job():
    """Comprimir archivos antiguos"""
    compressor = Compressor()
    compressor.compress_old_files(days=30)
    print(f"📦 Compresión completada: {time.strftime('%Y-%m-%d %H:%M:%S')}")

def main():
    # Programar trabajos
    schedule.every().hour.do(backup_job)
    schedule.every().day.at("02:00").do(cleanup_job)
    schedule.every().week.do(compress_job)
    
    print("🔄 Sistema de backup automático iniciado")
    print("  Backup: cada hora")
    print("  Limpieza: diariamente a las 2:00 AM")
    print("  Compresión: semanalmente")
    
    # Loop
    while True:
        schedule.run_pending()
        time.sleep(60)

if __name__ == "__main__":
    main()
```

### Caso 2: Análisis de Datos Distribuido

```python
#!/usr/bin/env python3
"""
Análisis de datos distribuido en múltiples fuentes
"""

from terminatodo import UnifiedStorageManager
from terminatodo.config.optional_deps import require, DependencyGroup
import pandas as pd
import numpy as np
from concurrent.futures import ThreadPoolExecutor

def analyze_file(file_info):
    """Analizar un archivo"""
    try:
        df = pd.read_csv(file_info['path'])
        return {
            'name': file_info['name'],
            'rows': len(df),
            'columns': len(df.columns),
            'memory_mb': df.memory_usage(deep=True).sum() / 1024**2,
            'null_count': df.isnull().sum().sum()
        }
    except Exception as e:
        return {
            'name': file_info['name'],
            'error': str(e)
        }

def main():
    require(DependencyGroup.ML)
    
    storage = UnifiedStorageManager()
    
    # Buscar archivos
    files = storage.search_files("*.csv", ["local", "cloud"])
    
    print(f"📊 Analizando {len(files)} archivos...")
    print("=" * 70)
    
    # Análisis paralelo
    with ThreadPoolExecutor(max_workers=4) as executor:
        results = list(executor.map(analyze_file, files))
    
    # Mostrar resultados
    total_rows = 0
    total_memory = 0
    
    for result in results:
        if 'error' not in result:
            print(f"\n📈 {result['name']}")
            print(f"  Filas: {result['rows']}")
            print(f"  Columnas: {result['columns']}")
            print(f"  Memoria: {result['memory_mb']:.2f} MB")
            print(f"  Nulos: {result['null_count']}")
            
            total_rows += result['rows']
            total_memory += result['memory_mb']
    
    print("\n" + "=" * 70)
    print(f"📊 RESUMEN TOTAL")
    print(f"  Filas totales: {total_rows:,}")
    print(f"  Memoria total: {total_memory:.2f} MB")

if __name__ == "__main__":
    main()
```

---

## ✅ Checklist de Ejemplos

- ✅ Combinación 1: Base Mínima (5 ejemplos)
- ✅ Combinación 2: Data Science Básica (3 ejemplos)
- ✅ Combinación 3: ML Completo (3 ejemplos)
- ✅ Combinación 4: Pipeline Completo (2 ejemplos)
- ✅ Combinación 5: Desarrollo (2 ejemplos)
- ✅ Integración con Terminato (1 ejemplo)
- ✅ Integración con TenMiNaTor (1 ejemplo)
- ✅ Integración con TERMINATOR (1 ejemplo)
- ✅ Casos de Uso Avanzados (2 ejemplos)

**Total: 20 ejemplos de código completos y funcionales**

---

**Ejemplos de Código - TerminaTodo v2.0**  
**Versión:** 1.0  
**Fecha:** 27 de Febrero de 2026
