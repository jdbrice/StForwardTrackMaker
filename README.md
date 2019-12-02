


Build the docker image:
NOTE: requires the star-sw-test base image. See: https://github.com/star-bnl/star-sw

```sh
docker build --rm -t star-sw-fwd -f docker/Dockerfile.star-sw-fwd docker
```

Use the image in this way:
```sh
docker run --rm --name FWD -ti -w /tmp/work -v <path_to_repo>/StRoot:/tmp/star-sw/StRoot -v <path_to_repo>/work:/tmp/work star-sw-fwd bash
```
