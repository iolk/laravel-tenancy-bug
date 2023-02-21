## To reproduce

Initialize the repo with

```
cp .env.example .env

docker run --rm \
    -u "$(id -u):$(id -g)" \
    -v "$(pwd):/var/www/html" \
    -w /var/www/html \
    laravelsail/php82-composer:latest \
    composer install --ignore-platform-reqs
```

Then run

```
vendor/bin/sail up -d
vendor/bin/sail artisan key:generate
vendor/bin/sail artisan migrate
vendor/bin/sail artisan tinker
```

With tinker create the following tenants

```
$tenant1 = App\Models\Tenant::create(['id' => 'foo', 'mail_from_name'=>'Foo Tenant', 'mail_from_address'=>'foo@localhost.com']);
$tenant1->domains()->create(['domain' => 'foo.localhost']);

$tenant2 = App\Models\Tenant::create(['id' => 'bar', 'mail_from_name'=>'Bar Tenant', 'mail_from_address'=>'bar@localhost.com']);
$tenant2->domains()->create(['domain' => 'bar.localhost']);
```

Run the `queue:work` command:

```
vendor/bin/sail artisan queue:work --sleep=3
```

Visit `foo.localhost` and `bar.localhost`
