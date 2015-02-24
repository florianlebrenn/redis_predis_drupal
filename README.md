# redis_predis_drupal
Configuration file to enable cache with redis module and predis

# Installation
* http://redis.io/download#installation
* https://www.drupal.org/project/redis
* https://github.com/nrk/predis

# settings.php file

```php
  /**
   * Redis conf with predis backend.
   */
  
  /**
   * Add predis library (version 0.8.7) to use redis.
   * Don't use version 1.* (file tree has changed)
   */
  define('PREDIS_BASE_PATH', DRUPAL_ROOT . '/sites/all/libraries/vendor/predis/predis/lib/');
  
  // Checking if Redis is running.
  if (file_exists('/var/run/redis_6379.pid')) {
    $redis_up = TRUE;
  
    // Checking if entitycache module exists.
    if (file_exists('sites/all/modules/contrib/entitycache/entitycache.info')) {
      $entity_cache = TRUE;
    }
  }
  
  if (!empty($redis_up)) {
    // Required configurations.
    $conf['lock_inc'] = 'sites/all/modules/contrib/redis/redis.lock.inc';
    $conf['path_inc'] = 'sites/all/modules/contrib/redis/redis.path.inc';
    $conf['cache_backends'][]       = 'sites/all/modules/contrib/redis/redis.autoload.inc';
    $conf['redis_client_interface'] = 'Predis';
    $conf['redis_client_base'] = 1;
    $conf['redis_client_host'] = '127.0.0.1';
    $conf['redis_client_port'] = '6379';
    // Uncomment this line if Redis is locally running via socket.
    // $conf['redis_cache_socket'] = '/var/run/redis/redis.sock';
    // $conf['cache_prefix'] = 'mysite_';
  
    // Optional not redis specific.
    // $conf['redis_client_password'] = 'isfoobared';
    // $conf['cache_lifetime'] = 0;
    // $conf['page_cache_max_age'] = 0;
    // $conf['page_cache_maximum_age'] = 0;
    // $conf['page_cache_invoke_hooks'] = FALSE;
    // $conf['page_cache_without_database'] = TRUE;
  
    // Cache bins.
    $conf['cache_default_class'] = 'Redis_Cache';
    $conf['cache_class_cache_bootstrap'] = 'Redis_Cache';
    $conf['cache_class_cache'] = 'Redis_Cache';
    $conf['cache_class_cache_menu'] = 'Redis_Cache';
    $conf['cache_class_cache_block'] = 'Redis_Cache';
    $conf['cache_class_cache_views'] = 'Redis_Cache';
    $conf['cache_class_cache_views_data'] = 'Redis_Cache';
    $conf['cache_field'] = 'Redis_Cache';
    $conf['cache_class_cache_field'] = 'Redis_Cache';
    $conf['cache_class_cache_image'] = 'Redis_Cache';
    $conf['cache_class_cache_libraries'] = 'Redis_Cache';
    $conf['cache_class_cache_metatag'] = 'Redis_Cache';
    $conf['cache_class_cache_search_api_solr'] = 'Redis_Cache';
  
    // Always Database Cache.
    $conf['cache_class_cache_form'] = 'DrupalDatabaseCache';
  
    // Entity Cache.
    if (!empty($entity_cache)) {
      $conf['cache_entity_node'] = 'Redis_Cache';
      $conf['cache_entity_fieldable_panels_pane'] = 'Redis_Cache';
      $conf['cache_entity_file'] = 'Redis_Cache';
      $conf['cache_entity_taxonomy_term'] = 'Redis_Cache';
      $conf['cache_entity_taxonomy_vocabulary'] = 'Redis_Cache';
    }
  
  }
```
