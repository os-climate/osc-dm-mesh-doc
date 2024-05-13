# Getting Started

## Prerequisites

There are a few requirements for your environment:
- Docker and Docker Compose is used as a run-time environment; All
of the services are packaged as containers and we use Docker Compose
as a simple way for you to get started (in a production setting
we use OpenShift, Kubernetes, and any cloud-variant of Kubernetes)
- If you are using the CLI (and you probably will), then Python
must be available, preferably in a virtual environment (venv)

## Starting the Ecosystem Platform

Docker images are currently not available via OS-Climate, so
we will use the original Docker images available on DockerHub.
The images are available under the username: "brodagroupsoftware".

If you wish to build the system and create your own
Docker images then follow the [Buiding the Ecosystem Guide](/docs/building-platform.md).

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

## Bootstrapping the Ecosystem Platform

The Ecosystem Platform can be started using the following
commands which will pull all necessary Docker images (under
the "brodagroupsoftware" username).

First, clone the platform run-time components "bgs-dm-mesh-srv".
~~~~
git clone https://github.com/brodagroupsoftware/bgs-dm-mesh-srv.git
~~~~

Next, change your current working directory to the
created directory.
~~~~
cd bgs-dm-mesh-srv
~~~~

Now, setup your environment:
~~~~
source ./bin/environment.sh
~~~~

This will setup several environment variables that are used by
the platform:
~~~~
USER_NAME:          ericbroda
HOME_DIR:           /Users/ericbroda/Development/scratch
DOCKERHUB_USERNAME: brodagroupsoftware
DOCKERHUB_TOKEN:    ****
ROOT_DIR:           /Users/ericbroda/Development/scratch
PROJECT:            bgs-dm-mesh-srv
PROJECT_DIR:        /Users/ericbroda/Development/scratch/bgs-dm-mesh-srv
PROTOCOL_BUFFERS_PYTHON_IMPLEMENTATION: python
~~~~

Finally, start the platform:
~~~~
$PROJECT_DIR/app/startd.sh
~~~~

This will get the core components running and available. You should
see several Docker images running:
~~~~
docker ps

CONTAINER ID   IMAGE                                             COMMAND                  CREATED         STATUS         PORTS                              NAMES
42ab62c65dd7   brodagroupsoftware/bgs-dm-search-srv:latest       "python3 /app/server…"   4 seconds ago   Up 2 seconds   0.0.0.0:23000->8000/tcp            docker-bgs-dm-search-srv-1
1716d89280c8   brodagroupsoftware/bgs-dm-registrar-srv:latest    "python3 /app/server…"   4 seconds ago   Up 2 seconds   0.0.0.0:21000->8000/tcp            docker-bgs-dm-registrar-srv-1
df7f9487cd67   brodagroupsoftware/bgs-dm-proxy-srv:latest        "python3 /app/server…"   5 seconds ago   Up 2 seconds   0.0.0.0:20000->8000/tcp            docker-bgs-dm-proxy-srv-1
06476365d0d5   brodagroupsoftware/bgs-dm-marketplace-ux:latest   "docker-entrypoint.s…"   5 seconds ago   Up 2 seconds   0.0.0.0:3000->3000/tcp             docker-bgs-dm-marketplace-ux-1
a0448815f89b   quay.io/coreos/etcd:v3.5.0                        "/usr/local/bin/etcd"    5 seconds ago   Up 3 seconds   0.0.0.0:2379-2380->2379-2380/tcp   docker-etcd1-1
5c34be81afa3   quay.io/coreos/etcd:v3.5.0                        "/usr/local/bin/etcd"    5 seconds ago   Up 3 seconds   2379-2380/tcp                      docker-etcd2-1
3f6ad9e74091   quay.io/coreos/etcd:v3.5.0                        "/usr/local/bin/etcd"    5 seconds ago   Up 3 seconds   2379-2380/tcp                      docker-etcd3-1
~~~~

Next, we will bootstrap a data product.

## Bootstrapping a Data Product

First, clone the data product run-time components "bgs-dm-meshdp-srv"
(note the name of the repo is close, but different from the previous
repo).
~~~~
git clone https://github.com/brodagroupsoftware/bgs-dm-meshdp-srv.git
~~~~

Next, change your current working directory to the recently
created directory.
~~~~
cd bgs-dm-meshdp-srv
~~~~

Now, setup your environment:
~~~~
source ./bin/environment.sh
~~~~

This will setup several environment variables that are used by
the platform:
~~~~
--- Environment ---
USER_NAME:          ericbroda
HOME_DIR:           /Users/ericbroda/Development/scratch
DOCKERHUB_USERNAME: brodagroupsoftware
DOCKERHUB_TOKEN:    ****
ROOT_DIR:           /Users/ericbroda/Development/scratch
PROJECT:            bgs-dm-meshdp-srv
PROJECT_DIR:        /Users/ericbroda/Development/scratch/bgs-dm-meshdp-srv
DATA_DIR:           /Users/ericbroda/Development/scratch/bgs-dm-samples-dat
~~~~

Data products are define in directory defined by the "DATA_DIR"
environment variable.  Instances are used to uniquely identify a
data product, and the data product identifier is used to specify
the configuration in DATA_DIR.

Finally, start the data product as follows, using instance 0
and data product "rmi":
~~~~
$PROJECT_DIR/app/startd.sh 0 rmi
~~~~

Now, you may see a few errors in the log for the data product.
That is OK.  Do not panic!  The reason this is happening
is that we have bootstrapped services but we have not registered
information (user, products etc) that are used to create
a healthy system.  We will use the test cases (next section)
to register information while also testing core system capabilities.

At this point one additional Docker container will be
running ("docker-bgs-dm-product-srv-1"):
~~~~
docker ps

