<div align="center">
  <img src="images/logo/inventree.png" alt="InvenTree logo" width="200" height="auto" />
  <h1>InvenTree</h1>
  <p>Inventory Management System </p>

<h4>
    <a href="https://demo.inventree.org/">View Demo</a>
  <span> · </span>
    <a href="https://docs.inventree.org/en/latest/">Documentation</a>
  <span> · </span>
  </h4>
</div>

<!-- About the Project -->
## :star2: About the Project

InvenTree is an open-source Inventory Management System which provides powerful low-level stock control and part tracking. The core of the InvenTree system is a Python/Django database backend which provides an admin interface (web-based) and a REST API for interaction with external interfaces and applications. A powerful plugin system provides support for custom applications and extensions.

Check out [our website](https://inventree.org) for more details.

<!-- Roadmap -->
### :compass: Roadmap

Want to see what we are working on? Check out the [roadmap tag](https://github.com/inventree/InvenTree/issues?q=is%3Aopen+is%3Aissue+label%3Aroadmap) and [horizon milestone](https://github.com/inventree/InvenTree/milestone/42).

<!-- Integration -->
### :hammer_and_wrench: Integration

InvenTree is designed to be **extensible**, and provides multiple options for **integration** with external applications or addition of custom plugins:

* [InvenTree API](https://docs.inventree.org/en/latest/api/api/)
* [Python module](https://docs.inventree.org/en/latest/api/python/python/)
* [Plugin interface](https://docs.inventree.org/en/latest/extend/plugins)
* [Third party tools](https://docs.inventree.org/en/latest/extend/integrate)

<!-- TechStack -->
### :space_invader: Tech Stack

<details>
  <summary>Server</summary>
  <ul>
    <li><a href="https://www.python.org/">Python</a></li>
    <li><a href="https://www.djangoproject.com/">Django</a></li>
    <li><a href="https://www.django-rest-framework.org/">DRF</a></li>
    <li><a href="https://django-q.readthedocs.io/">Django Q</a></li>
    <li><a href="https://django-allauth.readthedocs.io/">Django-Allauth</a></li>
  </ul>
</details>

<details>
<summary>Database</summary>
  <ul>
    <li><a href="https://www.postgresql.org/">PostgreSQL</a></li>
    <li><a href="https://www.mysql.com/">MySQL</a></li>
    <li><a href="https://www.sqlite.org/">SQLite</a></li>
    <li><a href="https://redis.io/">Redis</a></li>
  </ul>
</details>

<details>
  <summary>Client</summary>
  <ul>
    <li><a href="https://getbootstrap.com/">Bootstrap</a></li>
    <li><a href="https://jquery.com/">jQuery</a></li>
    <li><a href="https://bootstrap-table.com/">Bootstrap-Table</a></li>
  </ul>
</details>

<details>
<summary>DevOps</summary>
  <ul>
    <li><a href="https://hub.docker.com/r/inventree/inventree">Docker</a></li>
    <li><a href="https://crowdin.com/project/inventree">Crowdin</a></li>
    <li><a href="https://coveralls.io/github/inventree/InvenTree">Coveralls</a></li>
    <li><a href="https://packager.io/gh/inventree/InvenTree">Packager.io</a></li>
  </ul>
</details>

<!-- Docker Development Installment -->
## :star2: Docker Development Installment

You can use docker to launch and manage a development server, in a similar fashion to managing a production server.

The InvenTree dockerfile uses a multi-stage build process to allow both production and development setups from the same image.

There are some key differences compared to the docker production setup:

<ul>
  <li>The docker image is built locally, rather than being downloaded from DockerHub </li>
  <li>The docker image points to the source code on your local machine (mounted as a 'volume' in the docker container)</li>
  <li>The django webserver is used, instead of running behind Gunicorn</li>
  <li>The server will automatically reload when code changes are detected</li>
</ul>

### Quickstart and Development Setup Guide

To get "up and running" with a development environment, run the following commands:

```bash
git clone https://github.com/inventree/InvenTree.git && cd InvenTree
docker compose run inventree-dev-server invoke update
docker compose run inventree-dev-server invoke setup-test --dev
docker compose up -d
```

To get started with an InvenTree development setup, follow the simple steps outlined below. Before continuing, ensure that you have completed the following steps:
<ul>
  <li>Downloaded the InvenTree source code to your local machine</li>
  <li>Installed docker on your local machine (install Docker Desktop on Windows </li>
  <li>Have a terminal open to the root directory of the InvenTree source code</li>
</ul>

#### Perform Initial Setup

Perform the initial database setup by running the following command:

```bash
docker compose run inventree-dev-server invoke update --no-frontend
```

If this is the first time you are configuring the development server, this command will build a development version of the inventree docker image.

This command also performs the following steps:

<ul> 
  <li>Ensure required python packages are installed</li>
  <li>Perform the required schema updates to create the required database tables</li>
  <li>Update translation files</li>
  <li>Collect all required static files into a directory where they can be served by nginx</li>
</ul>
Ensure required python packages are installed
Perform the required schema updates to create the required database tables
Update translation files
Collect all required static files into a directory where they can be served by nginx

#### Import Demo Data

To fill the database with a demo dataset, run the following command:

```bash
docker compose run inventree-dev-server invoke setup-test --dev
```

#### Start Docker Containers

Now that the database has been created, and migrations applied, we are ready to launch the InvenTree containers:

```bash
docker compose up -d
```

#### Create Admin Account

If you are creating the initial database, you need to create an admin (superuser) account for the database. Run the command below, and follow the prompts:

```bash
docker compose run inventree-dev-server invoke superuser
```

### Running commands in the container

Using docker compose run [...] commands creates a new container to run this specific command. This will eventually clutter your docker with many dead containers that take up space on the system.

You can access the running containers directly with the following:

```bash
docker exec -it inventree-dev-server /bin/bash
```

You then run the following to access the virtualenv:

```bash
source data/env/bin/activate
```

#### Cleaning up old containers

If you have Docker Desktop installed, you will be able to remove containers directly in the GUI. Your active containers are grouped under "inventree" in Docker Desktop. The main dev-server, dev-db, and dev-worker containers are all listed without the "inventree" prefix. One time run containers, like those executed via docker compose run [...] are suffixed with run-1a2b3c4d5e6f where the hex string varies.

To remove such containers, either click the garbage bin on the end of the line, or mark the containers, and click the delete button that shows up. This is the recommended procedure for container cleanup.

### Restarting Services

Once initial setup is complete, stopping and restarting the services is much simpler:

**Stop InvenTree Services**

To stop the InvenTree development server, simply run the following command:

```bash
docker compose down
```

**Start InvenTree Services**

To start the InvenTree development server, simply run the following command:

```bash
docker compose up -d
```

**Restart InvenTree Services**

A restart cycle is as simple as:

```bash
docker compose restart
```

### Editing InvenTree Source

Any changes made to the InvenTree source code are automatically detected by the services running under docker.

Thus, you can freely edit the InvenTree source files in your editor of choice.

**Database Updates**
Any updates which require a database schema change must be reflected in the database itself.

To run database migrations inside the docker container, run the following command:

```bash
docker compose run inventree-dev-server invoke update --no-frontend
```

**Docker Image Updates**
Occasionally, the docker image itself may receive some updates. In these cases, it may be required that the image is rebuilt. To perform a complete rebuild of the InvenTree development image from local source, run the following command:

```bash
docker compose build --no-cache
```

<!-- Login Details -->
## Login Details

Multiple default accounts are provided, as detailed below. Each account is afforded a different set of permissions, so users can see the InvenTree roles/permission system in action

| Username	    | Password      | Descriptions    |
| ------------- | ------------- | -------- |
| allaccess     | nolimits      | View / create / edit all pages and items  |
| reader        | readonly      |Can view all pages but cannot create, edit or delete database records  |
| engineer      | partsonly     | Can manage parts, view stock, but no access to purchase orders or sales orders |
| admin         | inventree     | Can manage parts, view stock, but no access to purchase orders or sales orders  |

<!-- Feature Analysis -->
## :star2: Feature Analysis
***Parts***

The Part is the core element of the InvenTree ecosystem. A Part object is the archetype of any stock item in the inventory. Each Part defined in the database provides a number of different attributes which determine how that part can be used. The main part view is divided into 4 different panels, those are categories, details, tabs, and content of each tab.

***Stock***

In the stock session, we can monitor the stock item, stock location, and stock status. 
1. Stock item is an actual instance of a Part item. It represents a physical quantity of the Part in a specific location
2. Stock location represents a physical real-world location where Stock Items are stored
3. Stock status is used to see current condition of the individual stock item whether its 'ok', 'need attention', 'destroyed', 'lost', and etc.

***Build Orders***

A Build Order from section _build_ is used to create new stock by assembling component parts, according to a Bill of Materials (BOM). A BOM can be specified for any Part which is designated as an Assembly. The BOM consists of other Parts which are designated as Components.

A Build Order uses the BOM to allocate stock items to the assembly process. As the Build Order is completed, the required stock quantities are subtracted from allocated stock items.

***Orders***

Orders are divided as purchase orders, sales orders, and return orders. 

**Purchase Orders**
Purchase orders allow to track which parts are bought from suppliers and manufacturers, therefore converting externally bought items into stock items / inventory.

**Sales Orders**
Sales orders allow tracking of which stock items are sold to customers, therefore converting stock items or inventory into externally sold items. 

**Return Orders**
Return Orders allow stock items (which have been sold or allocated to a customer) to be to be returned into stock, typically for the purpose of repair or refund.

*Each of Purchase Orders, Sales Orders, and Return Orders has a specific status code, which represents the state of the order, there are _Pending_, _In Progress_, _Shipped_, _Cancelled_.*

<!-- Acknowledgments -->
## :gem: Acknowledgements

We would like to acknowledge a few special projects:
 - [PartKeepr](https://github.com/partkeepr/PartKeepr) as a valuable predecessor and inspiration
 - [Readme Template](https://github.com/Louis3797/awesome-readme-template) for the template of this page

Find a full list of used third-party libraries in [our documentation](https://docs.inventree.org/en/latest/credits/).

<p>This project is supported by:</p>
<p>
  <a href="https://inventree.org/digitalocean">
    <img src="https://opensource.nyc3.cdn.digitaloceanspaces.com/attribution/assets/SVG/DO_Logo_horizontal_blue.svg" width="201px">
  </a>
  <a href="https://www.netlify.com"> <img src="https://www.netlify.com/v3/img/components/netlify-color-bg.svg" alt="Deploys by Netlify" /> </a>
</p>

<!-- License -->
## :warning: License

Distributed under the [MIT](https://choosealicense.com/licenses/mit/) License. See [LICENSE.txt](https://github.com/inventree/InvenTree/blob/master/LICENSE) for more information.
