# NOVA Asset Administration Shell and Keycloak

The NOVA Asset Administration Shell (NOVAAS) is an open source reference implementation and execution environment - developed by NOVA School of Science and Technology - for the Asset Administration Shell (AAS) concept proposed by the Reference Architectural Model for Industrie 4.0 (RAMI4.0). 
The AAS implements the concept of the Digital Twin of a physical object turning it into an Industrie 4.0 component that facilitates interoperability in industrial applications. An identity and access management server has been included within the stack.

This repository contains all files that are actually needed to build a docker image for NOVAAS supported by Keycloak for Identity and Access Management.
In order to install and run it, dowload/clone the repository and build and use the docker-compose file.
The NOVAAS-Keycloak repository is preloaded with a set of files (documentation, asset, manifest) that are used to show the component. For creating a new specific NOVAAS these files need to be substituted with the proper ones.


## Using Docker-compose

To run the stack docker-compose is the best option. to start the stack the following docker-compose commands need to be executed:

`docker-compose run --rm start_dependencies`

This command allows the synchronization between the NOVAAS and the keycloak server. In particular it guarantees that keycloak server is ready before starting NOVAAS. Once the keycloak server is started, the following command can be executed:

`docker-compose up -d novvas`

To run the image. 

Furthermore, the command:

`docker-compose build`

can be used to build a new image. 

The provided stack will run two docker containers, namely:

1. NOVAAS: will run on port 1870, however it is possible to change this behaviour by setting the environmental variables PORT_FORWARDING and HOST in the .env file. The NOVAAS embeds an MQTT client for pushing out data. This client needs to be configured. To do that it is possible to access the NOVAAS backend by using the following link:

http://localhost:1870

1. Keycloak: will run on port 8080. The identity and access mangement service will be pre-loaded with an already existing realm. 

### Notes
There are a set of environmental variables you need to set to allow some costumization of the application.

##### NOVAAS

    - PORT_FORWARDING=1870: is the host port used to map the container port 1880.  
    - HOST=localhost: is the ip address of the host machine where NOVAAS is deployed.

These two environmenta variables are needed to properly configure the internal iFrame node that is used to expose in the ui the dashboards for each one of the internal data sources. Setting this variable to localhost will only expose the dash tab within the ui in the host. The next set of evironmental variables are used for configuring the keycloak clients within NOVAAS.

    - KEYCLOAK_URL=http://localhost:8080/auth: is the url of the authentication service;
    - KEYCLOAK_REALM=industry40_edge: is the name of the realm NOVAAS is using; this realm is preloaded in keycloak. The _realm.json_ within the repository is the realm in json format;
    - KEYCLOAK_CLIENT_ID=authServer: is the id of the authServer used by the NOVAAS to allow external application to use the API; 
    - KEYCLOAK_CLIENT_ID_UI=uiAuthServer: is the id of the authServer used by the internal NOVAAS User Interface;
    - KEYCLOAK_CLIENT_SECRET=9f274121-8e01-49f6-a57a-2e733942e0a1: is the secret that external users need to use to retrieve an access token;
    - KEYCLOAK_CLIENT_SECRET_UI=3e140df7-dd6c-4487-8047-cb4973de1c0d: is the secret that the inernal NOVAAS User Interface uses to retrieve an access token

#### Keycloak
    - KEYCLOAK_USER=admin: to create an initial admin user with username `admin;
    - KEYCLOAK_PASSWORD=admin: to create an initial user with password `admin`;
    - KEYCLOAK_IMPORT=/tmp/realm.json:  import a previously exported realm.




![Semantic description of image](/source/images/Screenshot_2020-12-15_at_22.40.31.png)"NOVAAS Backend once user is logged in"

To access the backend the user needs to insert username and password. These are the default username and password from the node-red settings file, namely:

- username: admin
- password: password

## Run another version of NOVAAS from this base folder

NOVAAS has been designed in order to be as generic as possible, if you want to run your own version of the NOVAAS you should perform the following steps:
1. Add all the documentation files (datashees, user manuals, etc.) within the folder "file/aasx/docu", the names of the files should be aligned with the names in the manifest;
1. Add an image of the concerned asset within the folder "files/images". Note that the name of the file **must** be kept -> novaas_concerned_asset.jpg;
1. Add the Manifest file within the folder "files/manifest". Note that the name of the file **must** be kept -> AmI_as_manifest.json. In particular this file follows the data model provided in https://www.plattform-i40.de/PI40/Redaktion/EN/Downloads/Publikation/Details_of_the_Asset_Administration_Shell_Part1_V3.html and can be created by using the aasx-package-explorer tool (https://github.com/admin-shell-io/aasx-package-explorer) ;
1. Change the httpauth file in the folder "files/httpauth" properly;
1. Run the docker and/0r docker-compose commands. 

## NOVAAS in action (Click on the Image to Show the Video)

[![Watch the video](/source/images/Screenshot_2020-12-15_at_22.20.37.png)](https://gitlab.com/gidouninova/novaas/-/blob/master/source/videos/NOVAAS_myMovie.mp4)

