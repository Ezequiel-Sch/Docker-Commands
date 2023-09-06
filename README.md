# Docker-Commands

## Docker CLI


----------------
Init a container:

```docker run <image-name>```

> Example: docker run ubuntu

--------------------
An interactive Docker container:

```docker run -it <image-name>``` 

> Example: docker run -it ubuntu

--------------------------
Running a container detached:

Adding `-d` to `docker run` will run the container in the background, giving us back control ofthe shell.

```docker run -d <image-name>```

> Example: docker run -d postgres

-------------

Listing containers:

```docker ps```

--------------------
Listing running containers:

`docker ps -a`

----------------------
Filtering running containers:

` docker ps -f "name=<container-name>" `

-----------------------------------
Container logs:

`docker logs <container-id>`

----------------------
Live logs:

`docker logs -f <container-id>`

-----------------------
Cleaning up:

`docker container rm <container-id>`

----------------------
Pulling an image:

`docker pull <image-name>`

> Example: docker pull postgres

---------------------
Image versions:

`docker pull <image-name>:<image-version>`

> Example: docker pull ubuntu:22.04

----------------------------------
List all local images:

`docker images`

-----------------------
Remove an image:

`docker image rm <image-name>`

--------------------

Remove all stopped containers:

`docker container prune`

-------------------------

Remove all images:

`docker image prune -a`

---------------------

## Writing Own Docker Images

A Dockerfile always start from another image, specified using the FROM instruction.

`FROM ubuntu`

----------------------------

Add a shell command to image:

`RUN <valid-shell-command>`

-----------------------------

Make sure no user input is needed for the shell-command.

`RUN apt-get install -y python3`

----------------------------

Build image from Dockerfile.

`docker build /location/to/Dockerfile`

-----------------------------------

Build image in current working directory.

`docker build .`

----------------------------------

Choose a name when building an image.

`docker build -t <first_image> .`

-------------------------------

Copy files from host to the image

`COPY <src-path-on-host> <dest-path-on-image>`

> Example: COPY /projects/pipeline_v3/pipeline.py /app/pipeline.py



if the destination path does not have a filename, the original filename is used:

> Example: COPY /projects/pipeline_v3/pipeline.py /app/

----------------------------------

Copy a folder from host to the image

`COPY <src-folder> <dest-folder>`

> Example: COPY /projects/pipeline_v3/ /app/

Not specifying a filename in the `src-path` will copy all the file contents.

> Example: COPY /projects/pipeline_v3/ /app/

will copy everything under `pipeline_v3/`

-----------------------------

We can't copy from a parent directory where we build a Dockerfile

-------------------------------

Downloading files Instead of copying files from a local directory, files are often downloaded in the image build.

Download a file:

`RUN curl <file-url> -o <destination>`

Unzip the file:

`RUN unzip dest-folder/filename.zip`

Remove the original zip file:

`RUN rm copy_directory/filename.zip`

-----------------------------------

### Downloading files efficiently

The solution is to download, unpack and remove files in a single instruction.

`RUN curl <file_download_url> -o <destination_directory/filename.zip> \&& unzip <destination_directory/filename.zip> -d <unzipped-directory> \&& rm <destination_directory/filename.zip>`

--------------------------------------------------------------------------------------------------------------------
