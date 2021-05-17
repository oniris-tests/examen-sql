# EXAMEN SQL

Para DBA/DBM

Dicho examen debe realizarse en MYSQL, presentando por codigo la solución al problema planteado, en un archivo de nombre examenDBA.sql y enviando dicha respuesta al email examen@proyectoniris.com con nombre y apellido en el asunto del mismo.

Por cualquier consulta, enviar email a info@proyectoniris.com

Buena suerte!

Fases de la aplicacion:

    Toma y análisis de requerimientos.
    Modelo entidad-relación.
    Creación de la estructura o modelo de datos.
    Creación de claves primarias y foráneas de las tablas.
    Inserción de registros en las tablas.
    Informes o explotación de datos.

Análisis de requerimientos:

Requerimientos
    Se necesita una aplicación que permita gestionar las apuestas de quinielas futbolísticas para un único apostante, es decir, 
    para uno mismo. La aplicación deberá ser capaz de escrutar los pronósticos una vez se tiene el resultado de la jornada, e 
    informar de cuantos aciertos se han logrado, no es necesario que gestione los gastos y premios ni apuestas múltiples(con 
    dobles y/o triples). En general deberá permitir mantener los datos referentes a las jornadas de Liga y a las quinielas para 
    explotar los datos referentes a los aciertos.

Análisis:

    Dadas estas especificaciones debemos conocer esencialmente como funcionan las quinielas futbolísticas. En el mundo del fútbol 
    podemos afirmar que se convocan jornadas en las que se organizan eventos, en estos eventos participan equipos, y sobre estos 
    eventos se pronostican resultados a 1,X, 2. El conjunto de pronósticos sobre los eventos de una misma jornada es lo que se llama 
    quiniela. Y el conjunto de resultados de los eventos de una misma jornada es lo que se llama combinación ganadora.

Entidades:

    Del análisis anterior proponemos las siguientes entidades:
        EQUIPOS
        JORNADAS
        EVENTOS
        QUINIELAS
        PRONOSTICOS

Estos son los razonamientos que se hicieron mientras se elaboraba el diagrama de los cuales se obtiene la cardinalidad de las relaciones:

   En una jornada se organizan varios eventos mientras que un evento se celebra en una jornada. Eventos es una entidad débil dependiente 
   de jornadas.
   Un equipo participa en varios eventos como local mientras que en un evento sola hay un equipo local.
   Un equipo participa en varios eventos como visitante mientras que en un evento sola hay un equipo visitante.
   En una jornada se sellan varias quinielas mientras que una quiniela solo es válida para una jornada.
   En una quiniela se realizan varios pronósticos mientras que un pronóstico pertenece a una quiniela concreta. Pronósticos es una
   entidad débil dependiente de quinielas.
   Sobre un evento se realizan varios pronósticos mientras que un pronóstico pertenece a un evento concreto.

   Las entidades EVENTOS y PRONOSTICOS se han considerado débiles. Por lo tanto la clave primaria de estas entidades será compuesta 
   interviniendo en ella la clave foránea de la entidad fuerte de la que dependen. EVENTOS depende de JORNADAS, y PRONOSTICOS depende
   de QUINIELAS.

Atributos de cada entidad

   Este apartado se debería integrar en el modelo entidad-relación, en su lugar lo haremos aparte con la intención de no cargar el 
   diagrama, de modo que el modelo presentado con anterioridad es un modelo simplificado. En la primera columna de la siguiente 
   propuesta de atributos se especifica el nombre del atributo, en la segunda el tipo de dato, en la tercera si puede ser o no nulo, y 
   en la cuarta columna, si se tercia, si es clave primaria, foránea, o los posibles valores si es un campo codificado.

EQUIPOS:

    ID_EQUIPO     numérico      no nulo     clave primaria
    EQUIPO        cadena(30)    no nulo

JORNADAS:

    ID_JORNADA    numérico      no nulo     clave primaria
    NOMBRE        cadena(30)    no nulo
    FECHA         fecha         no nulo
    DISPUTADA     cadena(1)     no nulo     posibles valores: ('S' , 'N') S -> sí , N -> No

