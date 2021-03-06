Supported tags and respective `Dockerfile` links
================================================

* [`3.2.7`, `3.2`, `latest-3`, `latest` (Dockerfile)](https://github.com/reduardo7/docker-grails/blob/master/Dockerfile-grails3)
* [`3.1.15` (Dockerfile)](https://github.com/reduardo7/docker-grails/blob/3.1.15/Dockerfile-grails2)
* [`3.0.17` (Dockerfile)](https://github.com/reduardo7/docker-grails/blob/3.0.17/Dockerfile-grails2)
* [`2.5.5`, `2.5`, `latest-2`, (Dockerfile)](https://github.com/reduardo7/docker-grails/blob/master/Dockerfile-grails2)
* [`2.5.3` (Dockerfile)](https://github.com/reduardo7/docker-grails/blob/2.5.3/Dockerfile-grails2)
* [`2.4.5` (Dockerfile)](https://github.com/reduardo7/docker-grails/blob/2.4.5/Dockerfile-grails2)
* [`2.3.11` (Dockerfile)](https://github.com/reduardo7/docker-grails/blob/2.3.11/Dockerfile-grails2)
* [`2.2.5` (Dockerfile)](https://github.com/reduardo7/docker-grails/blob/2.2.5/Dockerfile-grails2)
* [`2.1.5` (Dockerfile)](https://github.com/reduardo7/docker-grails/blob/2.1.5/Dockerfile-grails2)
* [`2.0.4` (Dockerfile)](https://github.com/reduardo7/docker-grails/blob/2.0.4/Dockerfile-grails2)

Docker Hub
==========

[reduardo7/docker-grails](https://hub.docker.com/r/reduardo7/docker-grails)

# Optimized Grails Container #

Docker image that will bootstrap an environment for running a Grails application using Docker optimized base images. By default, it will run the Grails app using the `prod run-app` (or `prod run-war` for Grails 2) directive which is the most optimized way of running Grails for production environments. However, you can easily change the default behaviour for your specific uses (see the _Changing Behaviour_ section for more details on this).

## Technologies / Versions Used

- Grails 2/3
- Java JDK 7+ (for Grails 2) or 8+ (for Grails 3)

## Running Using Defaults

By default, the container will start inside Grails interactive mode. From here, you can run the app by simply typing `run-app` (Grails 2) or `run` (Grails 3) or execute any other valid Grails commands you may want to.

Use the following command to run on default mode (remember to ALWAYS specify your app folder in the `-v` command):

```bash
docker-compose build
docker-compose up app
```

### Environment Variables 

The image contains the following customizable Grails related environment variables that can be changed inside the image's Dockerfile.

 - `GRAILS_VERSION`: Specifies the version of Grails to download.

> **IMPORTANT:** You have two `Dockerfile`: When using `GRAILS_VERSION` env var `2.x` you need to use `Dockerfile-grails2`, and when using `GRAILS_VERSION` env var `3.x` you need to use `Dockerfile-grails3`.

## Building the Image

You can build the image by yourself by executing:

`docker-compose build`

## How to use for your Grails apps 

You can also leverage the use of this image for your internal apps if you want more freedom of customization and speed of initialization. To do this: 

 1. Create a Dockerfile for your app.
 2. Use this image as the `FROM:` image of your app's Dockerfile.
 3. Put your app's Dockerfile on the root of the app's folder.
 4. Build your image using your own custom Dockerfile.

An example of a `Dockerfile` for a _MyGrailsAPP_ app could be:

### For Grails 3:
```
FROM reduardo7/grails:3.2.7

# Copy App files
COPY . /app

# Run Grails dependency-report command to pre-download dependencies but not 
# create unnecessary build files or artifacts.
RUN grails dependency-report

# Set Default Behavior
ENTRYPOINT ["grails"]
CMD ["run"]
```

### For Grails 2:
```
FROM reduardo7/grails:2.5.3

# Copy App files
COPY . /app

# Run Grails refresh-dependencies command to 
# pre-download dependencies but not create
# unnecessary build files or artifacts.
RUN grails refresh-dependencies

# Set Default Behavior
ENTRYPOINT ["grails"]
CMD ["run-app"]
```

Then build your Docker image by executing:

`docker build -t "{repo-name}/MyGrailsAPP" .`

And finally run your app as a Docker container by executing:

`docker run -it -p 8080:8080 {repo-name}/MyGrailsAPP`

## References

 - More info about [phusion/baseimage](https://github.com/phusion/baseimage-docker).
 - More info about [Grails](https://grails.org/).

## Credits ##

This image was inspired by [mozart/grails-docker](https://github.com/mozart-analytics/grails-docker).
