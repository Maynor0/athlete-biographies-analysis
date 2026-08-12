## Objetivo
Analizar la base de biografías de atletas para estudiar características físicas y demográficas como estatura, peso, IMC, edad, país de nacimiento y estado de vida, además de explorar relaciones estadísticas entre estas variables.

## Datos
El dataset principal se encuentra en `bios.csv` y contiene información biográfica de atletas, incluyendo fechas de nacimiento y fallecimiento, lugar de nacimiento, estatura, peso y NOC.

## Flujo de trabajo

### 1. Limpieza y preparación de datos
- Carga del archivo `bios.csv`.
- Revisión de estructura, tipos de datos y valores nulos.
- Conversión de `born_date` y `died_date` a formato fecha.
- Relleno de valores nulos en campos textuales.
- Normalización de texto y eliminación de tildes.
- Renombrado de columnas al español.

### 2. Variables derivadas
- Cálculo de la edad.
- Creación de la variable `Estado_Vida`.
- Cálculo del IMC.
- Cálculo de la década de nacimiento.

### 3. Análisis descriptivo
- Estadísticos básicos de estatura, peso, edad e IMC.
- Distribución por país de nacimiento.
- Histogramas y boxplots.

### 4. Análisis estadístico
- Correlación entre estatura y peso con `pearsonr`.
- Comparación de estatura por país con ANOVA.
- Comparación de IMC por país con ANOVA.
- Comparación de edad entre atletas vivos y fallecidos con t-test.

### 5. Visualización
- Histogramas de estatura y peso.
- Boxplot de IMC.
- Barras por década de nacimiento.
- Dispersión estatura–peso.
- Boxplots por país y por estado de vida.

## Resultados destacados
- La estatura presenta una distribución aproximadamente normal, concentrada entre 170 y 185 cm.
- El peso muestra asimetría a la derecha, con concentración principal entre 60 y 80 kg.
- El IMC presenta numerosos valores atípicos, reflejando diversidad corporal entre atletas.
- Existe una correlación positiva y significativa entre estatura y peso.
- Se observan diferencias significativas en estatura e IMC entre los principales países de nacimiento.
- Los atletas fallecidos presentan, como es esperable, mayor edad mediana que los atletas vivos.

## Tecnologías utilizadas
- Python
- pandas
- numpy
- scipy
- matplotlib
- seaborn

