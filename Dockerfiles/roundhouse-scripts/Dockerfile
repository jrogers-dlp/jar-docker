FROM dotnetroundhouse/roundhouse
ARG DB_HOST
ARG DB_USERNAME
ARG DB_PASSWORD
ARG DB_NAME
ARG ENVIRONMENT_NAME="dev"
ENV ROUNDHOUSE_OUTPUT="/rh/output"
ENV RH_SCRIPT_DIR="/rh/scripts"

COPY scripts $RH_SCRIPT_DIR

ENTRYPOINT [ "dotnet" ]
CMD [ "rh.dll", "/cs", "Server=$DB_HOST;User Id=$DB_USERNAME;Password=$DB_PASSWORD;Database=$DB_NAME;MultipleActiveResultSets=true;", "/f", "$RH_SCRIPT_DIR", "/o", "$ROUNDHOUSE_OUTPUT", "/env", "$ENVIRONMENT_NAME", "/ct", "60", "/ni"]