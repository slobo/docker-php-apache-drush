[azukiapp/php-apache](http://images.azk.io/#/php-apache)
==================

Base docker image to run **PHP** applications in [`azk`](http://azk.io)

Versions (tags)
---

<versions>
- [`latest`, `5`, `5.6`, `5.6.3`](https://github.com/azukiapp/docker-php-apache/blob/master/5.6/Dockerfile)
- [`5.5`, `5.5.9`](https://github.com/azukiapp/docker-php-apache/blob/master/5.5/Dockerfile)
</versions>

Image content:
---

##### PARENT:
- Ubuntu 14.04
- Git
- Vim
- Node
- MySQL Client
- PostgreSQL Client
- MongoDB
- Apache2 with root

###### THIS:
- PHP Version 5.6/5.5
- Composer

### Usage with `azk`

Example of using this image with [azk](http://azk.io):

```js
/**
 * Documentation: http://docs.azk.io/Azkfile.js
 */
 
// Adds the systems that shape your system
systems({
  "my-app": {
    // Dependent systems
    depends: [], // postgres, mysql, mongodb ...
    // More images:  http://images.azk.io
    image: {"docker": "azukiapp/php-apache"},
    // Steps to execute before running instances
    provision: [
      // "composer install",
    ],
    workdir: "/azk/#{manifest.dir}",
    shell: "/bin/bash",
    command: "# command to run app",
    wait: {"retry": 20, "timeout": 1000},
    mounts: {
      '/azk/#{manifest.dir}': path("."),
    },
    scalable: {"default": 2},
    http: {
      // my-app.dev.azk.io
      domains: [ "#{system.name}.#{azk.default_domain}" ]
    },
    ports: {
      // exports global variables
      http: "80/tcp",
    },
    envs: {
      // set instances variables
      APP_DIR: "/azk/#{manifest.dir}",
    },
  },
});
```

### Usage with `docker`

To create the image `azukiapp/php-apache`, execute the following command on the docker-php-apache folder:

```sh
$ docker build -t azukiapp/php-apache 5.6/
```

To run the image and bind to port 80:

```sh
$ docker run -it --rm --name my-app -p 80:80 -v "$PWD":/myapp -w /myapp azukiapp/php-apache php index.php
```

#### To run the sample project:

Start your image binding external port 80 in all interfaces to your container:

```sh
$ docker run -d -p 80:80 azukiapp/php-apache
```

Test your deployment:

```sh
$ curl http://localhost/

Hello world!
```

Logs
---

```sh
# with azk
$ azk logs my-app

# with docker
$ docker logs <CONTAINER_ID>
```

## License

Azuki Dockerfiles distributed under the [Apache License](https://github.com/azukiapp/dockerfiles/blob/master/LICENSE).
