# build-gluon
Docker based build environment for Gluon. At the moment this image just supplies a consistent build environment. More automation and a more user friendly handling is planned.

Build the docker image:  
`docker build -t build-gluon - < Dockerfile`

Start a container with a shell prompt:  
`docker run -it -v $(pwd)/openwrt:/gluon/openwrt -v $(pwd)/output:/gluon/output build-gluon`  
This command maps the directories `openwrt` and `output` in your current working directory to the according directories inside the container. `openwrt` will contain the files that are created during the build process whereas `output` will contain your built images. Reusing `openwrt` is recommended as it will speed up the build process significantly when building previous targets again. 
You can use different paths of course, this is just an example. Just replace `$(pwd)/openwrt` and `$(pwd)/output` with paths to your liking.

Next steps for a successful build:
* `cd` to `/gluon` and run a `git pull` (not strictly necessary if you just built the docker image since the build process cloned the most recent version, however, I'd recommend making it a habit)
* check out the branch/tag you want to build, e. g. `git checkout v2017.1.x`
* run `make update GLUON_RELEASE=<your release name>`, e. g. `make update GLUON_RELEASE=2017.1.x`
* start building with `make GLUON_TARGET=<your target> GLUON_RELEASE=<your release name>`. I'd recommend adding `-j`, followed by the number of threads your CPU has, in order to make use of multithreading and speed up the build process. An example build command could be `make -j4 GLUON_TARGET=ar71xx-tiny GLUON_RELEASE=2017.1.x`.

Notes:  
* When switching Gluon branches a `make -C openwrt dirclean` can be necessary in order to remove an old toolchain.
* Make sure that you have enough free space, e. g. `ar71xx` needs around 11 GB. The other targets require similar amounts.

Fore more information visit https://gluon.readthedocs.io/.
