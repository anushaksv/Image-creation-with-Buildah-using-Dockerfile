# Image-creation-with-Buildah-using-Dockerfile

Buildah is an open source, Linux-based tool that can build Docker and Kubernetes-compatible images.


## Description:-

Buildah is a command-line tool for creating Open Container Initiatives (OCI) or traditional Docker images, without a full container runtime or daemon installed. Here I have created an image using Dockerfile with buildah on Ubuntu.


## Pre-requisites:-

The buildah package is available in the official repositories for Ubuntu 20.10 and newer.


## Installation steps:-


```html
sudo apt-get -y update
. /etc/os-release
sudo sh -c "echo 'deb http://download.opensuse.org/repositories/devel:/kubic:/libcontainers:/stable/x${ID^}_${VERSION_ID}/ /' > /etc/apt/sources.list.d/devel:kubic:libcontainers:stable.list"
wget -nv https://download.opensuse.org/repositories/devel:kubic:libcontainers:stable/x${ID^}_${VERSION_ID}/Release.key -O Release.key
sudo apt-key add - < Release.key
sudo apt-get update -qq
sudo apt-get -qq -y install buildah
```
  
#### To check version:-
  
```html
buildah -v
buildah version 1.21.3 (image-spec 1.0.1-dev, runtime-spec 1.0.2-dev)
```

## Building with a Dockerfile

buildah can build a container image by referring the same Dockerfile that docker build refers to. 

Let's consider this simple Dockerfile for example.  

 
**NOTE:** I have created a custom website with files under "/website" folder.


 ```html
 ~# cat Dockerfile

 FROM httpd:2.4-alpine

 COPY ./website/ /usr/local/apache2/htdocs/

 CMD ["httpd-foreground"]
 ```

Once we save this file as Dockerfile in our local directory, we can use the bud command to build the image:


 ```html
 ~# buildah bud -t htmlapp
```

#### Output:-


 ```html
STEP 1: FROM httpd:2.4-alpine
✔ docker.io/library/httpd:2.4-alpine
Trying to pull docker.io/library/httpd:2.4-alpine...
Getting image source signatures
Copying blob 242407d61b32 done
Copying blob f1ff32cc6322 done
Copying blob 59bf1c3509f3 [======================================] 2.7MiB / 2.7MiB
Copying blob aa5e87b8047c done
Copying blob 907dfa98a03e done
Copying blob 085d9bf42ab2 done
Copying config 294cecd6c7 done
Writing manifest to image destination
Storing signatures
STEP 2: COPY ./website/ /usr/local/apache2/htdocs/
STEP 3: CMD ["httpd-foreground"]
STEP 4: COMMIT htmlapp
Getting image source signatures
Copying blob 8d3ac3489996 skipped: already exists
Copying blob 83efd5aabbd5 skipped: already exists
Copying blob fc8c77d3c450 skipped: already exists
Copying blob 71a62b93fe7b skipped: already exists
Copying blob dede9d4fb2e9 skipped: already exists
Copying blob 5b9bda3e17e8 skipped: already exists
Copying blob 0463cf4167d7 done
Copying config 188e4b12cd done
Writing manifest to image destination
Storing signatures
--> 188e4b12cdc
Successfully tagged localhost/htmlapp:latest
188e4b12cdcb706d5ecb19a113f76cbf0328bc52e132a9406008525eaf083aca
```

List our current images by running:-

 ```html
~# buildah images

REPOSITORY                TAG          IMAGE ID       CREATED         SIZE
localhost/htmlapp         latest       188e4b12cdcb   2 minutes ago   62.7 MB
docker.io/library/httpd   2.4-alpine   294cecd6c779   8 weeks ago     60.5 MB
```
