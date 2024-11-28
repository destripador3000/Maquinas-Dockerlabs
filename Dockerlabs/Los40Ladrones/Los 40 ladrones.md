Esta es una máquina catalogada como fácil en dockerlabs. Lo primero que debemos hacer es cargar la máquina.
```
sudo bash auto_deploy.sh los40ladrones.tar
```
Luego de esto le hacemos un escaneo con nmap. 
![[primera]](los40ladrones.png)
Aquí podemos ver que esta abierto el puerto 80. Por lo que haremos una enumeración dominios con gobuster. que sinceramente en su inicio no logre encontrar nada ya que utilice, una búsqueda con diccionario que no encontró nada pero posteriormente vi un write up y vi que habían archivos txt en la web por lo que decidí añadirlo en la búsqueda de gobuster y encontré dicho archivo.
![[Segunda]](los40ladrones1.png)
Una vez ingresamos a este archivo encontramos algunas pistas como la posible existencia de un usuario llamado toctoc y tenemos algunos números que pueden especificar puertos o algo más. 
![[Tercera]](los40ladrones2.png)
Viendo en otros Writes Ups me di cuenta que la posible manera de utilizar estos números ya que al revisarlo con el comando:
```
nmap --top-ports 25T -n 172.17.0.2
```
No salen en puertos filtrados entonces una manera buena de poder verlos abiertos o filtrados es con el comando:
```
kcnok 172.17.0.2 7000 8000 9000 -v
```
Una vez realizada la ejecución de este comando podemos volver a hacer un escaneo en la red y encontraremos el puerto ssh abierto.
![[Cuarta]](los40ladrones23.png)
Después de un tiempo intentando con hydra se logro encontrar la contraseña.
![[Quinta]](los40ladrones4.png)
Una vez dentro probe bastantes comandos y con el comando:
```
sudo -l
```
me di cuenta que podía ejecutar una consola bash como root en la ruta /opt/bash, entonces ejecutando:
```
sudo /opt/bash
```
Logramos escalar privilegios y ya quedaríamos registrados como usuario root.
![[Sexta]](los40ladrones5.png)
Con esta escalada de privilegios ya habríamos acabado la máquina.
