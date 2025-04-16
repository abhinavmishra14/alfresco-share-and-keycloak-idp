# alfresco-share-and-keycloak-idp

This project demonstrates how to configure Alfresco, Share, Content App, and Keycloak in a Dockerized environment. It also shows how to integrate the Share interface with Keycloak for OIDC-based authentication.

## Important Configuration (Check docker-compose for more info)

- Create a local IP to HOST_NAME name mapping in in `hosts` file. e.g. 192.168.1.12 alfresco.local.com or 127.0.0.1 localhost.

- Add a hostname in host file to map to HOST_NAME variable in `.env` before running Docker Compose. This is used to start keycloak with a hostname.

- Review `alfresco-realm.json` in case you want to add/update any user or group. Do not change the realm and client configuration until first import. You can play around realm later and adjust accordingly as needed.

- Add following properties to the alfresco (repository) JAVA_OPTS:

	```
	 -Dauthentication.chain=identity-service1:identity-service,alfrescoNtlm1:alfrescoNtlm
	 -Didentity-service.enable-basic-auth=true
	 -Didentity-service.auth-server-url=http://keycloak:8080
	 -Didentity-service.realm=alfresco
	 -Didentity-service.resource=alfresco
	 -Didentity-service.authentication.validation.failure.silent=false
	 -Didentity-service.authentication.enabled=true
	 ```
	 
- Add following properties to the share JAVA_OPTS:

	```
	-Daims.resource=alfresco
	-Daims.enabled=true
	-Daims.realm=alfresco
	-Daims.enableBasicAuth=true
	-Daims.authServerUrl=http://keycloak:8080
	```
	
- Add the following as a startup command for Keycloak:

	```
	start --import-realm --hostname=${HOST_NAME}
	  --hostname-port=8585
	  --http-enabled=true
	  --hostname-strict-https=false
	```
	
- Add the following environment variables for Keycloak (for h2):

	```
	 - KEYCLOAK_ADMIN=admin
	 - KEYCLOAK_ADMIN_PASSWORD=admin
	 - KC_HOSTNAME=keycloak
	 - KC_DB=h2
	```
	
- Add the following environment variables for Keycloak (for PostgreSQL. Make sure to keep same credentials for postgres container as well):

	```
	 - KEYCLOAK_ADMIN=admin
     - KEYCLOAK_ADMIN_PASSWORD=admin
	 - KC_HOSTNAME=keycloak
	 - KC_DB=postgres
	 - KC_DB_URL=jdbc:postgresql://postgres-idp:5432/keycloak
	 - KC_DB_USERNAME=keycloak
     - KC_DB_PASSWORD=idp@1234
    ```
	

### Docker Image Versions used for this project

- Alfresco Content Services : 7.4.2.5-A1
- Alfesco Share : 7.4.2.5-A1
- Keycloak : 24.0.3
- Alfresco Search Service (Solr6) : 2.0.15
- PostgreSQL : 14.4
- Transform Service (Community Version - CORE-AIO) : 5.1.7
- ActiveMQ : 5.18-jre17-rockylinux8

### Add-Ons used for this project

- OOTBee Support Tools : 1.2.2.0


### To build use following command:

- To build the images, This command will ignore any images which are already built and no changes to DockerFile has been identified. It will use cache.

`docker-compose -f ./docker-compose.yml build`

- To build the images with no cache. It will force rebuild

`docker-compose -f ./docker-compose.yml build --no-cache`


### To launch containers use following command:

`docker-compose -f ./docker-compose.yml up`


### To build and launch containers use following command:

`docker-compose -f ./docker-compose.yml up --build`


### To shutdown use following command:

`docker-compose -f ./docker-compose.yml down`

### To tail logs use following command:

`docker-compose -f ./docker-compose.yml logs -f`


### You can use launcher.bat/launcher.sh script to build, start, stop, purge volumes and tail logs:

- For Windows:

`.\launcher.bat build`

`.\launcher.bat start`

`.\launcher.bat stop`

`.\launcher.bat purge`

`.\launcher.bat tail`


- For Linux:

`.\launcher.sh build`

`.\launcher.sh start`

`.\launcher.sh stop`

`.\launcher.sh purge`

`.\launcher.sh tail`


## Access URLs

http://localhost:8080

Alfresco Content App

* user: admin
* password: admin

http://localhost:8080/share

Alfresco Share

* user: admin
* password: admin

http://localhost:8080/alfresco

Alfresco Content Services (Repository)

* user: admin
* password: admin

http://localhost:8585

Keycloak

* user: admin
* password: admin

**Sample users and groups available to test with**

Users

* Username: admin | Password: admin
* Username: johndoe | Password: admin

Roles
* editorial_role
* default-roles-alfresco

Groups

* Editorial
* Administrator
