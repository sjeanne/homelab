# Pi-Hole

Install Pi-hole in a docker container.
Let's see if DHCP configuration will work when Pi-hole will be my DHCP server

Follow this page from official documentation: https://docs.pi-hole.net/docker/

Create docker-compose.yaml in /administration and copy/paste content from pi-hole documentation

Add a `.env` file and define admin password in `PI_HOLE_ADMIN_PASSWORD` variable

Adjust HTTP/HTTPS port to 8080 and 8443 values.
