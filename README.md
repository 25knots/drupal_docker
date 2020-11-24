# Drupal 8/9 module development repo

This is the development repository for any module project.

It will help you install a clean site on your machine via Docker and can be used in CI as well. There is also a Behat and Selenium container.

## Installation

1. Clone this repository to a desired location.
2. Make sure composer dependencies or installed in the site dir, recommended:
https://github.com/25knots/drupal_docker_site
3. `docker-compose up -d`
4. Install the site with: `docker-compose exec web bash /var/www/scripts/install-site.sh`
5. You should be able to access the site on the IP address of your docker host and port 32770. E.g. `http://localhost:32770` Admin login is `admin/admin`.

## Enable Xdebug

1. Change the APP_ENVIRONMENT arg in the docker-compose.yml file for the web container to dev.
2. Rebuild the web container: `docker-compose build web`.
3. After running `docker-compose up -d` again Xdebug should be available.
