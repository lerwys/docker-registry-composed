Docker Registry Deploy Instructions
================================

### Get Certificate

Be sure to clone this repository with --recursive flag.
Otherwise, docker-compose will fail to initialize.

### Install Certificate in Host

Be sure to copy the certificate to the correct locations
on the host computer with the following commands:

    sudo mkdir -p /etc/docker/certs.d/dockerregistry.lnls-sirius.com.br:443
    sudo cp ./foreign/docker-registry-certs/certs/domain.crt /etc/docker/certs.d/dockerregistry.lnls-sirius.com.br:443/ca.crt

    sudo cp ./foreign/docker-registry-certs/certs/domain.crt /usr/local/share/ca-certificates/dockerregistry.lnls-sirius.com.br.crt
    sudo update-ca-certificates

### Run Image

    docker-compose up -d
