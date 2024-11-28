Esta es una máquina catalogada como muy fácil. Lo primero que hacemos es desplegar nuestro entorno de dockerlabs.
```
sudo bash auto_deploy.sh hedgehog.tar
```
Luego hacemos un escaneo de puertos:
```
nmap -p- -n -Pn -sS -sC -sV --min-rate 5000 172.17.0.2
```
![[hedgeho1.png]]
Descubrimos que en la parte principal de la página hay un apartado que dice "tails"
![[hedgeho2.png]]
Investigando un poco descubrí que si se hace un ataque de fuerza bruta pero con un pequeño truco invirtiendo nuestro diccionario rockyou.txt esto se hace con el comando tac:
```
tac rockyou.txt >> invertido.txt
```
Si bien el archivo invertido.txt ya inveirtio nuestro diccionario rockyou esto será inútil ante hydra ya que este archivo contiene palabras con espacios. Para eliminar los espacios podemos usar el comando sed:
```
sed -i 's/ //g' invertido.txt
```
De esta manera ya quedara invertido el archivo y podra ser utilizado por hydra
![[hedgeho3.png]]
Como se puede observar ya funciono y logramos encontrar la contraseña.
![[hedgeho4.png]]
![[hedgeho5.png]]
con sudo -l podremos observar como con el usuario sonic podremos ejecutar cualquier comando sin la necesidad de una contraseña. Por lo que ejecutando el comando:
```
sudo -u sonic /bin/bash
```
Nos encontraremos loggeados como sonic y luego ejecutando el comando:
```
sudo su
```
Quedaremos registrados como root. Y ya con eso acabaríamos esta máquina. 
