# Entrega Docker D-02

<br>

<div>
<p style = 'text-align:center;'>
<img src="imagenes/uca.png" alt="JuveYell" width="3000px">
</p>
<center>

**Autor: Jonathan Muñoz Morales**

</center>
</div>

<br>

# Parte 1
## Introducción
En este breve documento encontrarás los comandos usados para resolver los siguientes ejercicios propuestos. Así como las respuestas de las preguntas en caso de que las hubiese.
## Crear un volumen compartido *volumenDocker* y modificar *index.html*
Como primer paso para realizar este paso, hemos tenido que iniciar sesión en docker, por lo que al ser un comando de la anterior práctica no lo incluiremos.
Una vez iniciada sesión, hemos de crear dicho volumen, para ello hemos hecho uso del siguiente comando:
```
	docker volume create volumenDocker 
```
Con ese comando hemos creado el volumen para la web con el nombre *volumenDocker*.
Una vez creado, debemos acceder:
```
	docker volume ls
```
Ahora crearemos un archivo *index.html* en el volumen compartido:
```csharp
	docker run -it --name volumenDocker-container -v volumenDocker:/usr/share/nginx/html nginx sh -c "echo '<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="utf-8">
  <title>Apache en Docker</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
</head>
<body>
  <h1>Hola Usuario! :)</h1>
</body>
</html>' > /usr/share/nginx/html/index.html"
```
En este comando se especifica el nombre del volumen creado en el primer paso y la ruta en la que se montará el contenedor Nginx. También se crea un archivo *index.html* con el contenido:
```html
	<!DOCTYPE html>
	<html lang="es">
	<head>
	  <meta charset="utf-8">
	  <title>Apache en Docker</title>
	  <meta name="viewport" content="width=device-width, initial-scale=1.0">
	</head>
	<body>
	  <h1>Hola Usuario! :)</h1>
	</body>
	</html>
```
## Crear un contenedor de Nginx que use el volumen *volumenDocker*

Para crear el contenedor Nginx que use el volumen creado anteriormente:

```
	docker run -d --name my_nginx -p 80:80 -v volumenDocker:/usr/share/nginx/html nginx
```

De esta forma, hemos creado un volumen compartido con un contenedor Nginx.

## Cree un segundo contenedor que también use el volumen *volumenDocker*

De la misma forma que el contenedor anterior:

```
	docker run -d --name segundoContenedor -p 81:80 -v volumenDocker:/usr/share/nginx/html nginx
```
Como evidencia de que ambos contenedores están ejecutandose y que podemos acceder a ambos, mostraremos las imágenes correspondientes a cada contenedor:

<br>

<div>
<p style = 'text-align:center;'>
<img src="imagenes/primer-contenedor.png" alt="JuveYell" width="3000px">
</p>
<center>

**Figura 1: http://localhost:80**

</center>
</div>

Como se puede observar en la **Figura 1**, podemos acceder al primer contenedor con el puerto por defecto (80).

Por otra parte, para el segundo contenedor tenemos la siguiente imágen:

<br>

<div>
<p style = 'text-align:center;'>
<img src="imagenes/segundo-contenedor.png" alt="JuveYell" width="3000px">
</p>
<center>

**Figura 2: http://localhost:81**

</center>
</div>

Si observamos detenidamente la ruta de la **Figura2**, podemos observar que la ruta marca es: localhost:81.
Por lo que hemos podido observar que podemos acceder a ambos y que ambos están activos.
# Parte 2
## Crea una nueva red *redDocker*
Para crear una nueva red llamada *redDocker* hemos usado el comando:
```csharp
	docker network create redDocker
```
## Crear un contenedor de Ubuntu *Ubuntu1*
El comando usado para crear un contenedor que se llame *Ubuntu1* de Ubuntu ha sido:
```csharp
	docker run -it --name Ubuntu1 ubuntu
```
## Crear un contenedor de Ubuntu *Ubuntu2*
Como el anterior comando, es idéntico:
```csharp
	docker run -it --name Ubuntu2 ubuntu
```
## Conectar *Ubuntu1* a la red *redDocker*
Para conectar el contenedor *Ubuntu1* a la red hemos usado el comando: 
```csharp
	docker network connect redDocker Ubuntu1
```
## Intentar hacer ping a *Ubuntu1* desde *Ubuntu2*
## ¿Funciona?
No, hemos usado el comando:
```csharp
	docker exec Ubuntu2 ping Ubuntu1
```
Y no ha funcionado. El error que nos ha mostrado ha sido el siguiente:

<br>

<div>
<p style = 'text-align:center;'>
<img src="imagenes/error.png" alt="JuveYell" width="3000px">
</p>
<center>

**Figura 3: Error al intentar hacer ping**

</center>
</div>

## ¿Por qué?
Debido a que ambos contenedores no están en la misma red, no pueden realizar un ping.
## Conectar *Ubuntu2* a la red *redDocker*
Como hicimos con *Ubuntu1*, hemos usado el comando:
```csharp
	docker network connect redDocker Ubuntu2
```
## Intentar de nuevo hacer ping a *Ubuntu1* desde *Ubuntu2*
## ¿Funciona?
Sí, al hacerlo empieza a enviar paquetes y a mostrarlos por pantalla.
## ¿Por qué?
Como comentamos antes, debían estar en la misma red para que dos contenedores puedan comunicarse entre sí.

