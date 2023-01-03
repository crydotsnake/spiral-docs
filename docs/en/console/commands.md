# Creating Console Commands

## Introduction

Creating commands in the Spiral Framework is a straightforward process that allows you to extend the functionality of your application and provide additional functionality to your users. 
In this documentation, we will walk you through the process of creating a new command from start to finish.

## Prerequisites

Before you can create a new command you will need to have a working Spiral-based application.

## Create the Command

To create a new command, you can use the following template as a starting point:

```php
use Spiral\Console\Command;

class SomeCommand extends Command 
{
    protected const SIGNATURE = 'check:http {url : Site url} {--S|skip-ssl-errors : Skip SSL errors}';

    public function perform(): int
    {
        $url = $this->argument('url');
        $skipErrors = $this->option('skip-ssl-errors');

        return self::SUCCESS;
    }
}
```

In this example, the `SIGNATURE` constant defines two arguments: a required argument named `url`, with the description `"Site url"`, and an optional boolean option named `skip-ssl-errors`, with the short version `S` and the description `"Skip SSL errors"`.

The `perform` method is where the main logic of the command is implemented. 
This method should contain the code that is needed to complete the task of the command.

You may also make arguments optional or define default values for arguments:

```php
// Optional argument...
'check:http {url?}'
 
// Optional argument with default value...
'check:http {url=foo}'
```

`Options`, same as arguments, are another form of user input. Options are prefixed by two hyphens `--`.

```php
'{--S|skip-ssl-errors : Skip SSL errors}'
```

If the user must specify a value for an `option`, you should suffix the option name with a `=` sign.

```php
'some:command {argument} {--option=default}'
```

If you would like to define `arguments` or `options` to expect multiple input values, you may use the `*` character.

```php
'check:http {url*}'
```

## Invoke the Command

To invoke the command, you can use the command line to send a request to the console component. 
The command line syntax will depend on the name and arguments of your command.

For example, to invoke the `MyCommand` command, you can use the following command:

```bash
php app.php check:http google.com --S
```

## Arguments and Options

Instead of `SIGNATURE` constant you also can use `ARGUMENTS` with `OPTIONS` in your command class.

> **Note**
> This constants will be deprecated since v4.0

```php
use Spiral\Console\Command;

class SomeCommand extends Command 
{
    const NAME = 'parse:category';
    const DESCRIPTION = 'This is my command';

    const ARGUMENTS = [
        ['name', InputArgument::REQUIRED, 'Category name.']
    ];
        
    const OPTIONS = [
        ['silent', 's', InputOption::VALUE_NONE, 'Silent parse.']
    ];

    public function perform(): int
    {
        $name = $this->argument('name');
        $silent = $this->option('silent');

        return self::SUCCESS;
    }
}
```

In this example, the `ARGUMENTS` constant defines a single required argument named `name`, with the description `"Category name."`.
The `OPTIONS` constant defines a single option named `silent`, which is a boolean option (no value is required). 
The `s` value is the short version of the option.

## Helper Methods

You can use a set of helper methods available inside the `Spiral\Console\Command`. Given examples are intended to be
called in the `perform` method.

To write into output:

```php
$this->writeln('hello world');
```

To write into output without advancing to a new line:

```php
$this->write('hello world');
```

To write formatted output without advancing to a new line:

```php
$this->sprintf('Hello, <comment>%s</comment>', $name);
```

> **Note**
> This method is compatible with the `sprintf` definition.

To check if the current verbosity mode is higher than `OutputInterface::VERBOSITY_VERBOSE`:

```php
dump($this->isVerbose());
```

> **Note**
> You can freely use the `dump` method in console commands.

To render a table:

```php
$table = $this->table([
    'Column #1:',
    'Column #2:',
]);

foreach ($data as $row)
{
    $table->addRow([
        $row[1],
        $row[2]
    ]);
}

$table->render();
```

To add a table separator:

```php
$table->addRow(new Symfony\Component\Console\Helper\TableSeparator());
```

To determine if the input option is present:

```php
$this->hasOption(...);
```

To determine if the input argument is present:

```php
$this->hasArgument(...);
```

To asks for confirmation:

```php
$status = $this->confirm('Are you sure?', default: false);
```

To asks a question:

```php
$status = $this->ask('Are you sure?', default: 'no');
```

To prompt the user for input but hide the answer from the console:

```php
$status = $this->secret('User password');
```

To write a message as information output:

```php
$this->info('Some message');
```

To write a message as comment output:

```php
$this->comment('Some message');
```

To write a message as question output:

```php
$this->question('Some question');
```

To write a message as error output:

```php
$this->error('Some error');
```

To write a message as warning output:

```php
$this->warning('Some warning');
```

To write a message as alert output:

```php
$this->alert('Some alert');
```

To write a blank line:

```php
$this->newLine();
$this->newLine(count: 5);
```

## ApplicationInProduction

`ApplicationInProduction` is a class that makes it easy to ask the user for confirmation for actions in command if 
the application is running in production mode.

```php
use Spiral\Console\Confirmation\ApplicationInProduction;

final class MigrateCommand extends Command
{
    protected const NAME = 'db:migrate';
    protected const DESCRIPTION = '...';

    public function perform(ApplicationInProduction $confirmation): int
    {
        if (!$confirmation->confirmToProceed()) {
            return self::FAILURE;
        }
        
        // run migrations...
    }
}
```

## Events

| Event                                | Description                                                     |
|--------------------------------------|-----------------------------------------------------------------|
| Spiral\Console\Event\CommandStarting | The Event will be fired `before` executing the console command. |
| Spiral\Console\Event\CommandFinished | The Event will be fired `after` executing the console command.  |
