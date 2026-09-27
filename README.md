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
2. Descargar `kiva_loans.csv` y colocarlo en la carpeta `data/` (el dataset no se incluye en el repositorio).
3. Abrir `kiva_loans.ipynb` en VS Code, seleccionar el kernel del `venv` y ejecutar todas las celdas en orden ("Run All").
4. Al final se genera el dataset limpio (`kiva_loans_clean.parquet`), que tampoco se sube al repositorio.

## Pasos ejecutados

1. Importación del dataset.
2. Análisis exploratorio rápido (`.info()`, `.describe()`).
3. Diagnóstico de problemas (outliers, duplicados, nulos).
4. Transformación y limpieza.
5. Validación post-limpieza (nulos, tipos de datos, forma final).
6. Exportación del dataset limpio.

## Resumen de decisiones de limpieza

- **Duplicados:** no se encontraron filas ni IDs duplicados.
- **Nulos en `tags`, `region`, `use`, `borrower_genders`:** rellenados con un valor explícito (`"Sin etiquetas"` / `"No especificado"`).
- **Nulos en `partner_id`:** se conservan; se concentran en Kenia y EE. UU. (préstamos sin partner de campo).
- **Nulos en `funded_time` y `disbursed_time`:** se conservan como `NaT` (préstamos no financiados por completo o sin desembolso registrado).
- **Nulos en `country_code`:** se eliminaron 8 filas (volumen insignificante).
- **Fechas:** `posted_time`, `disbursed_time`, `funded_time` y `date` convertidas a `datetime`.
- **Texto y números:** sin inconsistencias de formato, valores negativos ni plazos en 0.
- **Outliers en montos:** se conservan; son préstamos grupales legítimos.
- **Columna derivada:** `porcentaje_financiado` (`funded_amount / loan_amount * 100`).
- **Codificación:** `sector`, `country`, `currency` y `repayment_interval` convertidas a `category`.
- **Resultado final:** 671.197 filas × 21 columnas, exportado en formato Parquet.

## Autor

Franco Recabarren
Ray Gomez
