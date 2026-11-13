# docker-compose
##1. ¿Por qué el frontend no necesita conectarse a PostgreSQL? 
Porque el frontend solo se encarga de mostrar la interfaz visual al usuario. Para obtener o guardar datos, le envía peticiones al backend. Es el backend quien tiene la lógica, los permisos y la responsabilidad de conectarse a la base de datos de forma segura.

##2. ¿Por qué el backend pertenece a dos redes?
Porque actúa como un intermediario. Necesita estar conectado al frontend para escuchar las peticiones que le envía la página web, y a la vez necesita estar en la red datos para poder consultar o guardar esa información en la base de datos PostgreSQL.

##3. ¿Qué servicio necesita publicar un puerto en el anfitrión?
Solo el frontend, es el único servicio al que un usuario necesita acceder. El backend y la base de datos se comunican internamente y no necesitan exponerse al exterior.

##4. ¿Por qué publicar PostgreSQL hacia fuera no es la solución adecuada para un fallo de comunicación interno?
Por seguridad, ya que si publicas el puerto de la base de datos hacia el exterior, estás abriendo una puerta para que cualquiera intente acceder a los datos. Los fallos de comunicación interna se deben solucionar revisando la configuración de las redes de Docker, no eliminando las barreras de seguridad.

##5. ¿Qué ventaja ofrece Docker Compose respecto a ejecutar los tres contenedores manualmente?
Automatización y evitar errores. En lugar de escribir a mano comandos largos para crear redes, volúmenes y cada contenedor uno por uno, Docker Compose te permite guardar toda esa infraestructura como código en un solo archivo. Así puedes levantar, apagar o replicar todo el proyecto con un solo comando.

##6. ¿Por qué hemos dividido la arquitectura en dos redes?
Para aislar los servicios y mejorar la seguridad. Al separarlos en frontend y datos, garantizamos que la web jamás pueda ver ni hablar directamente con la base de datos. Si un atacante lograra vulnerar el frontend, no tendría un camino directo por red hacia donde se guarda la información valiosa.

##7. ¿Por qué el backend utiliza `db` como dirección de PostgreSQL?
Porque Docker tiene su propio servidor DNS interno. Las direcciones IP de los contenedores cambian cada vez que se reinician, por lo que usar una IP sería inestable. Docker soluciona esto permitiendo que los contenedores dentro de una misma red se encuentren utilizando directamente el nombre del servicio que defines en el archivo.

##8. ¿Qué diferencia existe entre detener un proyecto y eliminar sus volúmenes?
Detener un proyecto solo apaga y destruye los contenedores. Si eliminas los volúmenes, estás destruyendo el disco duro virtual donde se guardaban los datos de manera permanente.

##9. ¿Qué consecuencias tendría para la empresa perder el volumen de PostgreSQL?
Significaría una pérdida total de información. Se borrarían todas las incidencias registradas, los usuarios y el historial de la aplicación. Al arrancar de nuevo, la base de datos estaría completamente en blanco.
