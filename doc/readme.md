# Flask y Gunicorn

<p align="left">
<img src="https://img.shields.io/badge/STATUS-FINALIZADO-blue">
</p>

## Índice

1. [Prerrequisitos](#prerrequisitos)
2. [Despliegue](#despliegue)
3. [Tarea de ampliación](#tarea-de-ampliación)

---


## Prerrequisitos

Deberemos tener actualizado el sistema operativo para instalar los paquetes necesarios y pip.

```
sudo apt-get update && sudo apt-get install -y python3-pip
```

![pre requisitos](../doc/img/cp1.png)

Para instalar pipenv deberemos ejecutar el siguiente comando:

```
pip3 install pipenv
```

![pre requisitos](../doc/img/cp2.png)

Para comprobar que esta instalado ejecutaremos el siguiente comando:

```
pipenv --version
```

![pre requisitos](../doc/img/cp3.png)

Para instalar python-dotenv deberemos ejecutar el siguiente comando:

```
pip3 install python-dotenv
```

![pre requisitos](../doc/img/cp4.png)


## Despliegue

Para alamcenar nuestro proyecto crearemos la siguiente carpeta con el comando:

```
sudo mkdir -p /var/www/app
```

![pre requisitos](../doc/img/cp5.png)

Le establecemos el propietario con el comando:

```
sudo chown -R $USER:www-data /var/www/app
```

Y le damos los permisos necesarios con el comando:

```
sudo chmod -R 775 /var/www/app
```

![pre requisitos](../doc/img/cp6.png)

Creacion de nuevo archivo oculto .env con el comando:

```
sudo nano /var/www/app/.env
```

![pre requisitos](../doc/img/cp7.1.png)


Haremos nano para hacer la configuracion del archivo .env


![pre requisitos](../doc/img/cp7.png)

Que sera algo como esto:

```
FLASK_APP=wsgi.py
FLASK_ENV=production
```

Abriremos la consola pipenv con el comando:

```
pipenv shell
```

![pre requisitos](../doc/img/cp8.png)

Usamos pipenv install flask gunicorn para instalar las dependencias necesarias:

![pre requisitos](../doc/img/cp9.png)

Para crear la aplicacion Flask crearemos el archivo application.py con el comando:

```
touch /var/www/app/application.py
```

![pre requisitos](../doc/img/cp10.png)

Ahora editaremos el archivo application.py con el comando:
(Esto estando dentro del directorio /var/www/app)

```
sudo nano application.py
```

![pre requisitos](../doc/img/cp11.png)

La aplicacion sera algo como esto:

```python
from flask import Flask

app = Flask(__name__)

@app.route('/')
def index():
    return 'Hello, World!'
```


Y el contenido del archivo wsgi.py sera algo como esto:

```python
from application import app

if __name__ == '__main__':
    app.run()
```
![pre requisitos](../doc/img/cp12.png)

Y para desplegarlo deberemos ejecutar el siguiente comando:

```
pipenv run flask run --host '0.0.0.0'
```

![pre requisitos](../doc/img/cp13.png)

Lo abriremos en el navegador y se verá algo como esto:

![pre requisitos](../doc/img/cp14.png)

Y ahora para desplegarlo con gunicorn deberemos ejecutar el siguiente comando:

```
gunicorn --workers 4 --bind 0.0.0.0:5000 wsgi:app
```

![pre requisitos](../doc/img/cp15.png)

Lo abriremos en el navegador y se verá algo como esto:

![pre requisitos](../doc/img/cp16.png)


Y ahora para poder configurar un servicio del sistema tendremos que obtener la ruta desde la que se ejecuta el gunicorn:

```
which gunicorn
```

![pre requisitos](../doc/img/cp17.png)

Y ahora iniciaremos nginx con el comando:

```
sudo systemctl start nginx
```

![pre requisitos](../doc/img/cp18.png)

Y ahora para que corra el gunicorn como servicio del sistema crearemos el archivo flask_app.service con el comando:

```
sudo nano /etc/systemd/system/flask_app.service
```

![pre requisitos](../doc/img/cp19.png)


Informaremos de que hay un nuevo servicio con el comando:

```
sudo systemctl daemon-reload
```

![pre requisitos](../doc/img/cp20.png)

Y ahora lo iniciaremos con el comando:

```
sudo systemctl enable flask_app
```

```
sudo systemctl start flask_app
```

Para hacer la configuracion de nginx haremos el siguiente comando:

```
sudo nano /etc/nginx/sites-available/app.conf
```

![pre requisitos](../doc/img/cp21.png)

Crearemos un enlace simbolico con el comando:

```
sudo ln -s /etc/nginx/sites-available/app.conf /etc/nginx/sites-enabled/
```

Y ahora lo comprobaremos con el comando:

```
ls -l /etc/nginx/sites-enabled/ | grep app.conf
```

![pre requisitos](../doc/img/cp22.png)

Para comprobar que la configuracion de nginx es correcta ejecutaremos el comando:

```
sudo nginx -t
```

![pre requisitos](../doc/img/cp23.png)

Y ahora rearrancaremos nginx con el comando:

```
sudo systemctl restart nginx
sudo systemctl status nginx
```

![pre requisitos](../doc/img/cp24.png)

Deberemos poner esta ruta en el archivo hosts en windows para previsualizar la aplicacion:

```
192.168.X.X app.izv www.app.izv
```

![pre requisitos](../doc/img/cp25.png)


Y se verá en la web así:


![pre requisitos](../doc/img/cp26.png)


## Tarea de ampliación

Para hacer ahora la tarea de ampliación clonaremos el repositorio de github con el comando en el directorio /var/www:

```
git clone https://github.com/Azure-Samples/msdocs-python-flask-webapp-quickstart
```

![pre requisitos](../doc/img/cp27.png)

Cambiremos los permisos y el propietario con el comando:

```
sudo chown -R $USER:www-data /var/www/msdocs-python-flask-webapp-quickstart/
sudo chmod -R 775 /var/www/msdocs-python-flask-webapp-quickstart/
```

![pre requisitos](../doc/img/cp28.png)

Ahora crearemos el archivo .env con el comando:

```
sudo nano msdocs-python-flask-webapp-quickstart/.env
```

![pre requisitos](../doc/img/cp29.png)

Y el contenido del archivo sera algo como esto:

```
FLASK_APP=application.py
FLASK_ENV=production
```

Ahora entraremos en el directorio de nuestra app e iniciaremos la shell de pipenv con el comando:

```
cd msdocs-python-flask-webapp-quickstart
pipenv shell
```

![pre requisitos](../doc/img/cp30.png)

Instalamos los requisitos con el comando:

```
pipenv install -r requirements.txt
```

![pre requisitos](../doc/img/cp31.png)

Instalamos flask y gunicorn con el comando:

```
pipenv install flask gunicorn
```

![pre requisitos](../doc/img/cp32.png)

Lanzar app:

![pre requisitos](../doc/img/cp34.png)

Pruebas de comprobacion:

![pre requisitos](../doc/img/cp35.png)

Y ahora para desplegarlo con gunicorn deberemos ejecutar el siguiente comando:

```
gunicorn --workers 4 --bind 0.0.0.0:5000 wsgi:app
```

![pre requisitos](../doc/img/cp36.png)
