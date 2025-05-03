19-4-2025
alo

NOTA: Para configurar los TEST PLANS con GUI correr "JMETER.BAT" o "JMETERW.CMD" para cargar APACHE JMETER con interfase gráfica 
(pero los tests deben correrse con linea de comandos. NO USAR GUI).  


Hice esta prueba en la DELL de vadeos instalando JMETER en E: (m.2 nvme drive) y usando la siguiente carpeta para INSTALAR Y EJECUTAR
APACHE JMETER después de copiar mis archivos de documentación de GIT y los archivos de documentacion (CON EXTENSION.txt) con cometarios como este
"E:\_borrables2\apache-jmeter-5.6.3\apache-jmeter-5.6.3\bin"

  Para correr APACHE JMETER en Windows hice lo siguiente:
1) Abri terminal de POWERSHELL en modo administrador.
2) me cambié al directorio: "E:\_borrables2\apache-jmeter-5.6.3\apache-jmeter-5.6.3\bin"
3) Copié los archivos de documentación y los .JMX con los TEST PLANS al directorio mencionado
4) Ejecuté JMETERW para ejecutar la versión de windows de APACHE JMETER sólo para poder chequear o cambiar los TEST PLANS 
para poner carga a la página web de AKS creada con contoso-website.
5) Chequeo y cambio de PLANES (por ejemplo dirección IP de la página web donde se haran pruebas de carga - load testing 
con los TEST PLANS. Se modificaron los planes y se guardan nuevos archivos .JMX .
6) Ejecuté en powershell las pruenas de carga con un comando como: ".\jmeter -n -t ".\test6.jmx" -l _ALO_results_file_6 -e -o .\_ALO_WRITE_FOLDER_6"
porque NO CONVIENE USAR LOS TESTS CON GUI SINO CON LINEA DE COMANDOS.
---------------------------------------------------------------------------------------------------------------------------------------
OJO - Se recomienda empezar a usar la carga de Apachejmeter para AKS con los siguientes parámetros en el THREAD GROUP DEL TEST PLAN:

1) number of threads (users) = 10.000
2) ramp-up period (seconds) = 30
3) loop count = 10
4) same user on each iteration = active (checked)

TEST 7 - corrié bien :
".\jmeter -n -t ".\test8.jmx" -l _ALO_results_file_8 -e -o .\_ALO_WRITE_FOLDER_8"


----------------------------------------------------------------------------------------------------------
 
---------------------------------------------------------------------------------------------------------------------------------------
OJO - Se recomienda empezar a usar la carga de Apachejmeter para AKS con los siguientes parámetros en el THREAD GROUP DEL TEST PLAN:

1) number of threads (users) = 10.000
2) ramp-up period (seconds) = 30
3) loop count = 10
4) same user on each iteration = active (checked)

TEST 7 - corrié bien :
".\jmeter -n -t ".\test7.jmx" -l _ALO_results_file_7 -e -o .\_ALO_WRITE_FOLDER_7"


----------------------------------------------------------------------------------------------------------
 
TEST 6 - - corrió bien :
".\jmeter -n -t ".\test6.jmx" -l _ALO_results_file_6 -e -o .\_ALO_WRITE_FOLDER_6"

----------------------------------------------------------------------------------------------------------
10-3-2025
alo
TEST 4 - corrió bien:
".\jmeter -n -t ".\mediatech.jmx" -l _ALO_results_file5 -e -o .\_ALO_WRITE_FOLDER5"

----------------------------------------------------------------------------------------------------------------
TEST 3:
- Me di cuenta que me estaban fallando las respuestas a la página web de mediatech (azure app service - alo)
y era porque en el URL de HTTP REQUEST DEFAULTS puse indebidamente un ESPACIO EN BLANCO.
----------------------------------------------------------------------------------------------------------------
TEST 2:
- Hice los cambios para correr la WEB APP DE MEDIATECH de AZURE APP tomando el TEST 1 y haciendo modificaciones
de FQDN y poniendo los HTTP REQUESTS 1 y 2 al mismo PATH: / (root) y al ejecutar el siguiente comando en el directorio
BIN de APACHE JMETER 5.6.3 me apareció error inicialmente corriendo lo siguiente:

".\jmeter -n -t ".\Mediatech Azure App Service WEB APP.jmx" -l _ALO_results_file2 -e -o .\_ALO_WRITE_FOLDER2"

Solucion:
1) Arregle una falla porque el archivo después del parámetro -l para el results file debe tener un nombre nuevo
diferente al nombre de una ejecución previa.
2) Me apareció luego el siguiente error: 
"Error in NonGUIDriver java.lang.IllegalStateException: Could not find the TestPlan class!"
Lo solucioné generando archivo .JMX con menu FLE-->SAVE TEST PLAN AS (en vez de "save selection as")

----------------------------------------------------------------------------------------------------------------
TEST 1:
.\jmeter -n -t .\_ALO_Test_Plan.jmx -l _ALO_results_file -e -o .\_ALO_WRITE_FOLDER

Ejecuté el comando ANTERIOR en powershell (después de HACER LO de MÁS ABAJO) PARA CORRER EL TEST: 

1) Me cambié a directorio: "C:\soporte\apache-jmeter-5.6.3\apache-jmeter-5.6.3\bin".
2) Leer el siguiente enlace: "https://jmeter.apache.org/usermanual/build-web-test-plan.html" para mi primera
prueba de "Building a Web Test Plan".
3) Combinar la documentación anterior con la documentación de inicio para principiantes que aparece en
archivo README.MD del directorio "C:\soporte\apache-jmeter-5.6.3\apache-jmeter-5.6.3".
4) ejecutar JMETER.BAT del directorio BIN para cargar la versión de GUI y CREAR TEST PLAN.
5) Crear TEST PLAN con las indicaciones del punto 2) y se salvó la configuración en el archivo "_ALO_Test_Plan.jmx".
6) En PS se ejecutó test con comando ".\jmeter -n -t .\_ALO_Test_Plan.jmx -l _ALO_results_file -e -o .\_ALO_WRITE_FOLDER"
y se guardarin resultados de la ejecución en carpeta  "_ALO_WRITE_FOLDER" (que no exitia antes de crear comando y debe
ser un nombre que se usa para crear carpeta nueva con resultados como una subcarpeta del directorio BIN)
7) Abrir archivo INDEX.HTML  de carpeta "_ALO_WRITE_FOLDER" con LOS RESULTADOS DEL TEST.

