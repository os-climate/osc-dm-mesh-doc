# Building the Platform

## Prerequisites

There are a few requirements for your environment:
- Docker and Docker Compose is used as a run-time environment; All
of the services are packaged as containers and we use Docker Compose
as a simple way for you to get started (in a production setting
we use OpenShift, Kubernetes, and any cloud-variant of Kubernetes)
- If you are using the CLI (and you probably will), then Python
must be available, preferably in a virtual environment (venv)

### Some Basic Preparation

We provide several scripts that make it easier to build and run
the Ecosystem Platform components.  They do, however, expect a small set
of environment variables:
- HOME_DIR: This points to your workspace top level directory; Components
will be cloned into this directory, and all component scripts expect this
to be in inplace fr build and run scripts to work
- DOCKERHUB_USERNAME: Your docker user name which is used to specify your Docker images
- DOCKERHUB_TOKEN: A valid Docker Hub token to publish your images (note
that this is OPTIONAL and is not required unless you wish to publish your
docker images to a public image repository)
- SAMPLES_DIR: This points to the directory containing sample data;
This is only required for a few components and will be note where needed

So, set "HOME_DIR" environment variable (typically in your
personal enviornment script) to your workspace top level.

For example, I use bash/zsh as my shell environment/tool
and I put the following into my "~/.zshrc" (as shown below,
under my personal home directory, I have
a "Development/scratch" directory, but you can change this
to anything you wish):
~~~~
export HOME_DIR="~/Development/scratch"
~~~~

### Cloning GitHub Repos

All of the components have their own GitHub repo:
- osc-dm-product-srv: Ecosystem Platform Data product agent server
- osc-dm-search-srv: Ecosystem Platform search server
- osc-dm-registrar-srv: Ecosystem Platform registrar
- osc-dm-marketplace-ux: Ecosystem Platform marketplace user interface
- osc-dm-proxy-srv: Ecosystem Platform proxy server
- osc-dm-monitor-srv: Ecosystem Platform monitor services

Clone each of the components from their GitHub repo into your
workspace top level directory (ie. HOME_DIR).

After cloning any component, each component should be located in $HOME_DIR/{component}.
So, if you have cloned all of the components listed above,
your directory structure will look liken the following:
~~~~
$HOME_DIR
    +- osc-dm-product-srv
    +- osc-dm-search-srv
    +- osc-dm-registrar-srv
    +- osc-dm-marketplace-ux
    +- osc-dm-proxy-srv
    +- osc-dm-monitor-srv
~~~~

For the purposes of this document we call each component directory the
"component workspace" directory.

### Build the Docker Containers

Start a new terminal window (we do this since environment variables
are local/specific to your terminal window which makes it easier
to run our scripts).  Go to the component workspace directory.

You will find that each component has a similar directory structure.
In each component's "bin" directory there are several scripts that will
setup your environment and build the Docker containers.

The build process is identical for each component unless otherwise noted.
So, **for each component**:
- setup your environent
- setup Python virtual environment and install libraries
- build the Docker containers

For each component, all of these actions will be executed in
the cloned directory.

#### Component: Setting up your Environment

Go to (or stay in) the component workspace directory (and terminal window).

Some environment variables are used by various source code and scripts.
Setup your environment as follows (note that "source" is used)
~~~~
source ./bin/environment.sh
~~~~

After running this script, you will be shown your current environment

This script will setup environment variables for
each project:
- PROJECT: the name of the project (for example: osc-dm-registrar-srv)
- PROJECT_DIR: the component workspace directory (for example: $HOME_DIR/osc-dm-registrar-srv)

#### Component: Setting up Python Virtual Environment

Go to (or stay in) the component workspace directory (and terminal window).

It is recommended that a Python virtual environment be created.
We have provided several convenience scripts to create and activate
a virtual environment. To create a new virtual environment using
these convenience scripts, execute the following (this will
create a directory called "venv" in your current working directory):
~~~~
$PROJECT_DIR/bin/venv.sh
~~~~

