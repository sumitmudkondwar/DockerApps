# How Port mapping works in Docker


Port mapping in Docker allows you to expose a container's internal ports to your host machine or external systems. This is essential when a service inside a container (like a web server) needs to be accessible outside the container. Here’s how it works:

### 1. **Ports Inside Containers:**
Each Docker container has its own isolated network. This means that containers don’t expose ports to the outside world by default. For example, if a web server is running inside a container on port `80`, it’s only accessible within that container’s network.

### 2. **Port Mapping Syntax:**
The Docker port mapping syntax in the `docker run` command is:

```bash
-p <host_port>:<container_port>
```

- **`host_port`**: The port on your host machine (or the external machine) that you want to expose.
- **`container_port`**: The port inside the container that your application is listening on.

### 3. **How It Works:**

When Docker starts a container, it sets up a network bridge between the container and the host machine. Port mapping binds a specific port on the host to a port on the container using this bridge.

- **Example:** If you have an application running in a container on port `80`, and you want to access it on your host machine via port `9000`, you would use the following command:

```bash
docker run -p 9000:80 <image_name>
```

Now, any traffic sent to `localhost:9000` on your host machine will be forwarded to port `80` inside the container.

### 4. **Docker’s Networking Modes:**
- **Bridge mode (default):** The container is connected to the Docker bridge network, and port mappings are required to access services from outside.
- **Host mode:** The container uses the host’s network stack directly. In this case, no explicit port mapping is needed because the container shares the host’s ports directly (useful for performance but less isolated).
- **None:** The container is isolated from external networks and has no external connectivity.

### 5. **Multiple Port Mappings:**
You can map multiple ports in the same container by using additional `-p` options.

```bash
docker run -p 8080:80 -p 443:443 <image_name>
```

In this case, port `8080` on the host forwards to port `80` in the container, and port `443` on the host forwards to port `443` in the container.

### 6. **Port Ranges:**
You can also map a range of ports using this syntax:

```bash
docker run -p 8000-8100:80 <image_name>
```

This maps host ports `8000` to `8100` to container port `80`.

### 7. **IP Binding (Optional):**
By default, Docker binds the mapped port to all IP addresses on the host (0.0.0.0), but you can specify a particular IP address on the host to bind to, like this:

```bash
docker run -p 127.0.0.1:8080:80 <image_name>
```

This will bind the port mapping only to `localhost`, making the service accessible only from the host machine itself.

### Summary:
Port mapping is a way to make applications inside Docker containers accessible from the outside world. You map a port on the host to a port inside the container, allowing traffic to flow between them via the Docker network bridge.