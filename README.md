# PHP Docker with XDEBUG
Source:
https://medium.com/@jasonterando/debugging-with-visual-studio-code-xdebug-and-docker-on-windows-b63a10b0dec

## Prerequisites
- PHP 7.3
- Docker for windows: https://docs.docker.com/docker-for-windows/
- Docker-compose: https://docs.docker.com/compose/install/

Make sure you share your drive (of this project folder) with docker:  
`Settings => Shared Drives`


### Using Visual Studio Code
For PHP with Visual Studio Code as IDE, install:
- Visual Studio Code: https://code.visualstudio.com/
- Install VSC extensions PHP IntelliSense and PHP Debug by Felix Becker
- Add debug configuration for PHP:
```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Listen for XDebug",
            "type": "php",
            "request": "launch",
            "port": 9000,
            "pathMappings": {
                "/var/www/html": "${workspaceFolder}/src"
            },
            "xdebugSettings": {
                "max_data": 65535,
                "show_hidden": 1,
                "max_children": 100,
                "max_depth": 5
            }
        }
    ]
}
```
### PHPStorm
TODO
https://thecodingmachine.io/configuring-xdebug-phpstorm-docker

## Start

### docker-compose.yml
Make sure the the port config used in the environmental variable XDEBUG_CONFIG in docker-compose.yml matches the port XDEBUG is listening to:
```yml
XDEBUG_CONFIG: remote_host=host.docker.internal remote_port=9000 remote_enable=1
```

Make sure the port you use to expose the container is not in use already. For instance, in the example below we use port 1880 to expose the container. 
```yml
    ports:
      - '1880:80'
```

### Run
Open a terminal in the same folder as the Dockerfile and run
```bash
docker-compose up
```

And after that, browse to localhost:1880 (in our example)