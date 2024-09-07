# First Docker file

1. create a html page "index.html" inside "SampleWebApp" folder and add some html there.
2. Now add a "dockerfile" just outside the "SampleWebApp" folder and add nginx image and copy your files.


If you see official documentation you will find the details.

https://hub.docker.com/_/nginx



![alt text](image.png)


# Building the docker image.

* cd to the folder where docker file is kept
* then run command "docker build -t SampleWebApp ." where -t is the tag you want to add to the image.
* the last "." tells the path of the dockerfile currently its in the same directory.
* repository name should be in lower case
* you can provide the image tag in the command "docker build -t SampleWebApp:1.0.0 .", but if you do not provide it will take latest as tag

