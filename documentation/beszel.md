# Install and configure Beszel

Follow ducomentation from Beszel project page: https://www.beszel.dev/guide/hub-installation

Beszel is composed of two services:
- the front end application that collects and displays data
- the agent that collects data on the Pi

To deploy the front end application, add in the docker-compose.yml file:
```
services:
  beszel:
    image: henrygd/beszel:latest
    container_name: beszel
    restart: unless-stopped
    environment:
      APP_URL: http://pi:8090
    ports:
      - 8090:8090
    volumes:
      - ./docker_volumes/beszel_data:/beszel_data
      - ./docker_volumes/beszel_socket:/beszel_socket
```

Run the docker serivce and open the application in a browser.

Create a Beszel account.

Add a system. It gives you a TOKEN and a KEY.
Add in .env file, two new variables:
``` bash
BESZEL_AUTH_TOKEN=<TOKEN>
BESZEL_AUTH_KEY=<KEY>
```

Deploy the agent by adding into the docker-compose a new service:
```
services:
  beszel-agent:
    image: henrygd/beszel-agent:latest
    container_name: beszel-agent
    restart: unless-stopped
    network_mode: host
    volumes:
      - ./docker_volumes/beszel_agent_data:/var/lib/beszel-agent
      - ./docker_volumes/beszel_socket:/beszel_socket
      - /var/run/docker.sock:/var/run/docker.sock:ro
    environment:
      LISTEN: /beszel_socket/beszel.sock
      HUB_URL: http://pi:8090
      TOKEN: ${BESZEL_AUTH_TOKEN}
      KEY: ${BESZEL_AUTH_KEY}
```

