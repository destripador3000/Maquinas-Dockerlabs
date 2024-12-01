Esta es una máquina catalogada como muy fácil en dockerlabs. Primero desplegamos la máquina en nuestro entorno de docker:
```
sudo su bash auto_deploy.sh borazuwarahctf.tar
```
luego escaneamos el objetivo con nmap:

```
nmap -p- -sS -sC -sV -Pn -n --min-rate 5000 172.17.0.2
```
Luego encontramos los puertos 22 y 80 abiertos con el servicio ssh y http. No podemos vulnerar con enumeración de usuarios el ssh ya que esta en una versión muy actualizada.

![[borazuwarahctf1]](nmap.png)
Cuando ingresamos a la web de esta máquina encontramos una imagen, lo que nos puede dar a entender que hay que usar una técnica de [[Esteganografía]] para poder descargar la imagen:
```
wget http://172.17.0.2/imagen.jpg
```
una vez descargada la imagen nos daremos cuenta que necesitaremos passphrase para poder entrar. Si intentamos usar una nos dirá que revisemos el archivo secreto.txt. La cual nos dice que sigamos buscando en la imagen.
![[borazuwarahctf2]](ppp.png)
Para ello usaremos [[Exiftool]] 
![[borazuwarahctf3]](ffff.png)

Aquí ya sabemos que el usuario es borazuwarah, con el cual usaremos [[Hydra]] para hacer un ataque de fuerza bruta en ssh.
![[borazuwarahctf4]](nsnsns.png)
Como podemos observar el usuario borazuwarah con contraseña 123456 en el servicio ssh. Una vez adentro utilizamos comandos para ver que clase de binarios o comandos podemos ejecutar yo ejecute:
```
sudo -l
```
Con este comando encontramos que podemos ejecutar el binario de bash como sudo al pasarle sudo /bin/bash quedamos registrados como root:
![[borazuwarahctf5]](lop.png)
Ya con esto quedamos registrados como root y ya hemos finalizado esta máquina.
