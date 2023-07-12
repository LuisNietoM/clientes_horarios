# Cálculo de los clientes horarios

## Lógica del negocio

- Domingos y días feriados en Panamá
    - 00:00 - 00:00 (del día siguiente): **Bloque horario bajo fuera de punta**

- Sábados 
    - 00:00 - 11:00: **Bloque horario bajo fuera de punta**
    - 11:00 - 23:00: **Bloque horario medio fuera de punta**
    - 23:00 - 00:00 (del día siguiente): **Bloque horario bajo fuera de punta**

- De lunes a viernes
    - 00:00 - 09:00: **Bloque horario bajo fuera de punta**
    - 09:00 - 17:00: **Bloque horario de punta**
    - 17:00 - 00:00 (del día siguiente): **Bloque horario medio fuera de punta**

## Entrada

- SERIALNO: Número de medidor.
- FECHAFULL: Fecha y hora de la medición.
- HORAFULL: Hora de la medición.
- KWHDEL_ACUM: Lectura acumulada de energía.
- KWDEL: Demanda de energía en el momento de la medición.

## Algoritmo

1. Se carga el archivo CSV de datos en un DataFrame de pandas.
2. Se agrega una columna llamada 'WEEKDAY' al DataFrame, que representa el nombre del día de la semana para cada fecha.
3. Se define una lista de días feriados en Panamá.
4. Se inicializan las listas para almacenar los datos calculados: 'medidor_', 'dia_', 'horario_', 'consumo_', 'bloque_', y 'demanda_max_'.
5. Para cada medidor y cada día en el DataFrame:
    - Se realiza la verificación de los bloques horarios según el día de la semana o si es un día feriado.
    - Se calculan los intervalos de tiempo para cada bloque horario.
    - Se filtran los datos correspondientes al medidor y al día en cuestión.
    - Se calcula el consumo restando el valor máximo y mínimo de la columna 'KWHDEL_ACUM'.
    - Se registra la demanda máxima de la columna 'KWDEL'.
    - Se registran los datos en las listas correspondientes.
6. Se crea un nuevo DataFrame utilizando las listas con los datos calculados.
7. Se crea un pivot table  con el DataFrame obtendio. 
8. Se guarda el DataFrame en un archivo Excel.

## Salida

El DataFrame contiene los resultados del cálculo de consumo y demanda máxima para cada medidor y día en el archivo de datos de entrada. La salida incluye las siguientes columnas:

- 'Medidor': El número de identificación del medidor.
- 'Fecha': La fecha correspondiente al cálculo.
- 'Horario': El intervalo de tiempo en el que se realizó el cálculo, de acuerdo con los bloques horarios establecidos.
- 'Consumo': El consumo de energía calculado para el intervalo de tiempo en kilovatios-hora (KWh).
- 'Bloque': El tipo de bloque horario en el que se encuentra el intervalo de tiempo ('Bajo', 'Medio' o 'Punta').
- 'Demanda máx.': La demanda máxima registrada durante el intervalo de tiempo en kilovatios (KW).
