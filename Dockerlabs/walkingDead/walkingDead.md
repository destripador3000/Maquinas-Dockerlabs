
Esta es una máquina fácil de dockerlabs. Empezamos como siempre desplegando esta máquina:
![[walkingDead]](walkingDead1.png)

Luego de eso le hacemos un escaneo de puertos encontrando el puerto 80 abierto y el puerto 22, el puerto 80 quiere decir que hay una página web corriendo por el servicio http:
![[walkingDead]](walkingDead2.png)

La página que encontramos es la siguiente:
![[walkingDead]](walkingDead3.png)

Utilizando la herramienta gobuster podemos ver un directorio llamado hidden:
![[walkingDead]](walkingDead4.png)

Si analizamos el código fuente de la máquina encontraremos el mismo directorio y un archivo .shell.php en el
![[walkingDead]](walkingDead5.png)

Al entrar en la ruta del archivo a simple vista no aparece nada pero esto nos da indicio de una webshell
![[walkingDead]](walkingDead6.png)

Al probar el comando whoami podremos notar que efectivamente es una webshell.
![[walkingDead]](walkingDead7.png)

Esto quiere decir que podremos hacer una reverse shell, pero primero tendremos que "urlecodear"
nuestra shell para que nos la pueda interpretar la barra de busqueda.
![[walkingDead]](walkingDead8.png)

Nos ponemos a la escucha por el puerto 444 con netcat y darle enter a nuestra shell en el buscador (En ese orden) Estaremos dentro de la máquina víctima.
![[walkingDead]](walkingDead9.png)

Posterior a esto hacemos un tratamiento de tty
![[walkingDead]](walkingDead10.png)
Encontre un archivo python, en el cual me quede estancado tratando de modificarlo y buscando otras maneras de escalar privilegios pero no funciono.
![[walkingDead]](walkingDead11.png)

Luego de esto decidí buscar que binarios podría ejecutar y como se puede observar se puede ejecutar python:
![[walkingDead]](walkingDead12.png)
Así que decidí buscar en la página de GTFObins y encontre el SUID para escalar privilegios:
![[walkingDead]](walkingDead13.png)

Con esto habremos acabado la máquina Walking Dead.
