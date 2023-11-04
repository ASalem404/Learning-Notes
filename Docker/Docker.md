- Docker Notes :

  - Dockerfile:
    A Dockerfile is a text document that contains a set of instructions for building a Docker image. Docker images are used to create lightweight and portable containers that can run applications in isolated environments. Dockerfiles provide a way to define the environment and dependencies for your application or service. Here are some basic Dockerfile commands and their explanations:

    1.  FROM:

    - Syntax: `FROM <base_image>`
    - Description: Specifies the base image from which you want to build your image. Every Dockerfile must start with a `FROM` instruction. The base image can be an official image from Docker Hub or a custom image you've created.

    2.  MAINTAINER (deprecated):

    - Syntax: `MAINTAINER <author_name>`
    - Description: Specifies the author or maintainer of the Dockerfile. This command is now deprecated in favor of using labels to provide metadata.

    3.  LABEL:

    - Syntax: `LABEL <key>=<value> <key>=<value> ...`
    - Description: Adds metadata to the image, such as version information, author, or any other key-value pairs that can be used for documentation or organization.

    4.  RUN:

    - Syntax: `RUN <command>`
    - Description: Executes a command in the shell of the container during the image build process. The result of this command is committed to the image as a new layer.

    5.  COPY and ADD:

    - Syntax:
    - `COPY <source> <destination>`
    - `ADD <source> <destination>`
    - Description: These commands copy files or directories from the host machine into the image.
      `COPY` is generally preferred for simple file copying,
      while `ADD` can do more advanced tasks like URL downloads and extracting tar archives.

    6.  WORKDIR:

    - Syntax: `WORKDIR /path/to/directory`
    - Description: Sets the working directory within the container for any subsequent instructions.
      This makes it easier to navigate the file system in your Dockerfile.

    7.  EXPOSE:

    - Syntax: `EXPOSE <port>`
    - Description: Informs Docker that the container will listen on the specified network port at runtime.
      This command doesn't actually publish the port; it's just documentation for the user.

    8.  CMD:

    - Syntax: `CMD ["executable", "param1", "param2"]` or `CMD command param1 param2`
    - Description: Provides the default command and parameters for the container when it's started.
      You can only have one `CMD` instruction in a Dockerfile. If a container is run with a command, it will override this default.

    9.  ENTRYPOINT:

    - Syntax: `ENTRYPOINT ["executable", "param1", "param2"]` or `ENTRYPOINT command param1 param2`
    - Description: Specifies the executable that will be run when the container starts.
      Unlike `CMD`, `ENTRYPOINT` cannot be overridden by providing a command when running the container.

    10. VOLUME:

    - Syntax: `VOLUME ["/data"]`
    - Description: Creates a mount point for external volumes or other containers to connect to.
      It is typically used to manage data that needs to persist across container instances.

  - Docker Compose
    Docker Compose is a tool for defining and running multi-container Docker applications. It uses a YAML file (usually named `docker-compose.yml`) to define the services, networks, and volumes that make up your application stack.

    1. `docker-compose up`:

       - Syntax: `docker-compose up [options] [SERVICE...]`
       - Description: This command creates and starts containers for all services defined in your `docker-compose.yml` file. If you don't specify any services, it will start all of them. It also builds images if they don't exist and creates the associated networks and volumes.

    2. `docker-compose down`:

       - Syntax: `docker-compose down [options]`
       - Description: This command stops and removes containers, networks, and volumes created by `docker-compose up`. It effectively tears down the entire application stack.

    3. `docker-compose build`:

       - Syntax: `docker-compose build [SERVICE...]`
       - Description: Use this command to rebuild the images for the specified services. It's useful when you've made changes to your application code or Dockerfiles and want to update the images without starting the containers.

    4. `docker-compose start`:

       - Syntax: `docker-compose start [SERVICE...]`
       - Description: Starts existing containers for the specified services without recreating them. It's useful when you've previously stopped the containers and want to start them again.

    5. `docker-compose stop`:

       - Syntax: `docker-compose stop [SERVICE...]`
       - Description: Stops running containers for the specified services without removing them. This allows you to halt the services without tearing down the entire application stack.

    6. `docker-compose ps`:

       - Syntax: `docker-compose ps [SERVICE...]`
       - Description: Lists the status of containers for the specified services. It shows whether the containers are running, stopped, or exited, along with other information.

    7. `docker-compose logs`:

       - Syntax: `docker-compose logs [SERVICE...]`
       - Description: Displays the logs generated by the services. You can specify one or more services to view their logs, or you can omit the service name to view logs for all services in the stack.

    8. `docker-compose exec`:

       - Syntax: `docker-compose exec [SERVICE] command [ARGS...]`
       - Description: Allows you to run a command in a running container. You can specify the service name and the command to be executed in that container.

    9. `docker-compose pull`:
       - Syntax: `docker-compose pull [SERVICE...]`
       - Description: Pulls the latest images for the specified services from their respective registries. This is useful for updating images without restarting the containers.

    These are some of the basic Docker Compose commands.
    Docker Compose provides a convenient way to manage multi-container applications, define their dependencies, and orchestrate their deployment and scaling.
    You can use these commands to interact with your Docker Compose-managed application stack.

  - Docker Volumes in mounting
    Docker volumes are commonly used for mounting data from the host system into containers or for sharing data between containers. Here's an example of how to use a Docker volume to mount data from the host system into a container.

    Let's say you have a directory on your host system that contains some configuration files, and you want to make these files available to a Docker container.

    1. Create a Docker volume:

       - You can create a named volume using the `docker volume create` command or allow Docker to create an anonymous volume for you.

       ```bash
       # Create a named volume
       docker volume create my-config

       # Alternatively, you can skip creating a named volume and let Docker create an anonymous volume for you when you run the container.
       ```

    2. Run a container and mount the volume:

       - Use the `-v` or `--volume` option to specify the volume you want to mount and the path within the container where you want the data to be accessible.

       ```bash
       # Run a container and mount the 'my-config' volume
       docker run -d -v my-config:/app/config my-container
       ```

       In this example, the `my-config` volume is mounted into the `/app/config` directory inside the container.

    3. Access the data within the container:

       - Now, any files placed in the directory on your host system that corresponds to the `my-config` volume will be accessible within the container at the specified mount path.

       For example, if you have a `config.txt` file in the directory on your host system that corresponds to the `my-config` volume, you can access it within the container at `/app/config/config.txt`.

    4. Modify and use the data in the container:

       - You can read, modify, or use the data in the mounted volume as needed. Any changes made within the container will be reflected in the volume, and those changes will be available on the host system as well.

    5. Clean up:

       - Volumes are not removed automatically when you remove a container. You can remove the container without affecting the volume. To clean up the volume when you're done with it, you can use the `docker volume rm` command.

       ```bash
       # Remove the volume
       docker volume rm my-config
       ```

    Using Docker volumes in this way allows you to manage configuration files, data, and other resources in a more structured and persistent manner. It also makes it easy to share data between containers and to update the data on the host system while the container is running.
