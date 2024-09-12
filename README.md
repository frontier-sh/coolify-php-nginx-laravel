# coolify-php-nginx-laravel
 
This is an example repository containing the full code I've used to [deploy PHP, nginx, and Laravel queues](https://frontier.sh/posts/20240510-phpfpm-nginx/) on [Coolify](https://coolify.io/).

## How does it work?

The `./nixpacks.toml` file sets up the following static files:

- our `start.sh` script, which starts up supervisord, which in turn starts all of its workers and keeps them up
- our `supervisord.conf` file that configures supervisord
- `worker-X.conf` files used to tell supervisord how to run PHP, nginx, and the default Laravel queue
- the `php-fpm.conf` file used for PHP-FPM's configuration
- our `nginx-template.conf` file used for nginx's configuration

You don't need to include these files yourself; they're written as nixpacks builds the image for you.

Those files get placed into the `./assets` directory on build, and the commands in the `[phases.build]` section move those files and use them appropriately.

You should be able to just drop this file into your repository, set Coolify's "Build pack" option to "Nixpacks" (the default), and configure the other settings to pull from your Github repo etc.

You can make any adjustments to the supervisord, PHP-FPM or nginx config as needed and redeploy for them to take effect. For example, you might want to reduce the number of Laravel queue workers, or adjust how many pools PHP-FPM uses.