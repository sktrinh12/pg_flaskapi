FROM sktrinh12/bdb-shinyapp:base_v1

ARG UNAME
ARG PASSWORD 
ENV UNAME $UNAME 
ENV APP_HOME "/home/${UNAME}"
ENV PASSWORD $PASSWORD 

RUN apt-get update && \
    apt-get upgrade -y && \
    apt-get clean

RUN useradd -m -d $APP_HOME "${UNAME}" \
    && chown -R ${UNAME}:${UNAME} $APP_HOME \
    && echo "${UNAME}:${PASSWORD}" | chpasswd 


# set up git ssh 
USER $UNAME
RUN mkdir -p $APP_HOME/.ssh/
COPY meta_app/shinyapp $APP_HOME/.ssh/id_rsa
RUN touch $APP_HOME/.ssh/known_hosts
RUN ssh-keyscan github.com >> $APP_HOME/.ssh/known_hosts

# git clone shiny app
RUN mkdir $APP_HOME/app \
    && git clone git@github.com:sktrinh12/shiny_meta_app.git $APP_HOME/app


# change ownership to UNAME 
USER root 
RUN chown -R $UNAME:$UNAME $APP_HOME

EXPOSE 3838

CMD ["R", "-e", "shiny::runApp('/home/bdb/app/meta_app', host= '0.0.0.0', port = 3838)"]
