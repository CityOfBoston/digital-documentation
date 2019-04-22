# D8 Dependency Injection \(DI\)

For an explanation on what DI is and why it's a good idea to use it, refer to the [Drupal docs](https://www.drupal.org/docs/8/api/services-and-dependency-injection/services-and-dependency-injection-in-drupal-8).

When you're using DI, you're asking the [Service Container](https://symfonycasts.com/screencast/drupal8-under-the-hood/what-is-the-service-container), which Drupal borrows from Symfony, to pass the correct object into your class so that you can interact with it in some way. See [drupal docs](https://api.drupal.org/api/drupal/services/) for a full list of services provided by Drupal core.

The main thing to keep in mind is that any time you are writing a class in Drupal 8, you should be accessing external services through the service container \(SC\). If you use `\Drupal::someMethod()` within a class, that is a red flag as it's an opportunity to use DI instead.

## Blocks, Controllers, Forms

Controllers \(extending `ControllerBase`\), blocks \(extending `BlockBase`\) and forms \(extending `FormBase`\) have special access to the SC. They can access it directly via their `create()`method. The example below gets access to the `database` and `request_stack` services, and then passes those into the `__construct()` method so that they can be used later on in the class. These services could also be accessed statically, `\Drupal::database()`, `\Drupal::service('database')`, `\Drupal::request()`, `\Drupal::service('request_stack')`, but since we're writing a class that's not the recommended approach.

```php
  protected $db;
  protected $requestStack;

  /**
   * {@inheritdoc}
   */
  public static function create(ContainerInterface $container) {
    return new static(
      $container->get('database'),
      $container->get('request_stack')
    );
  }

  /**
   * {@inheritdoc}
   */
  public function __construct(Connection $db, RequestStack $requestStack) {
    $this->db = $db;
    $this->requestStack = $requestStack;
  }
```

In the other methods of our class, we can now access our services like this: `$this->db->someMethod()` or `$this->requestStack->someMethod()`.

## Roll your own service

If you're writing a class with functionality that will be \(or could be\) used multiple other places in the application, consider making it a service. For instance, if you're interacting with a 3rd party API, you may want to write a class to make common interactions easier \(connect, fetch, etc\).

In this case, refer to [drupal docs](https://www.drupal.org/docs/8/api/services-and-dependency-injection/structure-of-a-service-file) for creating a service. The part of the `.services.yml` relevant to this article comes in the `arguments` line:

```text
arguments: ['@database', '@request_stack']
```

This instructs Drupal to reach into the SC and pass the `database` and `request_stack` services into your class. This replaces the `create()` method in the example above. You can then add your services to your constructor like this:

```php
  protected $db;
  protected $requestStack;

  /**
   * {@inheritdoc}
   */
  public function __construct(Connection $db, RequestStack $requestStack) {
    $this->db = $db;
    $this->requestStack = $requestStack;
  }
```

Now the other methods of your class can reference these services like this: `$this->db->someMethod()` or `$this->requestStack->someMethod()`.

