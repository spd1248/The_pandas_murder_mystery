
# The Pandas Murder Mystery

## Índice

* [The Pandas Murder Mystery](#the-pandas-murder-mystery)
* [Índice](#índice)
* [Descripción del proyecto](#descripción-del-proyecto)
* [Estado del proyecto](#estado-del-proyecto)
* [Características de la aplicación y demostración](#características-de-la-aplicación-y-demostración)
* [Acceso al proyecto](#acceso-al-proyecto)
* [Tecnologías utilizadas](#tecnologías-utilizadas)
* [Personas Desarrolladores del Proyecto](#personas-desarrolladores-del-proyecto)
* [Licencia](#licencia)
* [Conclusión](#conclusión)

## Descripción del proyecto
The Pandas Murder Mystery es una lección para aprender los conceptos y comandos de la librería Pandas en Python. El proyecto consiste en explorar la base de datos de un crimen y resolver el siguiente caso: 

"Te entregaron el informe del lugar del crimen, pero de alguna manera lo perdiste. Recuerdas vagamente que el crimen fue un ​asesinato​ que ocurrió en algún momento el ​15 de enero de 2018​ y que tuvo lugar en ​Pandas City​."

## Estado del proyecto
El proyecto está terminado. 

## Características de la aplicación y demostración
- **Exploración de Datos**: Análisis y limpieza de datos utilizando Pandas.
- **Visualización de Datos**: Gráficos y visualizaciones para entender mejor el caso.
- **Solución del Caso**: Aplicación de técnicas de análisis para resolver el misterio.
- **Documentación Detallada**: Instrucciones y explicaciones detalladas para guiar a los usuarios a través del proyecto.

## Documentación Detallada

```python
# Paso 1: Buscar en crime_scene_report (DataFrame) el informe del asesinato ocurrido en Pandas City el 2018-01-15 
informe = crime_scene_report[(crime_scene_report["Date"] == "2018-01-15") & 
                            (crime_scene_report["Type"] == "murder") & 
                            (crime_scene_report["City"] == "Pandas City")]
informe
# Reporte del crimen: "Security footage shows that there were 2 witnesses. The first witness lives at the last house on "Northwestern Dr". The second witness, named Annabel, lives somewhere on "Franklin Ave"."

# Paso 2: Buscar en person (DataFrame) los Person_Id de los dos testigos basándonos en la descripción del caso
# El testigo1 vive en la última casa de "Northwestern Dr"
personas_northwestern_dr = person[(person['Address_Street_Name'] == "Northwestern Dr")]
indice_max_numero_direccion = personas_northwestern_dr['Address_Number'].idxmax()
persona_ultima_casa = personas_northwestern_dr.loc[indice_max_numero_direccion]
persona_ultima_casa

# El testigo2 se llama Annabel y vive en "Franklin Ave"
personas_franklin_ave = person[(person['Address_Street_Name'] == "Franklin Ave")]
persona_annabel = personas_franklin_ave.loc[personas_franklin_ave['Name'].str.contains('Annabel')]
persona_annabel

# Paso 3: Buscar en interview (DataFrame) los Person_Id identificados en el Paso 2
# El Person_Id del testigo1 es 14887
entrevista_testigo1 = interview[(interview['Person_Id'] == 14887)]
entrevista_testigo1

# El Person_Id del testigo2 es 16371
entrevista_testigo2 = interview[(interview['Person_Id'] == 16371)]
entrevista_testigo2
# Entrevistas:
# Testigo1: "I heard a gunshot and then saw a man run out. He had a "Get Fit Now Gym" bag. The membership number on the bag started with "48Z". Only gold members have those bags. The man got into a car with a plate that included "H42W"."
# Testigo2: "I saw the murder happen, and I recognized the killer from my gym when I was working out last week on January the 9th."

# Paso 4: Buscar en getfitnow_checkin (DataFrame) las personas que ingresaron al gimnasio el 2018-01-09
personas_ingreso_2018_01_09 = getfitnow_checkin[getfitnow_checkin['Check_In_Date'] == '2018-01-09']
personas_ingreso_2018_01_09

# Paso 5: Buscar en personas_ingreso_2018_01_09 las Membership_Id que empiezan con '48Z'
miembros_48Z = personas_ingreso_2018_01_09[personas_ingreso_2018_01_09['Membership_Id'].str.startswith('48Z')]
miembros_48Z

# Paso 6: Buscar en getfitnow_members (DataFrame) el id de las personas que aparecen en miembros_48Z
# id persona1
id_persona1 = '48Z7A'
persona1 = getfitnow_members[getfitnow_members['Id'] == id_persona1]
persona1

# id persona2
id_persona2 = '48Z55'
persona2 = getfitnow_members[getfitnow_members['Id'] == id_persona2]
persona2

# Paso 7: Buscar en person (DataFrame) el nombre de la persona1 y la persona2
nombre_persona1 = 'Joe Germuska'
fila_persona1 = person[person['Name'] == nombre_persona1]
fila_persona1

nombre_persona2 = 'Jeremy Bowers'
fila_persona2 = person[person['Name'] == nombre_persona2]
fila_persona2

# Paso 8: Buscar en drivers_license (DataFrame) los License_Id
license_id_persona1 = 173289
fila_license_persona1 = drivers_license[drivers_license['Id'] == license_id_persona1]
fila_license_persona1

license_id_persona2 = 423327
fila_license_persona2 = drivers_license[drivers_license['Id'] == license_id_persona2]
fila_license_persona2
# Verificar que la placa coincida con el testimonio del testigo: "0H42W2".

# Paso 9: Ejecutar el código siguiente con el sospechoso:
from asesino import solucion
solucion("Jeremy Bowers")
# Respuesta: ¡Felicidades, encontraste al asesino! Pero espera, hay más... Si crees que estás preparado para un desafío, sigue investigando la transcripción del asesino para encontrar al verdadero villano detrás de este crimen.

# Paso 10: Buscar en interview (DataFrame) el Person_Id de Jeremy Bowers y leer la transcripción de la entrevista
person_id_jeremy_bowers = 67318
asesino = interview[interview['Person_Id'] == person_id_jeremy_bowers]
asesino
# Transcripción: "I was hired by a woman with a lot of money. I don't know her name but I know she's around 5'5" (65") or 5'7" (67"). She has red hair and she drives a Tesla Model S. I know that she attended the SQL Symphony Concert 3 times in December 2017."

# Paso 11: Definir variables que aparecen en la transcripción y buscar en drivers_license (DataFrame)
color = 'Red'
marca_auto = 'Tesla'
modelo_auto = 'Model S'
actor_intelectual = drivers_license.loc[(drivers_license['Hair_Color'] == color) & 
                                        (drivers_license['Car_Make'] == marca_auto) & 
                                        (drivers_license['Car_Model'] == modelo_auto)]
actor_intelectual

# Paso 12: Buscar el Id de una persona con una altura entre 65 y 67
resultado = actor_intelectual.loc[(actor_intelectual['Height'] >= 65) & 
                                  (actor_intelectual['Height'] <= 67)]
resultado

# Paso 13: Cambiar el formato de la fecha en facebook_event_checkin (DataFrame)
facebook_event_checkin['date'] = pd.to_datetime(facebook_event_checkin['date'], format='%Y%m%d')
facebook_event_checkin

# Paso 14: Definir las fechas como strings y convertirlas a datetime
fecha_min = "2017-12-01"
fecha_max = "2017-12-31"
fecha_min = pd.to_datetime(fecha_min)
fecha_max = pd.to_datetime(fecha_max)
rango_fecha = facebook_event_checkin.loc[
    (facebook_event_checkin['date'] >= fecha_min) & 
    (facebook_event_checkin['date'] <= fecha_max)
]
rango_fecha

# Paso 15: Filtrar facebook_event_checkin (DataFrame) por rango de fechas y nombre del evento
evento_nombre = "SQL Symphony Concert"
rango_fecha_evento = facebook_event_checkin.loc[
    (facebook_event_checkin['date'] >= fecha_min) &
    (facebook_event_checkin['date'] <= fecha_max) &
    (facebook_event_checkin['event_name'].str.contains(evento_nombre))
]
rango_fecha_evento

# Paso 16: Identificar el person_id de las personas que hayan asistido 3 veces al evento 'SQL Symphony Concert'
conteo_person_id = rango_fecha_evento['person_id'].value_counts()
conteo_person_id_3_veces = conteo_person_id.loc[conteo_person_id == 3

]
conteo_person_id_3_veces

# Paso 17: Buscar en person (DataFrame) el Id de la primera persona que aparece en conteo_person_id_3_veces
person_id_persona1 = 24556
persona1 = person[person['Id'] == person_id_persona1]
persona1

# Paso 18: Buscar en person (DataFrame) el Id de la segunda persona que aparece en conteo_person_id_3_veces
person_id_persona2 = 99716
persona2 = person[person['Id'] == person_id_persona2]
persona2

# Paso 19: Buscar en drivers_license (DataFrame) el Id de Miranda Priestly dado que la sospechosa es mujer para verificar que cumpla con la transcripción de la declaración del asesino ("I was hired by a woman with a lot of money. I don't know her name but I know she's around 5'5" (65") or 5'7" (67"). She has red hair and she drives a Tesla Model S. I know that she attended the SQL Symphony Concert 3 times in December 2017.")
license_id_persona2 = 202298
licencia_persona2 = drivers_license[drivers_license['Id'] == license_id_persona2]
licencia_persona2
```

## Acceso al proyecto
Puedes acceder al proyecto siguiendo estos pasos:

1. Clonar el repositorio:
   ```bash
   git clone https://github.com/spd1248/The_pandas_murder_mystery.git
   ```
   
2. Navegar al directorio del proyecto:
   ```bash
   cd The_pandas_murder_mystery
   ```

3. Instalar las dependencias necesarias:
   ```bash
   pip install -r requirements.txt
   ```

4. Ejecutar el proyecto:
   ```bash
   python main.py
   ```

El proyecto también está disponible en la siguiente URL: [https://github.com/spd1248/The_pandas_murder_mystery](https://github.com/spd1248/The_pandas_murder_mystery)

## Tecnologías utilizadas
El proyecto The Pandas Murder Mystery utiliza las siguientes tecnologías:

- **Python**: Lenguaje de programación principal utilizado para desarrollar el proyecto.
- **Pandas**: Librería de Python para manipulación y análisis de datos.
- **Jupyter Notebook**: Entorno interactivo para la ejecución de código en Python y la visualización de resultados.

## Personas Desarrolladores del Proyecto
Este proyecto fue desarrollado por:

- **Sergio Parada Dommarco** - [spd1248](https://github.com/spd1248)
- **Alain Medel Mejía** - [PhDSchrodinger](https://github.com/PhDSchrodinger)
- **Yesica Méndez** - [](https://github.com/)
- **Laura Sofía** - [](https://github.com/)

## Licencia
Este proyecto está bajo la Licencia MIT. Para más detalles, consulta el archivo [LICENSE](LICENSE).

## Conclusión
El proyecto The Pandas Murder Mystery ha sido una buena oportunidad para aplicar la librería Pandas en Python. A través de la exploración, análisis y visualización de datos, hemos podido resolver un caso ficticio de asesinato.
Esperamos que este proyecto sirva como una guía útil para aquellos que buscan aprender más sobre Pandas y su aplicación en el análisis de datos. 
```


