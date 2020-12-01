## How to build locally

from an empty dir:
`docker run -ti -v "$(pwd)":/go golang:1.9.1 bash`

inside the container:
```
go get github.com/rexray/rexray
cd src/github.com/rexray/rexray/
make # builds rexray binary
```


## Fix rexray volumes with OVH public cloud (Openstack)

Bug is well described here:
https://github.com/rexray/rexray/pull/1240

This solution is not "clean", so it has never been merged.
However, it seems suitable enough for our specific problem, except for one thing: the paths it checks to correlate the block devices to the volume ID is not the same with OVH's Openstack.

Here's how I did it:

Apply the patch:
```
wget https://patch-diff.githubusercontent.com/raw/rexray/rexray/pull/1240.patch -O 1240.patch
git am 1240.patch
```

fix libstorage/drivers/storage/cinder/storage/cinder_storage.go


## create plugin container

From my workstation, because it requires docker and I was lazy to run a docker container:
```
export DRIVER=cinder
export DOCKER_PLUGIN_ROOT=hervenicol
# edit Makefile, with `DOCKER_PLUGIN_NAME := hervenicol/rexray:latest` 
make build-docker-plugin
```

