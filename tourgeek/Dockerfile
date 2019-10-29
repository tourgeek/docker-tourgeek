FROM tourgeek-karaf

RUN echo start new cache - TODO Remove this soon
# Add repos
RUN git clone https://github.com/tourgeek/workspace.git \
&& cd workspace \
&& bin/git/checkout.sh \
&& mvn clean install

RUN git clone https://github.com/mojohaus/webstart.git;cd webstart/webstart-jnlp-servlet;mvn clean install
RUN cd workspace/wrapped;mvn clean install
RUN cd workspace/wrapped/jnlpservlet;mvn clean install
RUN cd workspace/wrapped/json;mvn clean install
RUN cd workspace/wrapped/c3p0;mvn clean install
RUN cd workspace/wrapped/sha;mvn clean install

COPY bin bin

# RUN cd bin/temp;mvn clean install

#COPY bin/old old
#COPY bin/old-local old-local
#COPY bin/old-local-new old-local-new
#COPY bin/org.jbundle.util.biorhythm.osgi/0.8.6 /root/.m2/repository/org/jbundle/util/biorhythm/org.jbundle.util.biorhythm.osgi/0.8.6
#COPY bin/org.jbundle.util.biorhythm/0.8.6 /root/.m2/repository/org/jbundle/util/biorhythm/org.jbundle.util.biorhythm/0.8.6
#COPY bin/org.jbundle.util.biorhythm.i18n/0.8.6 /root/.m2/repository/org/jbundle/util/biorhythm/org.jbundle.util.biorhythm.i18n/0.8.6

CMD ["sleep","36000"]
