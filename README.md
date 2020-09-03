# PHP Database Migration

Laravel's Eloquent is one of the most fluent and expressive ORM's available for PHP. However, unlike some of its competitors like Doctrine or Propel, it's highly tied to the Laravel ecosystem in some ways. It's still possible to use Eloquent outside Laravel with a bit of work. In this example, we are using Slim Framework, but most of the code will work in any framework of your choice, even in plain PHP if you are into that.

If you use Eloquent outside Laravel, you will be missing one of it's best features: database migrations. Using migrations you can store your database structure in code. This will make make updating production servers and working with co-workers a breeze. There's no need to open up MySQL command line or client, just run a few commands and your database it up to date.

There has been some effort in order to use Laravel migrations outside the framework, but it hasn't been possible because the migrations require a complete instance of Laravel to run. In this tutorial, we are using framework agnostic Phinx migration library instead.


## Usage

- Define your database configuration

```
define('DB_HOST', '127.0.0.1');
define('DB_NAME', 'myproject');
define('DB_USER', 'root');
define('DB_PASSWORD', 'root');
define('DB_PORT', 3306);
```

- Create new migration: 

```
php vendor/bin/phinx create MyFirstMigration -c config-phinx.php
```
You can find your first migration in the migrations directory, it's marked by a timestamp and migration name. Open it up.

By default, Phinx uses a change method. Because Eloquent doesn't support that, we need to use more traditional `up()` and `down()` methods. The up method describes the database changes we want to create and the down method reverses them. Here's an example of a migration. You can check all available properties in the [Laravel documentation](https://laravel.com/docs/7.x/migrations).

```
<?php

use \PhpMigration\Migration\Migration;

class MyFirstMigration extends Migration {
    public function up() {
        $this->schema->create('widgets', function(Illuminate\Database\Schema\Blueprint $table){
            // Auto-increment id 
            $table->increments('id');
            $table->integer('serial_number');
            $table->string('name');
            // Required for Eloquent's created_at and updated_at columns 
            $table->timestamps();
        });
    }
    public function down() {
        $this->schema->drop('widgets');
    }
}
```

- Running your first migration

```
php vendor/bin/phinx migrate -c config-phinx.php
```

## References

[phinx](https://book.cakephp.org/phinx/0/en/migrations.html)

## Contributing
Pull requests are welcome. For major changes, please open an issue first to discuss what you would like to change.

Please make sure to update tests as appropriate.

## License
[MIT](https://choosealicense.com/licenses/mit/)
