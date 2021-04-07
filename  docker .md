# Docker
[<TIL](Programming.md)

## node_modules directory not accessible after npm packages installation
Because we bind $HOME/app inside the container to the application folder
on the host. Unfortunately, the node_modules folder doesn’t exist there on the host,
so this bind effectively ‘hides’ the node modules that we installed.

There are several ways around this problem, but I think the most elegant is to use
a volume within the bind to contain node_modules. To do this, we just have to add
this one line to the end of our docker compose file:
```
volumes:
    - . :/app
    - /app/node_modules
```

[Source](https://jdlm.info/articles/2016/03/06/lessons-building-node-app-docker.html)

## Run commands inside the docker while outside it
`docker exec container_name bash -c "php composer.phar dump-autoload"`
