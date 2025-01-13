# Preparativos

**Crear la carpeta donde almacenar el docker-compose.yaml**   
<code>mkdir odoo</code>      
<code>cd odoo</code>

**Crear el archivo docker-compose.yaml**   
<code>nano docker-compose.yml</code>

~~~
services:
  web:
    image: odoo:17.0
    depends_on:
      - db
    ports:
      - 8069:8069
    volumes:
      - odoo-web-data:/var/lib/odoo
      - ./config:/etc/odoo
      - ./addons:/mnt/extra-addons
    environment:
      - HOST = db
      - USER = odoo
      - PASSWORD = odoo
  
  db:
    image: postgres:15
    environment:
      - POSTGRES_DB=postgres
      - POSTGRES_PASSWORD=odoo
      - POSTGRES_USER=odoo
      - PGDATA=/var/lib/postgresql/data/pgdata
    volumes:
      - odoo-db-data:/var/lib/postgresql/data/pgdata
    ports:
      - 5432:5432
  
  pgadming:
    image: dpage/pgadmin4
    environment:
      PGADMIN_DEFAULT_EMAIL: efernandezcostas@danielcastelao.org
      PGADMIN_DEFAULT_PASSWORD: odoo
    ports:
      - 5050:80
    
volumes:
  odoo-web-data:
  odoo-db-data:

~~~

# Instalación
**Lanzar el docker compose**   
<code>docker compose up -d</code>

*Para eliminarlo y limpiar los volúmenes de datos:*    
<code>docker compose down -v</code>

**Comprobar que se crearon los contenedores y están en ejecución**   
<code>docker ps -a</code>

~~~
CONTAINER ID   IMAGE                          COMMAND                  CREATED          STATUS                      PORTS                                                      NAMES
e5bed0d04321   odoo:17.0                      "/entrypoint.sh odoo"    22 minutes ago   Up 22 minutes               0.0.0.0:8069->8069/tcp, :::8069->8069/tcp, 8071-8072/tcp   odoo-web-1
97b6eee0f1af   dpage/pgadmin4                 "/entrypoint.sh"         22 minutes ago   Up 22 minutes               443/tcp, 0.0.0.0:5050->80/tcp, [::]:5050->80/tcp           odoo-pgadming-1
5f00ab5d0b16   postgres:15                    "docker-entrypoint.s…"   22 minutes ago   Up 22 minutes               0.0.0.0:5432->5432/tcp, :::5432->5432/tcp                  odoo-db-1
~~~

# Configuración
## PgAdmin
**Entrar a la página desde el navegador** *\<ip\>:\<puerto\>*   
![odoo3](https://github.com/user-attachments/assets/193fff01-d12e-4a38-ac1c-869423b0a901)

## Odoo
**Entrar a la página desde el navegador** *\<ip\>:\<puerto\>*   
![odoo1](https://github.com/user-attachments/assets/1d218df8-898e-4dae-a8c7-8bf84e3a8064)

**Tras poner los datos condigurados en docker-compose y una nueva base de datos:**   
![odoo4](https://github.com/user-attachments/assets/4a9e83e0-e1b3-4dbe-83ce-5d060f317539)


# Preguntas
**¿Que ocurre si en el ordenador local el puerto 5432 está ocupado? ¿Y si lo estuviese el 8069?**   
Al lanzar el contenedor con *docker compose up* daría error ya que los puertos ya estarían siendo utilizados.   

**¿Cómo puedes solucionarlo?**   
Podemos solucionarlo cambiando los puertos en el docker-compose.yaml, por ejemplo poniendo 5433 y/o 8070