EVENTOS:
    
    ID_JORNADA    numérico      no nulo     clave primaria
    ID_EVENTO     numérico      no nulo     clave primaria
    LOCAL         numérico      no nulo     clave foránea de la entidad EQUIPOS
    VISITANTE     numérico      no nulo     clave foránea de la entidad EQUIPOS
    RESULTADO     cadena(1)     nulo        posibles valores: ('1' , 'X' , '2')

QUINIELAS:

    ID_QUINIELA   numérico      no nulo     clave primaria
    ID_JORNADA    numérico      no nulo     clave foránea de la entidad EVENTOS junto con ID_EVENTO
    NOMBRE        cadena(30)    nulo
    ESCRUTADA     cadena(1)     no nulo     posibles valores: ('S' , 'N') S -> sí , N -> No
    ACIERTOS      numerico      nulo

PRONOSTICOS:
 
    ID_QUINIELA   numérico      no nulo     clave primaria
    ID_PRO        numérico      no nulo     clave primaria
    ID_JORNADA    numérico      no nulo     clave foránea de la entidad EVENTOS junto con ID_EVENTO
    ID_EVENTO     numérico      no nulo     clave foránea de la entidad EVENTOS junto con ID_JORNADA
    PRONOSTICO    cadena(1)     no nulo     posibles valores: ('1' , 'X' , '2')

Inserción de registros en las tablas:

   Para alimentar la BD lo habitual es disponer de alguna pantalla, o interface de usuario, donde mediante un formulario 
   permita realizar las inserciones. Como esto queda fuera del alcance de este examen, los registros se han creado directamente 
   con instrucciones de inserción directamente sobre la BD, aunque lo habitual hubiese sido que un usuario insertara los datos 
   desde los formularios de entrada de datos y mantenimiento de la aplicación.

   Para simplificar vamos a suponer que en la competetición participan solo seis equipos y, por tanto, se celebrarán diez 
   jornadas de Liga, cinco la primera vuelta y cinco más la segunda, con un total de tres eventos por jornada. Supondremos 
   también que la competición se encuentra en momento tal que la jornada 8 todavía no se ha disputado, es decir, se han disputado 
   ya 7 jornadas, por lo que en los registros de las tabla JORNADAS referentes a las jornadas 8, 9 y 10 el campo DISPUTADA 
   contendrá una "N" y los registros de la tabla eventos referentes a las mismas jornadas el campo RESULTADO estará a nulo.

Inserciones en la tabla EQUIPOS:

    (ID_EQUIPO, EQUIPO):
        (1, 'Las Palmas');
        (2, 'Xerez');
        (3, 'Getafe');
        (4, 'Nastic');
        (5, 'Celta');
        (6, 'Alcorcón'); 

Inserciones en la tabla JORNADAS:

    (ID_JORNADA, NOMBRE, FECHA, DISPUTADA):
        (1, 'Jornada 1', '2010-01-10', 'S');
        (2, 'Jornada 2', '2010-01-17', 'S');
        (3, 'Jornada 3', '2010-01-24', 'S');
        (4, 'Jornada 4', '2010-02-07', 'S');
        (5, 'Jornada 5', '2010-02-14', 'S');
        (6, 'Jornada 6', '2010-02-21', 'S');
        (7, 'Jornada 7', '2010-03-07', 'S');
        (8, 'Jornada 8', '2010-03-21', 'N');
        (9, 'Jornada 9', '2010-04-04', 'N');
        (10, 'Jornada 10', '2010-04-18', 'N');

