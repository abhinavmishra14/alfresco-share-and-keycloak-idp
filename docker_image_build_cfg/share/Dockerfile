ARG SHARE_TAG

FROM alfresco/alfresco-share:${SHARE_TAG}

ARG TOMCAT_DIR=/usr/local/tomcat

#Fix for Admin tools user search issue (https://github.com/Alfresco/acs-community-packaging/issues/367)
RUN sed -i 's@<show-authorization-status>true</show-authorization-status>@<show-authorization-status>false</show-authorization-status>@' \
	$TOMCAT_DIR/shared/classes/alfresco/web-extension/share-config-custom.xml

# Copy add-ons jar module if there are any
COPY Dockerfile add-ons/*.jar $TOMCAT_DIR/webapps/share/WEB-INF/lib/

# Copy and apply amp add-ons
COPY Dockerfile add-ons/*.amp $TOMCAT_DIR/amps_share/
RUN java -jar $TOMCAT_DIR/alfresco-mmt/alfresco-mmt*.jar install \
              $TOMCAT_DIR/amps_share $TOMCAT_DIR/webapps/share -directory -nobackup -force
			  
# Create dirs for simple modules
RUN bash -c 'mkdir -pv $TOMCAT_DIR/modules/share'

#Setup simple module config
COPY simple-module-config/share.xml $TOMCAT_DIR/conf/Catalina/localhost/
#Copy simple modules if you have any
COPY simple-modules/* $TOMCAT_DIR/modules/share/
			  
#TODO:: Add more steps as needed