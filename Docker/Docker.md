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
