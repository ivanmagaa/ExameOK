Contesta as seguintes preguntas, xustificándoas, nun README.md

1. Explica métodos para 'abrir' unha consola/shell a un contenedor en execución.

Unha das maneiras é, desde Docker-VStudio facer click dereito sobre o contenedor en execución na sección de "Containers" e seleccionar a opción de "Attach Shell".

2. No contenedor anterior (en execución), qué opciones tes que ter usado ó arrincalo para poder interactuar coas súas entradas e salidas

Para iso, debemos asignarlle os portos que debe empregar manualmente, por exemplo: 

```
docker run -d --name asir_httpd -p 8080:80 -v "$PWD"/htdocs:/usr/local/apache2/htdocs/ httpd

```
Neste caso, asignámoslle o porto `8000`


3. Cómo sería un ficheiro docker-compose para que dous contenedores se comuniquen entre si nunha rede só deles?

Deberíamos crear algún arquivo previo, pero o contido do docker-compose.yml que debemos ter para conectar dous contenedores entre eles, podería ser así:

```
services:  # Define os servizos (contenedores)
  
  <Nome_do_contedor>:
    image: <Imaxe_a_utilizar>
    container_name: <Nome_especifico>
    networks: 
      - <Nome_da_rede>
    command: tail -f /dev/null #Este comando fai que se mantenga activo o contenedor
  <Nome_do_contedor>:
    image: <Imaxe_a_utilizar>
    container_name: <Nome_especifico>
    networks: 
      - <Nome_da_rede>
    command: tail -f /dev/null #Este comando fai que se mantenga activo o contenedor

networks:  # Definición das redes
  <nome_da_rede>:  # Nome da rede que estamos a crear
    driver: <tipo_de_rede>  
```

4. Qué tes que engadir ó ficheiro anterior para que un contenedor teña unha IP fixa?

Na sección network de cada contenedor, deberíamos esta liña de comando para asignarlle a dirección IP que queiramos:

```

        ipv4_address: 172.18.0.3 #IP que lle otorgamos ao contenedor

```


5. Qué comando de consola podo usar para sabe-las ips dos contenedores anteriores?

Debemos executar o comando docker inspect <nome_da_rede>, a continuación debemos buscar no apartado `Containers` e a e a liña de `IPv4Address`.

6. Cál é a funcionalidade do apartado "ports" en docker compose?

Mapea os portos do `host:contenedor`

7. Para qué serve o rexistro CNAME? Pon un exemplo

Serve para definir alias para o nome canónico, é útil para suministrar nomes alternativos relacionados con diferentes servizos dentro do mesmo equipo. Por exemplo, se o resolutor intenta realizar resolución de nomes dun nome que indique o usuario, non sabe a priori se o nome refírese a un RR (A) de host o a un CNAME. Si se refire a un CNAME, o servidor pode devolver o CNAME.

8. Cómo podo facer para que a configuración dun contenedor DNS non se borre se creo outro contenedor?

No arquivo `ǹamed.conf` debemos engadir o seguinte:

docker run -d \
  --name dns-server \
  -v /path/to/your/dns/config:/etc/bind \
  -v /path/to/your/dns/data:/var/cache/bind \
  your-dns-image



10. Engade unha zoa tendaelectronica.int no teu docker DNS que teña:

Debemos modificar o arquivo `named.conf.local` para engadir a zoa.

- www á IP 172.16.0.1

Para facer isto debemos asignarlle direccións IP a os contenedores na rede `172.16.0.0`.

- owncloud sexa un CNAME de www
- un rexistro de texto có contido "1234ASDF"

Debemos crear o renomear un arquivo `db."".int` para engadir o rexistro de texto solicitado.

Comproba que todo funciona có comando "dig".

Para comprobalo debemos executar `docker compose up -d` e executar o comando `docker exec -it Exame1_alpine /bin/sh` e despois facer `dig @172.16.0.1 tendaelectronica.int`

Mostra nos logs que o servicio funciona ben usando a saída da terminal ó levantar o compose ou có comando "docker logs [nomeContenedorOuID]"
(o apartado 9 realízase na máquina virtual)
