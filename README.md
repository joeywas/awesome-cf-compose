# Awesome CF compose  [![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

A curated list of Docker Compose examples for use with CFML Docker images, whether the CF images from Adobe, available from [Adobe's Dockerhub CF repo](https://hub.docker.com/u/adobecoldfusion) or [Adobe's Amazon ECR CF repo](https://gallery.ecr.aws/adobe/), or the [native Lucee images](https://hub.docker.com/r/lucee/lucee), or the [Ortus CommandBox images](https://hub.docker.com/r/ortussolutions/commandbox/) for CF or Lucee.

This effort is based on the more general-purpose https://github.com/docker/awesome-compose project, but created separately to focus on aspects of CFML-oriented containers and integration examples that may be more typical of CFML application development.

These Compose files can provide a starting point for how you could integrate different services and manage their deployment with Docker Compose. Starting with Compose is a great first step to ultimate orchestration of containers--whether via Kubernetes, Swarm, or otherwise.

(You can convert Docker Compose files to Kubernetes manifests using the free [kompose tool](https://kompose.io/). Also, recent versions of Docker now support deployment of compose files via [docker context](https://docs.docker.com/engine/context/working-with-contexts/) whether [into AWS ECS](https://docs.docker.com/engine/context/ecs-integration/) or [into Azure ACI](https://docs.docker.com/engine/context/aci-integration/).)

So regard this project as a starting point for your further exploration, learning, and deployment, however it may be that you would run your containers.

## Contents
- [Example Docker Compose files for ColdFusion configuration variations](#Example-Docker-Compose-files-for-ColdFusion-configuration-variations)
- [Examples of Docker feature enablement](#Examples-of-Docker-feature-enablement)
- [Examples of integrated services](#Examples-of-integrated-services)
- [Getting started](#Getting-started)
- [Considerations regarding configuration](#Considerations-regarding-configuration)
- [Many kinds of examples planned](#Many-kinds-of-examples-planned)
- [Contribute](#Contribute)

## Example Docker Compose files for ColdFusion configuration variations
- [Considerations regarding use of CF containers](#Considerations-regarding-use-of-CF-containers)
- [ColdFusion latest](/cf-latest) (showing base CF image, as "latest" image, whatever version that may be)
- [ColdFusion 2021 latest](/cf-2021-latest) (showing how to specify "latest" CF2021 image, whatever update level that may be)
- [ColdFusion 2021 specific update](/cf-2021-update-specified) (showing how to specify a specific update level for the CF2021 image)
- [ColdFusion 2018 latest](/cf-2018-latest) (showing how to specify "latest" CF2018 image, whatever update level that may be)
- ColdFusion 2018 Update 1 (showing how to specify CF update level)
- [ColdFusion 2021 with modules imported](/cf-2021-installmodules)
- [ColdFusion 2021 with admin config via cfsetup json file](/cf-2021-importcfsettings)
- [ColdFusion with admin config via CAR file](/cf-2021-car-setup)
- [ColdFusion with setup script enabled](/cf-2021-setupscript)
- ColdFusion with secure profile enabled
- ColdFusion with mysql jar embedded
- [ColdFusion as a Dockerfile only](/cf-2021-latest-as-dockerfile) (For people who don't plan to or can't use Compose)
- [ColdFusion with site root in a subfolder](/cf-app_in_subfolder) (For people who want to run the root of their site as a subfolder of /app)
- [ColdFusion with specific Java version](/cf-2021-specific-java-version) 

### Examples of CF feature enablement
- [ColdFusion with Solr and and PDFg (CFHTML2PDF) features enabled](/cf-2021-with-addons)
- ColdFusion with Redis sessions enabled

### Examples of related CF feature enablements
- [ColdFusion 2021 with PMT 2021 enabled](/cf-2021-pmt)
- [ColdFusion 2018 with PMT 2018 enabled](/cf-2018-pmt)
- [ColdFusion with API Manager enabled](/cf-2021-api-manager)

## Examples of Lucee
- [Lucee latest](/lucee-latest) (showing base lucee image, as "latest" image, whatever version that may be)
- [Lucee with FusionReactor enabled](/lucee-with-fr)

## Examples of Commandbox images for ACF and Lucee
- [Commandbox image with latest Lucee5 ](/cmdbox-lucee-latest) (showing base luce5e image, as "latest" image, whatever version that may be)

## Examples of Docker feature enablement
- ColdFusion with CF webroot copied into image
- ColdFusion with CF webroot as host bind mount
- ColdFusion with CF webroot as Docker volume

### Examples of FusionReactor integration
- [ColdFusion 2021 with FusionReactor enabled](/cf-2021-with-FR)
- [Lucee with FusionReactor enabled](/lucee-with-fr)

## Examples of integrated services

### Examples of web server integration
- ColdFusion with Apache web server
- ColdFusion with IIS web server
- [ColdFusion with nginx web server](/cf-nginx)

### Examples of database integration
- ColdFusion with MySQL
- ColdFusion with SQL Server
- ColdFusion with Postgres
- ColdFusion with MongoDB

### Examples of reverse proxies/load balancers
- ColdFusion with Varnish
- ColdFusion with Squid
- ColdFusion with Traefik

### Other integrations
- ColdFusion with ? (let's generate ideas, and implement them)

### Examples of multiple integrated services
- ColdFusion with ? (a couple of examples)

Not intending to all possible permutations of the above service integration examples. See [Considerations regarding configuration](#Considerations-regarding-configuration), for more.

## Getting started

These instructions will get you through the bootstrap phase of creating and deploying examples of containerized applications with Docker Compose.

### Prerequisites

- Make sure that you have Docker and Docker Compose installed
  - Windows or macOS:
    [Install Docker Desktop](https://www.docker.com/get-started)
  - Linux: [Install Docker](https://www.docker.com/get-started) and then
    [Docker Compose](https://github.com/docker/compose)
- Download some or all of the examples from this repository.

### Running an example

The root directory of each example contains the `docker-compose.yaml` which
describes the configuration of service components. All examples can be run in
a local environment by going into the root directory of each one and executing:

```console
docker-compose up -d
```

Check the `README.md` of each example to get more details on the structure and expected output, if any.

To stop and remove the all containers of the example application, run:

```console
docker-compose down
```

## Considerations regarding configuration

Whether using or contributing to the repository, note  the following considerations.

### Considerations regarding use of CF containers

As a heads-up especially for those using the Adobe CF Docker images (though the concepts apply also in degrees to the Ortus CF/Lucee and native Lucee images), please note that the Adobe CF Docker images do offer many configuration options which are not being demonstrated in most of these simple examples. For more information on those options, see the [docs on using the Adobe Docker images](https://helpx.adobe.com/coldfusion/using/docker-images-coldfusion.html).

In particular, note how the docs explain that the images presume to find files in an `/app` folder within the image--or a docker/kubernetes volume that's been mounted to that folder. The basic example in the `cf-latest` shows some sample code in that folder, implemented within the container. You could copy your own code into that folder within the image in a Dockerfile, etc.

Note also that when running any of these Adobe CF examples, they default to using port 8500. If you may be running CF (or any app) that is already using port 8500, you will get an error, such as `Ports are not available: exposing port TCP 0.0.0.0:8500 -> 0.0.0.0:0`. You can modify the example docker-compose.yml offered here, changing the line referring to `"8500:8500"` so that the first number is whatever port you want to have the container use. So using `"8501:8500"` would cause it to be accessible instead at port 8501.

The previous comment applies similarly to the Ortus CF/Lucee and native Lucee images, which will default to using other ports, and again if you may be running something already on that port, then make the same sort of change in their respective docker-compose.yml file. 

### Limiting the combinations of examples
It would be tempting to create compose files that combine many things at once (integration of CF with Apache and mysql, or CF/Apache/mysql and the PMT, or CF/Apache/mysql and FR, or swapping out mysql for SQL Server, or IIS for Apache, and so on).

Because the number of such combinations could grow exponentially (and become clumsy to manage as a folder structure), there will be many cases where a given facet of integration will be demonstrated only once, in a single compose file, leaving the reader to take that information to bring together unique combinations on one or more other integrations which they may be interested to use.

Time will tell how to best organize when contributors may offer examples combining several integrations in one compose file.

### Some examples will presume dependencies and so will not be self-contained
While many integrations will be shown using resources ALSO found in the compose file (making it self-contained), as is the normal expectation for compose files, some examples will be setup presuming instead to integrate with some resource existing OUTSIDE of the compose file, such as a mysql server defined on or in the network of the host.

Such examples are necesary to meet real-world requirements as folks explore CF Docker images and integrations. Obviously, such examples will have dependencies which, if not existing, will cause the compose file to fail.

### Use of volumes or bind mounts
Similarly, some examples will demonstrate integration with Docker volumes (defined in the compose file) while others may demonstrate using bind-mount volumes that refer to host resources outside the docker file which again, if not existing, will cause the compose file to fail. Comments in the compose file or readme should clarify such expected dependencies, if they are not self-evident.

## Many kinds of examples planned
In this project, the focus will be on examples of the many kinds of integrations that are possible and potentially useful for users of CFML engines, ranging from optional web servers that can front it, to back-end databases resources it can integrate with, such as databases and caching engines--whether leveraging such integrations built-in to CF or not.

Initially I planned this to be just about the Adobe CF Docker images (since there's a general paucity of such info), but I appreciate how those using the other types of CF images could not only benefit from but also bring energy and great contributions to this effort.

A challenge will be how to clarify which compose files are suited to the ACF vs the Ortus CF or Lucee vs the native Lucee images. I haven't decided yet if that's best done by separate folders, or indications in the compose file name. I'm leaning toward folders. Even then, this could be a challenge to manage, but let's go for it!

For now, the lists below are from my initial effort that was laying out how to show use of the ACF images. The lists will evolve to more clearly cover all 3 kinds of images, where appropriate.

Speaking of ACF, the project is also expected to include examples of integrating with things like the ColdFusion API Manager (since CF2016 Enterprise) and the ColdFusion Performance Monitoring Toolkit (since CF2018 Standard and Enterprise), as well as demonstrations of implementation of other monitoring solutions, such as FusionReactor, SeeFusion, and other APMs.

[Contributions](#Contributions) are welcome.



<!--lint disable awesome-toc-->
## Contribute

We welcome examples that help people understand how to use Docker Compose for common applications. Check the [Contribution Guide](CONTRIBUTING.md) for more details.