Inserciones en la tabla EVENTOS:
    
    (ID_JORNADA, ID_EVENTO, LOCAL, VISITANTE, RESULTADO):
        (1, 1, 5, 1, '1');
        (1, 2, 2, 3, '1');
        (1, 3, 4, 6, '1');
        (2, 1, 1, 2, '2');
        (2, 2, 3, 4, '1');
        (2, 3, 6, 5, '2');
        (3, 1, 1, 4, 'X');
        (3, 2, 3, 5, '2');
        (3, 3, 6, 2, '1');
        (4, 1, 3, 6, '2');
        (4, 2, 2, 4, '2');
        (4, 3, 1, 5, '1');
        (5, 1, 1, 3, '1');
        (5, 2, 5, 2, '2');
        (5, 3, 6, 4, '1');
        (6, 1, 2, 1, 'X');
        (6, 2, 4, 3, '1');
        (6, 3, 5, 6, '2');
        (7, 1, 3, 2, 'X');
        (7, 2, 4, 5, '1');
        (7, 3, 6, 1, 'X');
        (8, 1, 3, 1, NULL);
        (8, 2, 2, 6, NULL);
        (8, 3, 5, 4, NULL);
        (9, 1, 1, 6, NULL);
        (9, 2, 5, 3, NULL);
        (9, 3, 4, 2, NULL);
        (10, 1, 6, 3, NULL);
        (10, 2, 4, 1, NULL);
        (10, 3, 2, 5, NULL);

Inserciones en la tabla QUINIELAS:
    
    (ID_QUINIELA, ID_JORNADA, NOMBRE, ESCRUTADA, ACIERTOS):
        (1, 1, 'Quini 1.1', 'S', 0);
        (2, 1, 'Quini 1.2', 'S', 1);
        (3, 1, 'Quini 1.3', 'S', 0);
        (4, 2, 'Quini 2.1', 'S', 1);
        (5, 2, 'Quini 2.2', 'S', 1);
        (6, 3, 'Quini 3.1', 'S', 1);
        (7, 3, 'Quini 3.2', 'S', 1);
        (8, 3, 'Quini 3.3', 'S', 3);
        (9, 3, 'Quini 3.4', 'S', 2);
        (10, 4, 'Quini 4.1', 'S', 2);
        (11, 4, 'Quini 4.2', 'S', 0);
        (12, 4, 'Quini 4.3', 'S', 0);
        (13, 5, 'Quini 5.1', 'S', 2);
        (14, 5, 'Quini 5.2', 'S', 2);
        (15, 6, 'Quini 6.1', 'S', 0);
        (16, 6, 'Quini 6.2', 'S', 1);
        (17, 6, 'Quini 6.3', 'S', 0);
        (18, 7, 'Quini 7.1', 'S', 1);
        (19, 7, 'Quini 7.2', 'S', 1);
        (20, 8, 'Quini 8.1', 'N', NULL);
        (21, 8, 'Quini 8.2', 'N', NULL);

Inserciones en la tabla PRONOSTICOS:
    
    (ID_QUINIELA, ID_PRO, ID_JORNADA, ID_EVENTO, PRONOSTICO):
        (1, 1, 1, 1, 'X');
        (1, 2, 1, 2, 'X');
        (1, 3, 1, 3, 'X');
        (2, 1, 1, 1, 'X');
        (2, 2, 1, 2, '1');
        (2, 3, 1, 3, '2');
        (3, 1, 1, 1, 'X');
        (3, 2, 1, 2, 'X');
        (3, 3, 1, 3, '2');
        (4, 1, 2, 1, '1');
        (4, 2, 2, 2, '2');
        (4, 3, 2, 3, '2');
        (5, 1, 2, 1, 'X');
        (5, 2, 2, 2, 'X');
        (5, 3, 2, 3, '2');
        (6, 1, 3, 1, 'X');
        (6, 2, 3, 2, '1');
        (6, 3, 3, 3, 'X');
        (7, 1, 3, 1, '2');
        (7, 2, 3, 2, '2');
        (7, 3, 3, 3, '2');

