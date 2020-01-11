# Docker container for Forward Tracker

1. Install Docker and make sure it is running correctly (https://docs.docker.com/v17.12/docker-for-mac/install/)
2. Build the star-fwd image 
3. OR skip 2 and download/import image from RCF

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


