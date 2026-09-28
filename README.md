# Proyecto 2 - Python: Limpieza de Datos (Kiva Crowdfunding)

Limpieza y análisis exploratorio del dataset de préstamos de Kiva usando Python y Pandas.
Bootcamp de Data Analytics - Factoría F5 Madrid.

## Cómo ejecutar el notebook

1. Clonar el repositorio y crear un entorno virtual:
```bash
python -m venv venv
venv\Scripts\activate
pip install -r requirements.txt
```
2. Descargar `kiva_loans.csv` y colocarlo en la carpeta `data/` (el dataset original no va en el repo, pesa demasiado).
3. Abrir `notebooks/kiva_loans.ipynb` en VS Code, elegir el kernel del `venv` y correr todas las celdas de arriba a abajo ("Run All").
4. Al terminar quedan generados `kiva_loans_clean.csv` y `kiva_loans_clean.parquet`, que tampoco se suben al repo (están en el `.gitignore`).

### Alternativa: Google Colab

También se puede correr directo en Colab, sin instalar nada local. La primera celda detecta sola si está en Colab o en local: si está en Colab, pide autenticarse con la cuenta de Google y descarga `kiva_loans.csv` desde Drive por la API (el archivo ya está compartido); si está en local, lo lee desde `data/kiva_loans.csv` como se explicó arriba. Solo hay que subir el notebook a Colab, ejecutar la primera celda, aceptar el permiso de autenticación, y correr el resto normal.

## Pasos ejecutados

1. Importación del dataset.
2. Análisis exploratorio rápido con `.info()` y `.describe()`.
3. Diagnóstico de problemas: outliers, duplicados y nulos.
4. Transformación y limpieza de las columnas afectadas.
5. Validación post-limpieza (nulos, tipos de datos, forma final).
6. Exportación del dataset limpio en CSV y Parquet.

## Resumen de decisiones de limpieza

- **Duplicados:** no había, ni filas repetidas ni IDs duplicados.
- **`tags`, `region`, `use`, `borrower_genders`:** los nulos no eran errores de carga, sino datos que simplemente no se registraron. En vez de borrar filas los rellenamos con un valor explícito ("sin etiquetas" o "No especificado") para no perder información.
- **`partner_id`:** se dejan los nulos tal cual. Están concentrados en Kenia y EE. UU., países donde los préstamos no pasan por un partner de campo, así que es un patrón real y no un dato faltante por error.
- **`funded_time` y `disbursed_time`:** también se dejan como `NaT`. Un préstamo sin fecha de financiamiento completo es, sencillamente, un préstamo que no se financió del todo.
- **`country_code`:** acá sí eliminamos las filas con nulo (solo 8 sobre 671.205), porque es un código categórico y no tenía sentido inventar un valor.
- **Fechas:** `posted_time`, `disbursed_time`, `funded_time` y `date` venían como texto y se pasaron a `datetime`.
- **Texto y números:** se revisaron `sector`, `activity` y `country` por inconsistencias de formato (mayúsculas, espacios) y no hizo falta corregir nada. Tampoco había plazos en 0 ni valores negativos.
- **Outliers en los montos:** los dejamos. Revisando las descripciones, los préstamos más altos (hasta $100.000) corresponden a proyectos comunitarios o agrícolas reales, no a errores de carga.
- **Columna nueva:** `porcentaje_financiado` (`funded_amount / loan_amount * 100`), para ver qué tan financiado quedó cada préstamo.
- **Categorías:** `sector`, `country`, `currency` y `repayment_interval` pasaron a tipo `category` en vez de one-hot, porque acá el objetivo es explorar datos, no entrenar un modelo.
- **Resultado final:** 671.197 filas y 21 columnas, exportado en CSV y Parquet.

## Autor

Franco Recabarren
Ray Gomez
