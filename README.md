Guia para utilizar el job JENKINSFILE-PRACTICA.

PASO NUMERO 1 
Empezamos definiendo LOGIN y PASSWORD.
Seguido iniciamos el pipeline y le pasamos los parametros “name” y “surname” que van a formar nuestro LOGIN.
seguido, nuestro último parámetro será choice que nos dará la opción de elegir el departamento donde ubicamos  a nuestro usuario.

Comenzamos con los STAGES siendo el primero para la creación del usuario y asignarle un grupo.
En este paso se concatenan los parámetros NAME y SURNAME para formar el LOGIN.
la siguiente ejecución será la creación del usuario, la cual se realiza mediante el comando  “sudo useradd -m -c” y le pasamos los parámetros NAME y SURNAME seguido ubicamos al usuario en un departamento con el parámetro DEPARTAMENT más nuestro LOGIN.
Por último en este paso asignamos el usuario a un departamento utilizando el comando “sudo usermod -aG” junto con el parámetro DEPARTAMENT y nuestro LOGIN ya formado.

PASO NUMERO 2 

Comenzamos con el paso número 2 la generación de una contraseña.
A lo anteriormente definido como PASSWORD le voy a pasar el script para que ejecute en la consola el comando “openssl rand -base64 12” que lo que nos dará va a ser una contraseña aleatoria.
Siguiente la asignamos la contraseña aleatoria-temporal al usuario con los siguientes comando a ejecutar en consola:
"echo '${LOGIN}:${PASSWORD}' | sudo chpasswd" 
"sudo passwd -e ${LOGIN}"
Con esto ya asignamos la 
contraseña a nuestro usuario.

PASO NUMERO 3

Este es el último paso, la visualización de la contraseña que le asignaremos al usuario.
Lo lograremos haciendo un ECHO pasandole nuestro LOGIN que mostrará el nombre y apellido del usuario junto con PASSWORD que nos enseñará la contraseña aleatoria.

