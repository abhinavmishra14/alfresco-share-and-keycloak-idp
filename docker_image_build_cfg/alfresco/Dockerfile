ARG ACS_TAG

FROM alfresco/alfresco-content-repository-community:${ACS_TAG}

ARG TOMCAT_DIR=/usr/local/tomcat

#Change to root so that sed command can be executed
USER root

# Copy add-ons jar module if there are any
COPY Dockerfile add-ons/*.jar $TOMCAT_DIR/webapps/alfresco/WEB-INF/lib/

# Copy and apply amp add-ons
COPY Dockerfile add-ons/*.amp $TOMCAT_DIR/amps/
RUN java -jar $TOMCAT_DIR/alfresco-mmt/alfresco-mmt*.jar install \
              $TOMCAT_DIR/amps $TOMCAT_DIR/webapps/alfresco -directory -nobackup -force
			  
# Create dirs for simple modules
RUN bash -c 'mkdir -pv $TOMCAT_DIR/modules/platform'

#Change back to alfresco
USER alfresco

#Setup simple module config
COPY simple-module-config/alfresco.xml $TOMCAT_DIR/conf/Catalina/localhost/
#Copy simple modules if you have any
COPY simple-modules/* $TOMCAT_DIR/modules/platform/

#Copy and override the alfresco-global.properties which comes with custom image. 
#It could be useful in cases when you already built a custom image but launch an container with some additional config in global properties.
COPY alfresco-global.properties $TOMCAT_DIR/shared/classes/alfresco-global.properties

#TODO:: Add more steps as needed


