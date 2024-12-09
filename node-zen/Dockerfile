# This Docker image utilizes the Actian Zen (Pervasive ODBC Interface) Client Driver
# to allow connecting to an Actian Zen database via Nodejs.

# Basing off of this image requires the use of additional dependencies in your application.
# Examples include:
# odbc (npm package)
# @ngrey5/odbc (opinionated version of the package above)
# Using the packages above requires 'python3' and 'build-essential' to be installed from the apt-get Debian package manager
# due to it containing native node modules that need to be built on the OS at installation


# The following are required to build this image:
# -----------------------------------------------
# /assets (directory)
#   zen-client.tar.gz              : actian zen client archive file. See: https://esd.actian.com/product/Zen_PSQL/v14_SP2_Production/Linux_X86_64-bit/Zen_v14_SP2_Classic/Zen-Client-linux-x86_64-14.21.004.000.tar.gz/http
# -----------------------------------------------

# The following commands are available when using this image
# ----------------------------------------------------------
# > dsnadd -dsn=DSN_NAME -host=HOST_NAME -db=DATABASE_NAME
# ----------------------------------------------------------


# Create a build to copy, extract, and remove unneccesary files from the zen-client
# We need this to avoid Docker 'layering' each command and contributing it to the total size of the image
# Note: we use the same base image to prevent compatibility issues
FROM debian:buster-slim AS unpack-zen
COPY ./assets/zen-client.tar.gz /usr/local/zen-client.tar.gz
RUN tar -zxf /usr/local/zen-client.tar.gz --directory /usr/local
RUN rm -rf /usr/local/zen-client.tar.gz


FROM node:16-buster-slim
# Update repos and install dependecies for odbc
RUN apt-get update
RUN apt-get install unixodbc unixodbc-dev -y

# Set the necessary environment variables to utilze the actian zen client
ENV ACTIANZEN_ROOT=/usr/local/actianzen
ENV PATH=${PATH}:/${ACTIANZEN_ROOT}/bin:/bin:/usr/bin
ENV LD_LIBRARY_PATH=${ACTIANZEN_ROOT}/lib64:${ACTIANZEN_ROOT}/lib:${ACTIANZEN_ROOT}/bin:/usr/lib
ENV MANPATH=${MATHPATH}:${ACTIANZEN_ROOT}/man

# Copy the unpacked files from the unpack-zen build
COPY --from=unpack-zen /usr/local/actianzen ${ACTIANZEN_ROOT}

# Run the post install script provided by the client
# This installs the driver and creates a 'zen-svc' user that we won't use
RUN ${ACTIANZEN_ROOT}/etc/clientpostinstall.sh

# Copy the Actian Zen (Pervasive ODBC Interface) driver definitions to the system file
RUN cat ${ACTIANZEN_ROOT}/etc/odbcinst.ini > /etc/odbcinst.ini