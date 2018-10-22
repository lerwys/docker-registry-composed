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

Restart Docker with the following command:

    systemctl restart docker.service

### Run Image

    docker-compose up -d

### Migrate DockerHub images to personal Docker Registry

Replace "dockerregistry.lnls-sirius.com.br" with your docker registry
DNS address and "lnlsdig" with your DockerHub organization, if any.
More instructions can be found at: [migrator project](https://github.com/docker/migrator)

    docker run -it --rm -v /var/run/docker.sock:/var/run/docker.sock \
    -v /etc/docker/certs.d:/etc/docker/certs.d:ro -e V1_REGISTRY=docker.io \
    --add-host dockerregistry.lnls-sirius.com.br:10.2.128.31 \
    -e USE_INSECURE_CURL=true -e V2_REGISTRY=dockerregistry.lnls-sirius.com.br \
    -e DOCKER_HUB_ORG=lnlsdig -e V1_REPO_FILTER="epics-ioc" docker/migrator 2>&1 | \
    tee migration.log

If you get an error like "curl => API failure getting token", try running
docker with "--net host" flag, like this:

    docker run --net host -it --rm -v /var/run/docker.sock:/var/run/docker.sock \
    -v /etc/docker/certs.d:/etc/docker/certs.d:ro -e V1_REGISTRY=docker.io \
    --add-host dockerregistry.lnls-sirius.com.br:10.2.128.31 \
    -e USE_INSECURE_CURL=true -e V2_REGISTRY=dockerregistry.lnls-sirius.com.br \
    -e DOCKER_HUB_ORG=lnlsdig -e V1_REPO_FILTER="epics-ioc" docker/migrator 2>&1 | \
    tee migration.log