CONTAINER ID   IMAGE                                             COMMAND                  CREATED              STATUS              PORTS                              NAMES
4512a8463445   brodagroupsoftware/bgs-dm-product-srv:latest      "python3 /app/server…"   5 seconds ago        Up 3 seconds        0.0.0.0:24000->8000/tcp            docker-bgs-dm-product-srv-1
42ab62c65dd7   brodagroupsoftware/bgs-dm-search-srv:latest       "python3 /app/server…"   About a minute ago   Up About a minute   0.0.0.0:23000->8000/tcp            docker-bgs-dm-search-srv-1
1716d89280c8   brodagroupsoftware/bgs-dm-registrar-srv:latest    "python3 /app/server…"   About a minute ago   Up About a minute   0.0.0.0:21000->8000/tcp            docker-bgs-dm-registrar-srv-1
df7f9487cd67   brodagroupsoftware/bgs-dm-proxy-srv:latest        "python3 /app/server…"   About a minute ago   Up About a minute   0.0.0.0:20000->8000/tcp            docker-bgs-dm-proxy-srv-1
06476365d0d5   brodagroupsoftware/bgs-dm-marketplace-ux:latest   "docker-entrypoint.s…"   About a minute ago   Up About a minute   0.0.0.0:3000->3000/tcp             docker-bgs-dm-marketplace-ux-1
a0448815f89b   quay.io/coreos/etcd:v3.5.0                        "/usr/local/bin/etcd"    About a minute ago   Up About a minute   0.0.0.0:2379-2380->2379-2380/tcp   docker-etcd1-1
5c34be81afa3   quay.io/coreos/etcd:v3.5.0                        "/usr/local/bin/etcd"    About a minute ago   Up About a minute   2379-2380/tcp                      docker-etcd2-1
3f6ad9e74091   quay.io/coreos/etcd:v3.5.0                        "/usr/local/bin/etcd"    About a minute ago   Up About a minute   2379-2380/tcp                      docker-etcd3-1
~~~~

## Setup the CLI

The Ecosystem Platform has a CLI (Command Line Interface)
which exposes almost all capabilities from a terminal.

We will use the CLI to get some of our compnents setup,
so let's get it installed.

First, clone the CLI [bgs-dm-mesh-cli](https://github.com/brodagroupsoftware/bgs-dm-mesh-cli).

Next, setup your environment as follows (note that "source" is used)
~~~~
source ./bin/environment.sh 0 rmi
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

## Registering Users (including a Publisher)

The CLI documentation explains how to issue commands to
the Ecosystem Platform and its data products.  But to
make this easy and convenient, we will just use some
of the test cases to register users and products (you
can run all test cases by following instructions
later in this document).

Go to your CLI ("bgs-dm-mesh-cli") directory (ensure
your environment is setup) and then execute the following
test case:
~~~~
pytest ./tests/test_cli_users.py
~~~~

After this is run, your data product registration error
will go away within a minute, and your data product will
be fully registered and ready!

## Viewing Data Product in the Bazaar

Assuming you have followed the tutorial this far, then
the Ecosystem Platform will be running and a sample data product will
be registered.

Now, let's see if we can see it in our Bazaar (Marketplace). Open your
favorite browser and point to the following link:
~~~~
http://localhost:3000
~~~~

You should see a "Login" screen that looks something like the following:

![UX Login](/images/UX-Login.png)

Login with the following credentials:
- Email Address / Username:  subscriber.user@brodagroupsoftware.com
- Password: anything you want (we do not have security enabled at this point)
- Role: Subscriber (selected from the dropdown list)

Now, you should see a "Marketplace" screen that looks something like the following:

![UX Marketplace](/images/UX-Marketplace.png)

At this point we only have a limited number of sample data products.
Find one of the data product "tiles" that interest you and click
on "VIEW" (at the bottom of the tile).

Now, you should see an "Product" screen for the selected product,
that looks something like the following:

![UX Product](/images/UX-Product.png).

Click on the "VIEW" at the bottom of the
first Artifact tile.

Now, you should see an "Artifact" screen for the selected artifact,
that looks something like the following:

![UX Artifact](/images/UX-Artifact.png)

Click on "ADD TO CART" and you should see a small notification show up
at the bottom of the screen indicating that the artifact has been
added to your cart.

Click on the "CART" at the top left part of the screen in the header. You
should see the contents of your cart containin the artifact that you just
added to the cart.  Your screen will look something like this:

![UX Cart](/images/UX-Cart.png)

Now, lets "purchase" the items in the cart (note: there is no "purchase"
and nothing is bought.  Rather, we use a shopiing cart metaphor to
retain useful artifacts in an order for future reference).  Click
on "ORDER CART" and you should see an empty cart shown, meaning that
the order has completed and you have no items in the cart anymore.

Click on "ORDERS" on the left in the header at the top of the screen.

![UX Order](/images/UX-Order.png)

There are several options in the header at the top of the screen that
helps navigation:
- MARKETPLACE: presents the Marketplace screen
- DASHBOARD: Not implemented yet, but is intended to be a summary
of your products, carts, and activity
- SEARCH: Not implemented yet, but is intended to provide natural
language search to find/filter data products

At any point you can logout by clicking "LOGOUT" at the top left of
the header at the top of the screen.

![UX Logout](/images/UX-Logout.png)

## Testing the Ecosystem Platform

Go to your CLI terminal.  There are several test cases (in the "tests" directory):
- test_cli_product_generate.py: Generate UUIDs for product and artifacts
- test_cli_users.py: Register and view users
- test_cli_product.py: Register and view products
- test_cli_discovery.py: View information about a data product
- test_cli_carts_orders.py: Register carts and "purchase" orders

Run each of the above test cases (except test_cli_product_generate.py
as this is already setup with sample data)