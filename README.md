Docker Registry Deploy Instructions
================================

### Generate Certificate

Generate certificates for Docker Registry
(or use one provided by a Certificate Authority):

    ./generate-cert.sh

### Run Image

    docker-compose up -d
