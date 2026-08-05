## 1. Limpieza de datos (Python)

- Lectura de `bios.csv` y revisión de estructura (`head`, `info`, `isna`).
- Conversión de fechas:
  - `born_date` → `Fecha_Nacimiento`
  - `died_date` → `Fecha_Fallecimiento`
- Tratamiento de nulos:
  - Relleno de `Unknown` en ciudad, región, país, NOC y nombre para evitar nulos.
- Normalización de texto:
  - Función `quitar_tildes` para eliminar acentos en `Atleta`, `Ciudad_Nacimiento`, `Region_Nacimiento`, `NOC`.
- Renombrado de columnas a español y formato de análisis:
  - `athlete_id` → `ID_Atleta`
  - `height_cm` → `Estatura_Cm`
  - `weight_kg` → `Peso_Kg`
  - etc.
- Creación de subconjuntos:
  - `df_geo` para análisis geográfico.
  - `df_fisico` para perfil físico (estatura/peso/IMC).
  - `df_tiempo` para análisis temporal.
  - `df_fallecidos` para registros con fecha de fallecimiento.
  
  
  ## 2. Análisis exploratorio (Python)

Se generaron los siguientes gráficos y conclusiones:

- **Distribución de Estatura**  
  Histograma + KDE sobre `Estatura_Cm`. La estatura se concentra alrededor de 175 cm, con forma aproximadamente normal.

- **Distribución de Peso**  
  Histograma + KDE sobre `Peso_Kg`, centrado en torno a 70–75 kg y ligeramente sesgado a la derecha.

- **Perfil Físico de los Atletas (Estatura vs Peso)**  
  Scatter sobre `df_fisico` muestra correlación positiva entre estatura y peso; la mayoría de atletas está en el rango 170–180 cm y 60–80 kg.

- **TOP 10 Países de Nacimiento**  
  Barra horizontal con mapeo de códigos a nombres (Estados Unidos, Alemania, Reino Unido, etc.), mostrando fuerte representación de países con tradición deportiva.

- **Distribución de Años de Nacimiento**  
  Histograma de `Año_Nacimiento`, con máxima concentración entre 1960 y finales de los 70.

- **Registros con y sin Fecha de Fallecimiento**  
  Countplot de `Estado_Fallecimiento`, donde la mayoría de registros no tiene fecha de fallecimiento, lo que limita análisis de mortalidad.
  
  
  ## 3. Modelo en Power BI (DAX)

Se construyó un modelo en Power BI sobre la tabla `bios_new` con:

### Medidas base

- `Total Atletas = COUNTROWS(bios_new)`
- `Paises Unicos = DISTINCTCOUNT(bios_new[Pais_Nacimiento])`
- `Regiones Unicas = DISTINCTCOUNT(bios_new[Region_Nacimiento])`
- `Ciudades Unicas = DISTINCTCOUNT(bios_new[Ciudad_Nacimiento])`

### Medidas de calidad de datos

- `% Con Fecha Nacimiento = DIVIDE([Con Fecha Nacimiento], [Total Atletas])`
- `% Con Fecha Fallecimiento = DIVIDE([Con Fecha Fallecimiento], [Total Atletas])`
- `% Unknown País = DIVIDE(CALCULATE(COUNTROWS(bios_new), bios_new[Pais_Nacimiento] = "Unknown"), [Total Atletas])`

### Medidas físicas

- Promedio, mínimo y máximo de `Estatura_Cm` y `Peso_Kg`.
- `IMC Promedio = AVERAGE(bios_new[IMC])` (IMC calculado en Python y/o Power BI).

### Columnas calculadas

- `Año Nacimiento = YEAR(bios_new[Fecha_Nacimiento])`
- `Década Nacimiento = INT(YEAR(bios_new[Fecha_Nacimiento]) / 10) * 10`
- `Edad = DATEDIFF(bios_new[Fecha_Nacimiento], TODAY(), YEAR)`

### Rangos para segmentación

- `Rango Estatura` con bandas (<160, 160–169, 170–179, 180–189, 190+).
- `Rango Peso` con bandas (<60, 60–69, 70–79, 80–89, 90+).

### Estado de fallecimiento

- `Estado Fallecimiento = IF(ISBLANK(bios_new[Fecha_Fallecimiento]), "Sin fecha de fallecimiento", "Con fecha de fallecimiento")`


## 4. Dashboard en Power BI

El informe `powerbi/base6_atletas.pbix` se compone de tres páginas:

1. **Resumen general**
   - Tarjetas KPI: total atletas, países únicos, regiones únicas, ciudades únicas, % con fecha de nacimiento, % con fecha de fallecimiento.
   - Barras: top países de nacimiento.
   - Gráfico simple de estado de fallecimiento (con/sin fecha).

2. **Perfil físico**
   - Scatter: estatura vs peso.
   - Barras/histogramas: rangos de estatura (`Rango Estatura`) y rangos de peso (`Rango Peso`).
   - Tarjeta: IMC promedio.

3. **Tiempo**
   - Columnas: año de nacimiento.
   - Columnas agregadas: década de nacimiento.
   - (Opcional) histogramas de edad o edad al fallecer si se utiliza la columna `Edad`.