Informes o explotación de datos
    
   Al igual que para las inserciones, lo más normal de una aplicación es que disponga de alguna funcionalidad en las pantallas de gestión 
   que permita obtener informes. Esto también esta fuera del ámbito de este curso, así que lo haremos con consultas SQL directamente sobre 
   la BD.

   Usted debería estar capacitado para desarrollar esta parte de la aplicación, puesto que es lo que se ha estado trabajando durante todo 
   el curso. No voy a quitarle el protagonismo que merece y dejaré que ponga en práctica lo aprendido. Los informes o consultas a 
   desarrollar las encontrará a continuación, en el apartado de ejercicios. En esta lección, por ser la última, no se han publicado 
   las soluciones, aunque si los resultados de las consultas que se piden en los ejercicios para que puedan ser contrastados con los 
   resultados que usted obtenga.

   Sin embargo hay una cuestión que por la naturaleza de esta aplicación de ejemplo es preferible introducir. En los ejercicios se 
   le pedirá un tipo de consultas que requiere que aparezca dos veces la tabla EQUIPOS en la cláusula FROM. Esto es debido a que en 
   todo evento participan dos equipos, el local y el visitante. El modo de poder usar la misma tabla por duplicado en la cláusula from 
   es mediante alias de tabla, con ello se romper la ambigüedad del mismo modo que se hace con los campos de igual nombre pero de 
   tablas distintas. Veamos esto con un ejemplo:

   ¿Que calendario tiene en la competición el Xerez, equipo de identificador 2?:
   
    select J.ID_JORNADA,date_format(J.FECHA,'%d-%m-%Y') FECHA, L.EQUIPO LOCAL , V.EQUIPO  VISITANTE
        from JORNADAS J, EVENTOS E, EQUIPOS L, EQUIPOS V
        where J.ID_JORNADA  = E.ID_JORNADA
            and E.LOCAL       = L.ID_EQUIPO
            and E.VISITANTE   = V.ID_EQUIPO   
            and (E.LOCAL = 2 or E.VISITANTE = 2)
        order by J.FECHA

Ejercicio 1
    Desarrolle un informe que muestre los pronósticos de una quiniela, pruebe su funcionamiento con la quiniela de identificador 4.

Ejercicio 2
    Desarrolle un informe que muestre la combinación ganadora de una jornada, pruebe su funcionamiento con la jornada de identificador 3.

Ejercicio 3.1
    Desarrolle un informe que escrute una quiniela, es decir, que muestre los eventos en los que se acertó el resultado. Pruebe el 
    funcionamiento con la quiniela de identificador 6.
    Nota: Deberá usar la función IF para calcular la columna ACIERTO.

Ejercicio 3.2
    
   Tomando como patrón la consulta resultante del ejercicio 3.1, desarrolle una consulta que calcule los aciertos de las quinielas, 
   es decir, escrute las quinielas. Agrupe los datos por quiniela. Si una quiniela no tiene ningún acierto no es necesario que 
   aparezca en la lista resultante. 

   Añadale un filtro para poder calcular los aciertos de una quiniela concreta. Este dato es especialmente 
   útil para que un usuario, o un proceso automático, pueda actualizar el campo ACIERTOS de la tabla QUINIELAS, 
   que contiene un valor nulo hasta que se conocela la combinación ganadora y, en consecuencia, el dato a atualizar 
   una vez escrutada la quiniela. 

Ejercicio 3.3
    Desarrolle una consulta que calcule los aciertos de las quinielas pero esta vez considerando las quinielas que no presentan ningún acierto.

Ejercicio 4
    Desarrolle un informe que muestre la media de aciertos agrupado por jornada. No considere quinielas de jornadas no disputadas. 
    No es necesario recalcular los aciertos de las quinielas, en su lugar use el campo ACIERTOS de la tabla QUINIELAS.

Ejercicio 5
    Desarrolle un informe que muestre la media de aciertos agrupado por meses. No considere quinielas de jornadas no disputadas, 
    ni recalcule los aciertos de las quinielas.
    Nota: deberá utilizar la función DATE_FORMAT(FECHA_A_FORMATEAR,'%m-%Y') de MySQL para poder agrupar por mes-año.
