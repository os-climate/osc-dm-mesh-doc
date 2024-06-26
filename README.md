
> [!IMPORTANT]
> On June 26 2024, Linux Foundation announced the merger of its financial services umbrella, the Fintech Open Source Foundation ([FINOS](https://finos.org)), with OS-Climate, an open source community dedicated to building data technologies, modeling, and analytic tools that will drive global capital flows into climate change mitigation and resilience; OS-Climate projects are in the process of transitioning to the [FINOS governance framework](https://community.finos.org/docs/governance); read more on [finos.org/press/finos-join-forces-os-open-source-climate-sustainability-esg](https://finos.org/press/finos-join-forces-os-open-source-climate-sustainability-esg)

# osc-dm-mesh-doc - OS-Climate Ecosystem Platform

Welcome to [OS Climate's](https://os-climate.org/) Ecosystem platform!

We are part of something called [Project-X](/docs/about-project-x.md), which is an
OS-Climate project under the "Data Commons" work
package.  Our objective is to make climate data easier to find, consume
share, and trust.

This repo is the starting point for understanding Project-X components,
which are included under what we call our
[Ecosystem Platform](/docs/about-ecosystem-platform.md).  It
contains several functional units:
- Climate Data Bazaar: A catalog for climate data which makes
climate data easy to find, consume, share, and trust
- Geospatial Index services: Services that provide a uniform grid index
for geospatial data (ie. for data that has a location, or latitude and longitude)
- Several sample "data products" that make climate data accessible
in the Bazaar
- Several geospatial sample "data products" that illustrate
use of location-based data

The Ecosystem Platform was originally developed by Broda Group Software
with key contributions by:
- [Eric Broda](https://www.linkedin.com/in/ericbroda/)
- [Davis Broda](https://www.linkedin.com/in/davisbroda/)
- [Graeham Broda](https://www.linkedin.com/in/graeham-broda-3a2294b3/)

If you just want to try out the Ecosystem Platform then the best place
to start is in our [Getting Started](/docs/getting-started.md) tutorial.

If you are a developer and wish to modify the code and create
your own Docker images (which we encourage, after all this is open
source software), then follow the instructions in the
[Building the Platform](/docs/building-platform.md) tutorial.

If you want to see where we are going with the platform, then check
out our [Roadmap](/docs/roadmap.md) (note this is still under construction
and will be available soon).

If you want to participate in setting the direction for the platform,
or just want to talk to the developers, then we have the place for you.
We get together regularly and all of the meeting details are on the
[OS-Climate event calendar](https://west.exch092.serverdata.net/owa/calendar/f55f275b1e724cc49b5a52f50c30a11f@os-climate.org/022d1c0017744eebbf9f14f737493bd67046415453482209411/calendar.html)
(look for "Project-X" on Thursdays at 9:30 EST).  We would love to talk with you!

If you have any feedback at all, then we have a few venues that
let you interact with us:
- [OS-Climate Slack](https://os-climate.slack.com/)
- GitHub Issues for the project or component (we will try to
respond quickly to any issues you find)
- LinkedIn: You can find me (Eric Broda, Leader for this project)
on LinkedIn at: https://www.linkedin.com/in/ericbroda/
and I would be happy to respond if you DM me.

Otherwise, there is a bunch of stuff below that tells you about our
platform, the problems it solves, and the architecture and components
that make it all happen.

Enjoy!

## Ecosystem Platform Components

This repo is contains or references the consolidated documentation for our
Ecosystem Platform components.  There are several components, each with
their source code, that are available:
- osc-dm-mesh-srv: Execution environment for Ecosystem Platform
- osc-dm-meshdp-srv: Execution environment for Ecosystem Platform Data Products
- osc-dm-mesh-cli: Command language interface to interact with Ecosystem Platform
components using a terminal window command line
- osc-dm-product-srv: Ecosystem Platform Data product agent server
- osc-dm-search-srv: Ecosystem Platform search server
- osc-dm-registrar-srv: Ecosystem Platform registrar
- osc-dm-marketplace-ux: Ecosystem Platform marketplace user interface
- osc-dm-proxy-srv: Ecosystem Platform proxy server
- osc-dm-monitor-srv: Ecosystem Platform monitor services
- osc-dm-samples-dat: Sample data, primarily for Ecosystem Platform data products
- osc-dm-utilities-lib: Library of common utilities used by Ecosystem Platform components

The Geospatial components are defined in:
- bgs-geo-h3index-srv: Geospatial index server based upon H3 index schemes

Introductions for each component follows below as well as links to
further documentation for each component.

### osc-dm-mesh-srv: Ecosystem Platform Execution Environment

[osc-dm-mesh-srv](https://github.com/os-climate/osc-dm-mesh-srv)
is the runtime environment for the Ecosystem Platform, implemented using Docker Compose,
starts up several components, including the Registry UX, Registry
Service, and various Data Products necessary for data management within
the mesh.

A quick note on our Docker Compose execution environment. Every
runnable component has a Docker image which means that we can support
just about any on-premises or cloud-native environment.  But with a goal
of geting our platform running in a few minutes
on a local machines, we have offered a simple run-time environment based
upon docker which is prevelent in most environments (and is easily downloaded)
that can run easily and quickly on most laptops.

### osc-dm-meshdp-srv: Ecosystem Platform Data Product Execution Environment

[osc-dm-meshdp-srv](https://github.com/os-climate/osc-dm-meshdp-srv)
is the runtime environment for the Ecosystem Platform data products.  It
is implemented using Docker Compose (see above for reason for
Docker Compose support) and starts up a Data Products.

### osc-dm-mesh-cli: Ecosystem Platform Command Language Interface

[osc-dm-mesh-cli](https://github.com/os-climate/osc-dm-mesh-cli)
is the CLI for the Ecosystem Platform Registry provides a suite of capabilities that
facilitate the management and interaction with a Ecosystem Platform environment.
Here are the key functions and operations it supports:

1. **User Management:**
   - Register different types of users (administrators, publishers,
   subscribers, guests), each with distinct privileges
   - View user details and manage user authentication and sessions

2. **Data Product Management:**
   - Register and configure data products
   - View and manage data product configurations
   - Generate unique identifiers (UUIDs) for new data products and their artifacts

3. **Artifact Management:**
   - View and retrieve details about artifacts, and manage artifact-specific
   settings and metadata, directly from data product

4. **Carts and Orders:**
   - Manage shopping carts for subscribers, allowing them to add and remove
   data products and artifacts.
   - Process orders, converting carts into finalized purchases.

5. **Search:**
   - Perform natural language searches within the Ecosystem Platform
   to discover products and artifacts based on specified criteria

### osc-dm-product-srv: Ecosystem Platform Data Product Agent (server)

[osc-dm-product-srv](https://github.com/os-climate/osc-dm-product-srv)
is a FastAPI server designed for managing data products within a data
mesh environment offers a comprehensive suite of functionalities to
streamline data operations across an interconnected network.

This server enables users to register new data products and access
metadata about data products. Soon it will offer observability, usage,
and management capabilities, enabling
administrators to modify system information or data products.
In some respects, this server acts as a hub for managing the
lifecycle of a data product.

### osc-dm-search-srv: Ecosystem Platform Search Server

[osc-dm-search-srv](https://github.com/os-climate/osc-dm-search-srv)
is a FastAPI-based search service for our Ecosystem Platform.
This service leverages a Vector database to facilitate natural language
similarity searches, allowing users to find content that closely matches
their queries in semantic meaning rather than just keyword alignment.
It is designed to keep its content store updated with the most
current and recent data product information.

### osc-dm-registrar-srv: Ecosystem Platform Registrar

[osc-dm-registrar-srv](https://github.com/os-climate/osc-dm-registrar-srv)
is a FastAPI service for a Ecosystem Platform that is designed to
handle the registration of, access to, and management of users,
products, carts, and orders, consistently managing crucial operational
data in one platform.

Utilizing ETCD as its underlying data store, the service benefits
from ETCD's strong consistency, high availability, and key-value
storage capabilities, ensuring a scalable and reliable infrastructure.

### osc-dm-marketplace-ux: Ecosystem Platform Marketplace User Interface

[osc-dm-marketplace-ux](https://github.com/os-climate/osc-dm-marketplace-ux)
is a React/MUI-based user interface for the Ecosystem Platform environment
presents a "marketplace" style platform that revolutionizes the
way users access and interact with products, carts, and orders.

By adopting a familiar shopping metaphor, users can "buy"
data products, adding them to carts and processing orders.  Note
nothing is actually bought, rather we use a "shopping cart" metaphor for data
requests - this way the orders can be used to represent
past, or presumably frequently used, data which can be easily
accessed and consumed.

One quick comment on our UX styling.  Yes, it is minimal on purpose.
And, as a typical React/MUI application, it is quite easy to customize
the look and feel. So, we do expect to customize it a
bit (logos, colours, icons etc) but actually want folks that
download and use the kit to do much of the look-and-feel
customization based upon their needs (but we do hope the share
it with us at some point!).

### osc-dm-proxy-srv: Ecosystem Platform Proxy Service

[osc-dm-proxy-srv](https://github.com/os-climate/osc-dm-proxy-srv)
is a FastAPI service that acts as a proxy/router for Ecosystem Platform service
requests.  It is the primary interface point for external UX functionality
and provides dynamic routing to a ecosytem of virtually unlimited
number of data products.

### osc-dm-monitor-srv: Ecosystem Platform Monitor Service

[osc-dm-monitor-srv](https://github.com/os-climate/osc-dm-monitor-srv)
is a FastAPI service that monitors components in the Ecosystem Platform.
It occasionally pings core services as well as active data
products to gather health information and operational metrics. This
data will soon be available on both our CLI and UX.

### osc-dm-samples-dat: Sample Data for Ecosystem Platform

[osc-dm-samples-dat](https://github.com/os-climate/osc-dm-samples-dat)
is a repository of sample data that populates the registrar and various data
products within a Ecosystem Platform environment, integrating with
both the marketplace UX and the Ecosystem Platform CLI.  These
samples are offered to allow users to see data product capabilities
and understand data product concepts.  And, it provides the necessary
data to support a simple and outstanding "Getting Started" experience.

This repository includes a small subset of pre-configured datasets
that demonstrate core data product capabilities, including:
- RMI: utilities data
- PUDL: details emissions data for utilities
- And, soon, many others.

### osc-dm-utilities-lib: Library of Common Utilities for Ecosystem Platform

NOT YET IMPLEMENTED (link not active)

[osc-dm-utilities-lib](https://github.com/os-climate/osc-dm-utilities-lib)
is a library of common utilities is designed to streamline and
enhance development processes by providing a comprehensive toolkit
for HTTP requests, data access, and general utility functions.

Tailored for developers working on complex applications, the library
includes robust methods for performing secure and efficient HTTP
communications, simplified interfaces for database interactions,
and a wide range of general-purpose tools for tasks such as data
manipulation, format conversions, and error handling.

## License

Use of this source code is governed by an MIT-style
license that can be found in the LICENSE file or at
https://opensource.org/licenses/MIT.


