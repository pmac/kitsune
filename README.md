# Kitsune

Kitsune is the platform that powers [SuMo (support.mozilla.org)](https://support.mozilla.org)

It is a [Django](http://www.djangoproject.com/) application. There is
[documentation](https://kitsune.readthedocs.io/) online.

You can access the staging site at <https://support.allizom.org/>

See [what's deployed](https://whatsdeployed.io/s-J18)

## Development

To get Kitsune running locally all you really need is to have [Docker installed](https://www.docker.com/products/docker-desktop),
and follow the following steps.

1. Fork this repository & clone it to your local machine.
   ```
   git clone https://github.com/mozilla/kitsune.git
   ```

2. Download base Kitsune docker images:
   ```
   make pull
   ```

3. Create your database and install node and bower packages
   ```
   make init
   ```

4. (Optional) Enable the admin control panel
   ```
   echo "ENABLE_ADMIN=True" >> .env
   ```

5. Run Kitsune
   ```
   make run
   ```

The running instance will be located at http://0.0.0.0:8000/ unless you specified otherwise, and the administrative control panel will be at http://0.0.0.0:8000/admin.

After that you can do some additional optional steps if you want to for the admin:

* Create a superuser
  ```
  docker-compose exec web ./manage.py createsuperuser
  ```

* Create some data
  ```
  docker-compose exec web ./manage.py generatedata
  ```

* Update product details
  ```
  docker-compose exec web ./manage.py update_product_details
  ```

* Get search working

   First, make sure you have run the "Create some data" step above.

   1. Enter the web container: `docker-compose exec web bash`
   2. Build the indicies: `./manage.py esreindex` (You may need to pass the `--delete` flag)
   3. Precompile the nunjucks templates: `./manage.py nunjucks_precompile`
