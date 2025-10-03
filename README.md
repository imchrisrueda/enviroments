# Entornos de desarrollo

Este repositorio contiene la configuración de mis entornos de trabajo organizados en dos formatos:

- **Conda/Mamba (`.yml`)** → para entornos completos, reproducibles y con dependencias nativas (ej. PyTorch + CUDA, librerías geoespaciales, etc.).
- **uv/pip (`requirements.txt`)** → para entornos ligeros, puramente Python, cuando no se requieren binarios del sistema.

Actualmente gestiono tres entornos principales:

1. `ai-gpu` → Inteligencia Artificial con GPU (PyTorch, CUDA, etc.)
2. `dias` → Ciencia de datos + geoespacial + bases de datos
3. `manim` → Animación matemática con Manim

---

## 📂 Estructura del repo

```

.
├─ enviroments/
│  ├─ conda/
│  │  ├─ ai-gpu.yml
│  │  ├─ dias.yml
│  │  └─ manim.yml
│  └─ uv/
│     ├─ ai-gpu-requirements.txt
│     ├─ dias-requirements.txt
└─     └─ manim-requirements.txt

````

- Los archivos en `envs/conda/` se usan con **mamba** o **conda**.  
- Los archivos en `envs/uv/` se usan con **uv** (o pip).  

---

## 🚀 Requisitos previos

### Conda + Mamba
1. Instalar [Miniconda](https://docs.conda.io/en/latest/miniconda.html) o [Anaconda](https://www.anaconda.com/).
2. Instalar **mamba** en el entorno base:
   ```bash
   conda install -n base -c conda-forge mamba
   ````

### uv (alternativa ligera)

1. Instalar [uv](https://docs.astral.sh/uv/):

   ```bash
   curl -LsSf https://astral.sh/uv/install.sh | sh
   ```

---

## 🐍 Usando Conda/Mamba

### Crear un entorno desde cero

```bash
mamba env create -f envs/conda/dias.yml
mamba env create -f envs/conda/ai-gpu.yml
mamba env create -f envs/conda/manim.yml
```

### Activar un entorno

```bash
conda activate dias
conda activate ai-gpu
conda activate manim
```

### Actualizar un entorno existente

```bash
mamba env update -n dias   -f envs/conda/dias.yml --prune
mamba env update -n ai-gpu -f envs/conda/ai-gpu.yml --prune
mamba env update -n manim  -f envs/conda/manim.yml --prune
```

> `--prune` elimina paquetes que ya no están listados en el `.yml`.

### Exportar cambios (para versionar en GitHub)

Si instalas nuevos paquetes en el entorno y quieres actualizar el `.yml`:

```bash
conda env export -n dias --no-builds > envs/conda/dias.yml
conda env export -n ai-gpu --no-builds > envs/conda/ai-gpu.yml
conda env export -n manim --no-builds > envs/conda/manim.yml
```

---

## ⚡ Usando uv/pip

### Crear un entorno ligero

```bash
uv venv
uv pip install -r envs/uv/dias-requirements.txt
```

Esto crea un entorno virtual en `.venv/` y luego instala solo las dependencias Python listadas en el `requirements.txt`.

### Usar el entorno

```bash
source .venv/bin/activate   # Linux/Mac
.venv\Scripts\activate      # Windows PowerShell
```

### Congelar dependencias para uv

Si agregas nuevas librerías:

```bash
uv pip freeze > envs/uv/dias-requirements.txt
```

---

## 🔎 Verificación rápida

* **AI-GPU**:

  ```bash
  python -c "import torch; print(torch.__version__, torch.cuda.is_available())"
  ```

* **dias (geoespacial)**:

  ```bash
  python -c "import geopandas, rasterio; print(geopandas.__version__, rasterio.__version__)"
  ```

* **manim**:

  ```bash
  manim --version
  ```

---

## ✅ Buenas prácticas

* Siempre hacer `git pull` antes de trabajar, para asegurarte de que tu `.yml` o `requirements.txt` estén actualizados.
* Cada vez que instales un paquete nuevo, **exporta y sube el cambio** al repositorio.
* Usa **mamba** para entornos complejos (GPU, geoespacial).
* Usa **uv** para entornos ligeros, rápidos de recrear.
* No subas carpetas de entornos (`env/`, `.venv/`, etc.), solo los archivos `.yml` y `requirements.txt`.

---

## 📌 Cheat sheet de comandos

```bash
# Crear entorno con mamba
mamba env create -f envs/conda/dias.yml

# Actualizar entorno
mamba env update -n dias -f envs/conda/dias.yml --prune

# Activar entorno
conda activate dias

# Exportar entorno actualizado
conda env export -n dias --no-builds > envs/conda/dias.yml

# Crear entorno con uv
uv venv
uv pip install -r envs/uv/dias-requirements.txt
```

