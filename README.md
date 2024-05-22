# Lunar Hill

Imagine waiting for more than 4 months for a Brick Hill clone that will never last

"Never trust Brick Hill clones." -Maxed1

"Don't make Brick Hill clones, it's a loser activity" -Bandito Burrito

## Prerequisites
 - [Docker](https://www.docker.com/get-started/)
 - Debian Linux or Debian based Linux distro

## Notes on Local Development
 - The renderer is an external service. All requests to the Renderer helper class will be ignored in local.

## Setup
1. Install the required dependencies:
    ```sh
    sudo apt install php composer php-redis php-mysql php-gd php-simplexml nodejs npm mariadb-server mariadb-client redis
    ```

2. Rename .env.example to .env
    ```sh
    cp .env.example .env
    ```

3. Add MySQL user
    ```sh
    sudo mysql
    ```

    ```sql
    CREATE DATABASE testbh;

    CREATE USER bob@localhost IDENTIFIED BY 'password1';
    GRANT ALL PRIVILEGES ON testbh.* TO bob@localhost IDENTIFIED BY 'password1';

    REVOKE DELETE, TRUNCATE, DROP ON testbh.* FROM bob@localhost;
    ```

3. Modify `APP_URL` and `SESSION_DOMAIN` to the domain you wish to use. Tests use `http://laravel-site.test`
4. Overwrite DNS to point your local computer to that domain. Linux is located at `/etc/hosts` macOS at `/private/etc/hosts` and Windows at `C:\Windows\System32\Drivers\etc\hosts`.

   ```
   127.0.0.1 laravel-site.test
   127.0.0.1 api.laravel-site.test
   127.0.0.1 admin.laravel-site.test
   ```

5. Run setup commands

   ```sh
    npm install --no-bin-links
    php artisan key:generate
    php artisan migrate:fresh --seed
    composer update
    npm run dev
   ```
6. Running OpenSearch
   ```
   docker run -d \
        --name opensearch \
        -p 9200:9200 \
        -p 9600:9600 \
        -e "discovery.type=single-node" \
        -e "plugins.security.disabled=true" \
        -v opensearchdata:/usr/share/opensearch/data \
        opensearchproject/opensearch:2
   ```

   Adding stuff to OpenSearch
   ```sh
   php artisan scout:import App\Models\User\User
   php artisan scout:import App\Models\Item\Item
   ```

7. Default username and password are `Test Name` `password`

Good luck getting the [renderer](https://github.com/lunar-hill/renderer) working.

forgot to say, running is done by `php artisan serve --port=80`

## Development Tools

Anyone not using VSCode is invalid.

Suggested extensions
 - [Laravel Artisan](https://marketplace.visualstudio.com/items?itemName=ryannaddy.laravel-artisan)
 - [Laravel Blade Formatter](https://marketplace.visualstudio.com/items?itemName=shufo.vscode-blade-formatter)
 - [Laravel Blade Snippets](https://marketplace.visualstudio.com/items?itemName=onecentlin.laravel-blade)
 - [PHP Intelephense](https://marketplace.visualstudio.com/items?itemName=bmewburn.vscode-intelephense-client)
 - [Volar](https://marketplace.visualstudio.com/items?itemName=johnsoncodehk.volar)
 - [Prettier](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode)

Code completion suggestions for Intelephense can be written using
```
php artisan ide-helper:model -M
php artisan ide-helper:generate
```

## Running Tests

Laravel tests are run with `php artisan test`

PHPStan tests are run with `./vendor/bin/phpstan analyse --memory-limit=2G`
