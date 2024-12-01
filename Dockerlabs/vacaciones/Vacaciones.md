Está máquina es catalogada como muy fácil. Primero la desplegamos en nuestro entorno de docker:
```
sudo bash auto_deploy.sh vacaciones.tar
```
Luego le hacemos un escaneo con nmap:
```
nmap -p- -sS -sC -Pn -n --min-rate 5000 172.17.0.2
```
Una vez realizado el escaneo encontramos los puertos 22 y 80 abiertos, corriendo el servicio ssh y http respectivamente.
![[1]](vacaciones1.png)
Como podemos ver el servicio ssh es un poco anticuado por lo que es propenso a enumeración de usuarios. Intente enumerar con un script pero hay algunos que me siguen dando errores para poder ejecutarlos. Tener en cuenta que tuve que ver algunos write-ups en los que me di cuenta que no había cargado bien mi máquina ya que todos hablaban de un comentario en el código fuente, cosa que yo no veía y decidí reiniciarlo, algo que funciono de maravilla.
![[2]](vacaciones2.png)
Como podemos ver esta el usuario juan y camilo cosa que utilizaremos para entrar al servidor ssh.
Al utilizar [[Hydra]] para encontrar la contraseña para el servicio ssh con el usuario juan tardo demasiado por lo que decidí hacerlo con el usuario Camilo.
![[3]](vacaciones3.png)
Como se puede observar decidí quitar el argumento -t 64 ya la ejecución de este ataque también estaba tardando un poco. Quitando el argumento el hallazgo de la contraseña fue muy rápido.
Una vez dentro tener siempre en cuenta lo que leemos en los comentarios ya que como decía el comentario el usuario juan le había escrito un mail al usuario camilo. Al revisar el mail encontramos la contraseña del usuario juan.
![[4]](vacaciones4.png)
Aquí podemos ver la contraseña y ya podemos acceder al usuario juan. con este usuario si podemos utilizar el comando:
```
sudo -l
```
encontrando que podemos ejecutar ruby con privilegios de sudo.
Luego de darnos cuenta de esto buscando en [[GTFObins]] nos damos cuenta que con el comando:
```
sudo ruby -e 'exec "/bin/sh"'
```
podremos escalar privilegios. Cosa que efectivamente paso.
![[5]](vacaciones5.png)
Como se puede ver ya pudimos acceder a la máquina y pudimos escalar privilegios. 
Esta máquina es interesante ya que la encontré un poco desafiante tal vez por la confusión de dos usuarios y también por las trabas que tuve al principio de iniciarla.
