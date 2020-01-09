# Docker container for Forward Tracker

1. Install Docker and make sure it is running correctly (https://docs.docker.com/v17.12/docker-for-mac/install/)
2. Build the star-fwd image 
3. OR skip 2 and download/import image from RCF
4. Work inside the container by mounting StRoot into container

## 1. Download and setup Docker
Follow directions here:
https://docs.docker.com/v17.12/docker-for-mac/install/

## 2. Build the star-fwd image
move into a clean directory and checkout this repo:
```
git clone git@github.com:jdbrice/StForwardTrackMaker.git
cd StForwardTrackMaker
```

### Build the star-fwd image
```
docker build --rm -t star-fwd -f docker/Dockerfile.star-fwd docker
```

Make sure that it was built correctly:
```
docker image ls
```

Should show the `star-fwd` image:
```
REPOSITORY            TAG                 IMAGE ID            CREATED             SIZE
star-fwd              latest              fe9cfcdad597        53 minutes ago      4.62GB
```

## 3. OR skip 2 and download/import image from RCF
If you have access to RCF you can download my pre-built image from RACF.
I have them kept here: `/star/data03/pwg/jdb/FWD/docker/` with the image filenames taged with the date I created them.
Download the image from RACF then on your local machine run:
```
docker load < image_file.tar.gz
```

where image file is e.g. `star-fwd_12122019.tar.gz` - the image I built on `12/12/2019`

After importing the image run `docker image ls` and verify that the `star-fwd:latest` image is listed.


NOTE for me: I build the images from my version with this command
```
docker save star-fwd:latest | gzip > "star-fwd_`date +"%m%d%Y"`.tar.gz"
```



## 4. Use the star-fwd image for development
The following command allows you to mount the `StRoot` and `work` directories of this repo into the running container. That way you can edit the code on your local machine using the container to build and run.

Develop inside the container:
```
docker run --rm --name FWD -ti -w /tmp/work -v <path_to_repo>/StRoot:/tmp/star-sw/StRoot -v <path_to_repo>/work:/tmp/work star-fwd bash
```
this will start you inside the container with you local `StRoot` and `work` directories from this repo mounted.
You can edit the code/files in these directories and run them inside the container.
NOTE: the `--rm` flag means that the container will be destroyed when you run `exit` from inside the container. Your work should be saved in the mounted volumes, so this should be OK, if not remove the `--rm` flag.

Inside the container your working directory should be `/tmp/work/`

