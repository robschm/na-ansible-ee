<!-- Improved compatibility of back to top link: See: https://github.com/othneildrew/Best-README-Template/pull/73 -->
<a name="readme-top"></a>

<!-- PROJECT LOGO -->
<br />
<div align="center">
  <h3 align="center">NetApp Ansible/AWX Execution Environment</h3>
</div>



<!-- TABLE OF CONTENTS -->
<details>
  <summary>Table of Contents</summary>
  <ol>
    <li>
      <a href="#about-the-project">About The Project</a>
      <ul>
        <li><a href="#built-with">Built With</a></li>
      </ul>
    </li>
    <li>
      <a href="#getting-started">Getting Started</a>
      <ul>
        <li><a href="#prerequisites">Prerequisites</a></li>
        <li><a href="#installation">Installation</a></li>
      </ul>
    </li>
    <li><a href="#usage">Usage</a></li>
    <li><a href="#license">License</a></li>
    <li><a href="#contact">Contact</a></li>
    <li><a href="#acknowledgments">Acknowledgments</a></li>
  </ol>
</details>



<!-- ABOUT THE PROJECT -->
## About The Project



### Built With

This section should list any major frameworks/libraries used to bootstrap your project. Leave any add-ons/plugins for the acknowledgements section. Here are a few examples.

* [![Ansible][Ansible.com]][ansible-url]
* [![Docker][Docker.com]][docker-url]


<p style="text-align: right">(<a href="#readme-top">back to top</a>)</p>



<!-- GETTING STARTED -->
## Getting Started

### Directory Layout 

```
  .na-ansible-ee
  |-- _build                                  
      |-- bindep.cfg               # system requirements for the container image
      |-- requirements.txt         # list of Python dependencies
      |-- requirements.yml         # list of Ansible collections 
  |-- execution-environment.yml    # starting point for ansible-builder
  ```

### Prerequisites

* Python, e.g. 3.11
* docker or podman installed
* Python Libraries
  * ansible-builder >= 3.0 (for version 3 execution environment definitions)
  * ansible-runner

In order to build images, you need to have either podman or docker installed as well
as the ansible-builder Python package.
The `--container-runtime` option needs to correspond to the Podman/Docker executable
you intend to use.


### Installation

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- USAGE EXAMPLES -->
## Usage

### How to build EE

Build docker image with a tag, e.g. robschm/awx_netapp_ee:latest
```
$ ansible-builder build -f execution-environment_v3_py311.yml --tag <your_tag> --container-runtime docker -v3
```

### Using EE

Run playbook with ansible-navigator (Text-based user interface for Ansible):
```console
ansible-navigator run --eei <your_tag> test_ee.yml -m stdout
```
Options:
 --eei  --execution-environment-image  Specify the name of the execution environment image (default: ghcr.io/ansible/creator-ee:v0.20.0)
 -m     --mode                         Specify the user-interface mode (stdout|interactive) (default: interactive)

You can copy `ansible-navigator.yml.example` to `ansible-navigator.yml` and change
the default configuration of `ansible-navigator` to shorten the command:

```console
ansible-navigator run test_ee.yml
``` 


### Debugging EE
Display list of installed collections in the created `Execution Environment`:
```console
# docker run --rm <your_tag> ansible-galaxy collection list
Collection          Version
------------------- -------
ansible.posix     1.5.4  
community.crypto  2.16.0 
community.general 8.0.2  
netapp.ontap      22.8.3  
```

Ansible Runner is a tool and python library that helps when interfacing with Ansible directly
or as part of another system whether that be through a container image interface,
as a standalone tool, or as a Python module that can be imported.
The goal is to provide a stable and consistent interface abstraction to Ansible.

Run a playbook with ``ansible-runner``:
```console
ansible-runner  run -p test_ee.yml   --container-image=<your_tag> .    
```

Run execution environment directly with docker:
```console
docker run -it --rm --workdir /runner/project/project -v /home/robschm/projects/na-ansible-ee/:/runner/project/project robschm/na-ansible-ee:latest ansible-playbook test_ee.yml
```


### Upload EE
Upload EE to to a container registry like ``hub.docker.com``

> [!attention]
> The container repository must be created before trying to push the image.

Steps
- login to docker hub
- tag your image
- push image to docker hub

Example
```console
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

Add latest tag after pushing versioned image
Add latest tag to current version (latest must be added manually to point to the latest numbered version)
```console
docker login
docker tag me/mystuff:v0.0.1 me/mystuff:latest
docker push me/mystuff:latest
```

### Configuration in AWX
Path:
```
https://docker.io/robschm/awx-netapp-ee:latest
```



<p style="text-align: right">(<a href="#readme-top">back to top</a>)</p>




<!-- LICENSE -->
## License

Distributed under the MIT License.

<p style="text-align: right">(<a href="#readme-top">back to top</a>)</p>



<!-- CONTACT -->
## Contact

Robert Schmitt - rs@dirk-schmiedt.de

Project Link: [https://github.com/robschm/na-ansible-ee](https://github.com/robschm/na-ansible-ee)

<p style="text-align: right">(<a href="#readme-top">back to top</a>)</p>



<!-- ACKNOWLEDGMENTS -->
## Acknowledgments

Use this space to list resources you find helpful and would like to give credit to. I've included a few of my favorites to kick things off!

* [Best-README-Template](https://github.com/othneildrew/Best-README-Template)
* [Img Shields](https://shields.io)
* [Self build AWX execution environment](https://thedatabaseme.de/2022/09/09/self-build-awx-execution-environment/)
* [thedatabaseme AWX EE](https://github.com/thedatabaseme/awx-ee)
* [Ansible AWX EE](https://github.com/ansible/awx-ee/)
* [Ansible Execution Environment](https://www.sdnit.se/en/post/ansible-execution-environment)

<p style="text-align: right">(<a href="#readme-top">back to top</a>)</p>


<!-- MARKDOWN LINKS & IMAGES -->
<!-- https://www.markdownguide.org/basic-syntax/#reference-style-links -->
[Ansible.com]: https://img.shields.io/badge/ansible-%231A1918.svg?style=for-the-badge&logo=ansible&logoColor=white
[Ansible-url]: https://ansible.com
[Docker.com]: https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=Docker&logoColor=white
[Docker-url]: https://docker.com 
























