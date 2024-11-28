Máquina catalogada como muy fácil, primero le hacemos un escaneó de puertos:
```
nmap -p- -sS -sC -Pn -n -vvv --min-rate 5000 172.17.0.2
```
Encontramos 3 puertos con tres servicios corriendo en cada uno de ellos:
![[obsession1.png]]
Los cuales tienen versiones actualizadas que al parecer con [[Searchsploit]] no se logro encontrar ninguna vulnerabilidad. Así que hacemos una enumeración de dominios con [[Gobuster]], encontrando los subdominios /backup e /important:
![[obsession2.png]]
En el subdominio /important encontre un manifiesto que si bien no parece decir nada puede ser útil para más adelante:
![[obsession3.png]]
Mientras tanto en el subdominio backup encontre un archivo que dice tener el usuario para todos los servicios:

![[obsession4.png]]
Si esto es cierto podremos atacar el servicio ssh y el ftp primero porbare un ataque de fuerza bruta con [[Hydra]] por el servicio ssh. Encontrando que la contraseña para el usuario russoski en el servicio ssh es iloveme:
![[obsession5.png]]

Cabe recalcar que como he hecho bastantes máquinas anteriores he tenido porblemas con el ssh una de las soluciones para este problema fue:
![[obsession6.png]]
una vez dentro de la máquina con el comando:
```
sudo -l 
```
Puedo observar que comandos puedo ejecutar como root y así ver como puedo escalar privilegios.
al ejecutarlo pude observar que no necesito contraseña de root para ejecutar el comando /usr/bin/vim y con esta información me pude dirigir a la página [[GTFObins]] y pude ver que con el exploit:
```
sudo vim -c ':!/bin/sh'
```
Podría ingresar como root. Al ejecutarlo pude ver que efectivamente si funciono.
![[obsession7.png]]
Y con esto hemos finalizado la máquina obsession.
Leyendo los WriteUps de otros usuarios de dockerlabs me di cuenta que es necesario entrar al servicio FTP para obtener 2 archivos que allí se encuentran. Estos archivos tratan de la conversación de dos personas sobre entrenos en el gimnasio sin embargo el objetivo principal de la máquina la hemos conseguido, llegar a root pero es importante tener en cuenta este detalle para acciones posteriores en distintas máquinas. 
Tener en cuenta de que si se hubiera querido entrar por el servicio ftp primero ejecutar:
```
ftp 172.17.0.2
```
Posteriormente hubiermos necesitado un usuario y una contraseña válidos:
```
usuario: anonymous
contraseña: anonymous
```
Estas dos credenciales son usadas ya que son las más comunes pero puede que no sean solo esas.

