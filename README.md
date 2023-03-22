# How to build EE

```
$ ansible-builder build --tag robschm/na-ansible-ee:0.0.1 --container-runtime docker -f execution-environment.yml
File context/_build/requirements.txt had modifications and will be rewritten
Running command:
  docker build -f context/Dockerfile -t robschm/na-ansible-ee:0.0.1 context
Complete! The build context can be found at: /home/robert/PycharmProjects/na-ansible-ee/context
```

```
$ docker images
REPOSITORY                          TAG                  IMAGE ID       CREATED              SIZE
robschm/na-ansible-ee               0.0.1                d2035b5499ab   About a minute ago   2GB
```


# Test EE
``ansible-runner``
```
$ ansible-runner  run -p test_ee.yml   --container-image=my_ee .
```

[TODO] ``ansible-navigator``
```
$ ansible-navigator run -eei  dsc-na-ansible-ee  --pp never dsc-na-ansible-ee/test_ee.yml
```
 
# Upload EE
Upload EE to ``hub.docker.com``

> [!attention]
> The repository must be created before trying to push the image.

Example
```
$ docker login
Authenticating with existing credentials...
WARNING! Your password will be stored unencrypted in /home/robert/.docker/config.json.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/#credentials-store

Login Succeeded

$ docker tag  robschm/na-ansible-ee:0.0.1  robschm/awx-netapp-ee:latest

$ docker images
REPOSITORY                          TAG                  IMAGE ID       CREATED          SIZE
robschm/na-ansible-ee               0.0.1                d2035b5499ab   12 minutes ago   2GB
awx-netapp-ee2                      latest               d2035b5499ab   12 minutes ago   2GB

$ docker push robschm/awx-netapp-ee2:latest
```

# Configuration in AWX
Path:
```
docker.io/robschm/awx-netapp-ee:latest
```

