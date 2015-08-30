# Installation instructions

## Paths for future reference
  * `/<root-dir>/`: The filesystem path where eZ Platform is installed in.
    Examples: `/home/myuser/www/` or `/var/sites/<project-name>/`

## Prerequisite

  These instructions assume you have strong technical knowledge and have already installed PHP, web server & *a database server* needed for this software.
  For further information on requirements see: https://doc.ez.no/display/EZP/Requirements
  *Note: set php.ini memory_limit=256M before running ezplatform::install command below

## Install

1. **Create an empty database**, and optionally setup Solr

    The following step will ask you for credentials/details for which database to use, so make sure to create one first.

    *Optional: At this point you can also setup Solr to be used by eZ Platform*

    *Note: Right now installer only supports MySQL, Postgres support should be (re)added in one of the upcoming releases.*

2. **Get eZ Platform**:

    There are two ways to install eZ Platform described below, what is common is that you should make sure
    relevant settings are generated into `ezpublish/config/parameters.yml` as a result of this step.

    `parameters.yml` contains settings for your database, mail system, and optionally [Solr](http://lucene.apache.org/solr/)
    if `search_engine` is configured as `solr`, as opposed to default `legacy` *(a limited database powered search engine)*.

    A. **Archive** (tar/zip) *from http://share.ez.no/downloads/downloads*

       Extract the eZ Platform 15.01 (or higher) archive to a directory, then execute post install scripts:

       *Note: The post install scripts will ask you to fill in some settings, including database settings.*

       ```bash
       cd /<directory>/
       curl -s http://getcomposer.org/installer | php
       php -d memory_limit=-1 composer.phar run-script post-install-cmd
       ```


    B. **Composer**

     You can get eZ Platform using composer with the following commands:

     *Note: composer will take its time to download all libraries and when done you will be asked to fill in some settings, including database settings.*

       ```bash
       curl -s http://getcomposer.org/installer | php
       php -d memory_limit=-1 composer.phar create-project --no-dev ezsystems/ezplatform <directory> [<version>]
       cd /<directory>/
       ```

     Options:
       - `<version>`: Optional, *if omitted you'll get latest*, examples for specifying:
        - `dev-master` to get current development version (pre release) `master` branch
        - `v0.9.0` to pick a specific release/tag
        - `~0.9.0` to pick latests v0.9.x release
       - For core development: Add '--prefer-source' to get full git clones, and remove '--no-dev' to get things like phpunit and behat installed.
       - Further reading: https://getcomposer.org/doc/03-cli.md#create-project

3. *Only for *NIX/OS X users* **Setup folder rights**:

       *Skip to step #4 if you plan to install LegacyBridge and eZ Publish Legacy with this installation.*

       One common issue is that the `ezpublish/{cache,logs,sessions},` directories **must be writable both by the web server and the command line user**.
       To make sure both have access, you can run the following commands in your `<root-dir>` to ensure that permissions will be set up properly.

       In the example below, replace `www-data` with your web server user.

       ```bash
       $ chown -R www-data ezpublish/{cache,logs,sessions}
       $ chmod -R ug+rwX ezpublish/{cache,logs,sessions}
       ```

       Note: *User group is on purpose not specified as it is assumed this is set to current command line users group that will continue to need access.
       To make this work flawlessly you'll need to add web server user group as primary group to your command line user, to make sure it has access to files created by web user and vica-versa.*

       Note2: *On Linux it is also possible to use ACL to setup more advance rights if you are more comfortable using that.*

4. *Optional* **Install LegacyBridge and eZ Publish Legacy**:

    eZ Platform, unlike eZ Publish 5.x, does not come with eZ Publish Legacy (the updated version of eZ Publish 4.x).
    For the time being it is still possible to run Legacy side by side with eZ Platform, further instructions here:
    https://doc.ez.no/display/EZP/Installing+eZ+Publish+Legacy+on+top+of+eZ+Platform

5. **Configure a VirtualHost**:

    A virtual host setup is the recommended, most secure setup of eZ Publish.
    General virtual host setup template for Apache and Nginx can be found in [doc/ folder](doc/).


6. **Run installation command**:

    You may now complete the eZ Platform installation with ezplatform:install command, example of use:

    ```bash
    $ php -d memory_limit=-1 ezpublish/console ezplatform:install --env prod demo
    ```

    **Note**: Password for the generated `admin` user is `publish`, this name and password is needed when you would like to login to backend Platform UI. Future versions will prompt you for a unique password during installation.

You can now point your browser to the installation and browse the site. To access the Platform UI backend, use the `/ez` URL.
