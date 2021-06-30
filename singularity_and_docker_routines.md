# Convert local docker image to sif singularity file

Install Singularity :
```
https://singularity.hpcng.org/user-docs/3.7/quick_start.html#quick-installation-steps
```

If you are using docker locally there are two options for creating Singularity images without the need for a repository. You can either build a SIF from a docker-save tar file or you can convert any docker image present in docker’s daemon internal storage.

```
$ docker images alpine
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
alpine              latest              965ea09ff2eb        7 weeks ago         5.55MB

$ singularity run docker-daemon:alpine:latest
INFO:    Converting OCI blobs to SIF format
INFO:    Starting build...
Getting image source signatures
Copying blob 77cae8ab23bf done
Copying config 759e71f0d3 done
Writing manifest to image destination
Storing signatures
2019/12/11 14:53:24  info unpack layer: sha256:eb7c47c7f0fd0054242f35366d166e6b041dfb0b89e5f93a82ad3a3206222502
INFO:    Creating SIF file...
Singularity>
```
while docker-archive permits you to do the same thing starting from a docker image stored in a docker-save formatted tar file:

```
$ docker save -o alpine.tar alpine:latest

$ singularity run docker-archive:$(pwd)/alpine.tar
INFO:    Converting OCI blobs to SIF format
INFO:    Starting build...
Getting image source signatures
Copying blob 77cae8ab23bf done
Copying config 759e71f0d3 done
Writing manifest to image destination
Storing signatures
2019/12/11 15:25:09  info unpack layer: sha256:eb7c47c7f0fd0054242f35366d166e6b041dfb0b89e5f93a82ad3a3206222502
INFO:    Creating SIF file...
Singularity>
```

The results file will be saved in:
```
/root/.singularity/cache/oci-tmp/XXXXXX.sif
```
