# Docker File Instructions

## FROM

Downloading the specific tag image from repository into the docker or kubernetes etc

```bash
FROM nginx:mainline-alpine-perl as base
```
also we can use the from command to copy from one image to other image.

```bash
FROM base as Final
```

## COPY

copy command is used to copy the files from your absolute or relative directory to the directory of the image.
in tis case its copying from relative location **`./SampleWebApp/`** to **`/usr/share/nginx/html`**

```bash
COPY ./SampleWebApp/ /usr/share/nginx/html
```

now in this example we are copying the files that are stored in an other image ***`base`*** image in path ***`/usr/share/nginx/html`*** to the ***`/src/`*** location.


```bash
COPY --from=base /usr/share/nginx/html /src/
```


## WORKDIR

Work Directory is to set the workdirectory then use variable multiple places like this.
Work Directory is not in yout local machine its inside the container.

```bash
FROM nginx:mainline-alpine-perl as base
WORKDIR /usr/share/nginx/html
COPY ./SampleWebApp/ .

#FROM base as Final
FROM nginx as Final
COPY --from=base /usr/share/nginx/html /src/
```

In the code above we are setting the workdir then using "." to say use workdir

> If directory do not exists then **`WORKDIR`** will create the directory.


## ARG & ENV

ARG are something where you can store some variables and use them later.
For Example

```bash
FROM nginx:mainline-alpine-perl as base
ARG build_configuration=Release
WORKDIR /usr/share/nginx/html
WORKDIR $build_configuration
COPY ./SampleWebApp/ .

#FROM base as Final
FROM nginx as Final
COPY --from=base /usr/share/nginx/html /src/
```

In the code above ARG is setting the **`build_configuration`** some data then using it later to create the directory using the **`WORKDIR`**.

> Even if we are setting **`build_configuration`** in the dockerfile but we can pass the **`build_configuration`** from docker build command to override that value.

> ARG - used for build time customization

```bash
docker build --build-arg some_name=a_value
```

> ENV - used for run time customizations.

```bash
docker run -e "env_var_name=some=value" imagename
```


## EXPOSE

We can write EXPOSE 80 or some port in docker file but its only for the documentation to let user know that we are exposing the port.

```bash
FROM nginx:mainline-alpine-perl as base
WORKDIR /usr/share/nginx/html
EXPOSE 80
COPY ./SampleWebApp/ .

#FROM base as Final
FROM nginx as Final
COPY --from=base /usr/share/nginx/html /src/
```

## RUN

Run command is used to build the dotnet web app or any other app in docker file.

```bash
FROM nginx:mainline-alpine-perl as base
WORKDIR /usr/share/nginx/html
EXPOSE 80
COPY ./SampleWebApp/ .

RUN dotnet build "DockerSampleApp.csproj"

#FROM base as Final
FROM nginx as Final
COPY --from=base /usr/share/nginx/html /src/
```

The Command **`RUN dotnet build "DockerSampleApp.csproj"`** will build the code for us and also we have a publish command to publish the code.
**`RUN dotnet publish "DockerSampleApp.csproj"`**


## ENTRYPOINT

We use this command to provide executables when the container is launched.


## Volumns

If you have some files and folders in your dotnet application like images you want those images should be copied in your local server, because if you restart the container all the images saved in your local will be remapped and end user will not suffer, that is an usecase of the docker volumes.

to create the volume we can run this command

```bash
docker run -d -v <volume path in local:app path in the container> --name samepl-web-container -p 9100:80 sample-web-app:1.0

docker run -d -v C:\dockerbackup:/user/share/nginx/dockerfiles --name samepl-web-container -p 9100:80 sample-web-app:1.0

```
now if you go to the container path 

```bash
docker exec -it <ContainerID> bash

cd /user/share/nginx/dockerfiles

ls

echo "Hello, I am Sumit Here!!!" > myfile.txt

exit

docker rm <ContainerID> -f

docker run -d -v C:\dockerbackup:/user/share/nginx/dockerfiles --name samepl-web-container1 -p 9200:80 sample-web-app:1.0

docker exec -it <ContainerID> bash

cd /user/share/nginx/dockerfiles

ls
```

if you do this exrcise you will see the files are intact in the local machine and not deleted even if the container is restarted.