Once your virtual enviornment has been created, it can be activated
as follows (note: you *must* activate the virtual environment
for it to be used, and the command requires "source" to ensure
environment variables to support venv are established correctly):
~~~~
source $PROJECT_DIR/bin/vactivate.sh
~~~~

Install the required libraries as follows:
~~~~
pip install -r requirements.txt
~~~~

#### Component: Build the Docker Container

Go to (or stay in) the component workspace directory (and terminal window).

Make sure Docker is running and available.

Build the docker container (docker will need to be running) as follows.
This will build the container in your local environment which is what
we want for now (the script can also publish the container to DockerHub
but that is for another tutorial):
~~~~
$PROJECT_DIR/bin/dockerize.sh
~~~~

You should see a docker image for the component in your
local docker environment (your list may vary depending on
the images you currently have setup):
~~~~
docker images

REPOSITORY                                 TAG       IMAGE ID       CREATED        SIZE
brodagroupsoftware/osc-dm-search-srv       0.0.1     03c683fe574a   19 hours ago   1.43GB
brodagroupsoftware/osc-dm-search-srv       latest    03c683fe574a   19 hours ago   1.43GB
<none>                                     <none>    32c24016838e   19 hours ago   1.43GB
brodagroupsoftware/osc-dm-marketplace-ux   0.0.1     89f713cfad58   20 hours ago   1.95GB
brodagroupsoftware/osc-dm-marketplace-ux   latest    89f713cfad58   20 hours ago   1.95GB
brodagroupsoftware/osc-dm-product-srv      0.0.1     78b65b5a1aa3   21 hours ago   246MB
brodagroupsoftware/osc-dm-product-srv      latest    78b65b5a1aa3   21 hours ago   246MB
brodagroupsoftware/osc-dm-registrar-srv    0.0.1     8d26b2ccc496   23 hours ago   273MB
brodagroupsoftware/osc-dm-registrar-srv    latest    8d26b2ccc496   23 hours ago   273MB
brodagroupsoftware/osc-dm-proxy-srv        0.0.1     7cb6bb2e9d70   40 hours ago   272MB
brodagroupsoftware/osc-dm-proxy-srv        latest    7cb6bb2e9d70   40 hours ago   272MB
brodagroupsoftware/osc-dm-monitor-srv      0.0.1     09bd25968e77   3 days ago     286MB
brodagroupsoftware/osc-dm-monitor-srv      latest    09bd25968e77   3 days ago     286MB
<none>                                     <none>    1ab28a6515b6   4 days ago     1.95GB
<none>                                     <none>    f08834c5d783   4 days ago     273MB
quay.io/coreos/etcd                        v3.5.0    a7908fd5fb88   2 years ago    110MB
~~~~

## Setup the CLI

The Ecosystem Platform has a CLI (Command Line Interface)
which exposes almost all capabilities from a terminal.

We will use the CLI to get some of our compnents setup,
so let's get it installed.

First, clone the CLI [osc-dm-mesh-cli](https://github.com/os-climate/osc-dm-mesh-cli).

Next, setup your environment as follows (note that "source" is used)
~~~~
source ./bin/environment.sh
~~~~

Next, create a new virtual environment using
these convenience scripts, execute the following (this will
create a directory called "venv" in your current working directory):
~~~~
$PROJECT_DIR/bin/venv.sh
~~~~

Activate the virtual environment:
~~~~
source $PROJECT_DIR/bin/vactivate.sh
~~~~

Install the required libraries as follows:
~~~~
pip install -r requirements.txt
~~~~

Install pytest:
~~~~
pip install pytest
~~~~

## Test Cases

Go to your CLI terminal (and run your ./bin/environment.sh)
and run any of the available test cases (in the "tests"
directory):
- test_cli_product_generate.py: Generate UUIDs for product and artifacts
- test_cli_users.py: Register and view users
- test_cli_product.py: Register and view products
- test_cli_discovery.py: View information about a data product
- test_cli_carts_orders.py: Register carts and "purchase" orders

Running a test case is done as follows:
~~~~
pytest ./tests/test_cli_users.py
~~~~

