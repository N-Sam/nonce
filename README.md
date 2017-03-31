[![Latest Stable Version](https://poser.pugx.org/wberredo/nonce/v/stable)](https://packagist.org/packages/wberredo/nonce)
[![Latest Unstable Version](https://poser.pugx.org/wberredo/nonce/v/unstable)](https://packagist.org/packages/wberredo/nonce)
[![License](https://poser.pugx.org/wberredo/nonce/license)](https://packagist.org/packages/wberredo/nonce)

# nonce
Use wordpress nonce functions in a object oriented environment.

## Installation

Add this package as requirement at your composer.json file and
then run 'composer update'
```json
"wberredo/nonce": "1.0.*"
```

Or directly run
```bash
composer require wberredo/nonce
```

## Setup

If you want to change some configs before you start to generate
nonces, you will use *Nonce_Config* class.
```php
// set lifetime for 4 hours
Nonce_Config::set_nonce_lifetime( 4 * HOUR_IN_SECONDS );

// set message showed when showAys is called
Nonce_Config::set_error_message( "Are you sure" );
```

## Usage
To create a nonce you have to use the *Nonce_Generator* class and
to verify a nonce already created you will need the *Nonce_Verifier*
class.

### Nonce_Generator
To generate a nonce
```php
$nonce_gen = new Nonce_Generator( "default-action" );
$nonce = $nonce_gen->generate_nonce();
```

To generate a URL nonce
```php
// you can also set parameters with set functions
$nonce_gen = new Nonce_Generator();
$complete_url = $nonce_gen
                    ->set_url( "http://github.com/WBerredo" )
                    ->set_action( "default_action" )
                    ->generate_nonce_url();
```

To retrieve a nonce field.
```php
$nonce_gen = new Nonce_Generator();
$nonceField = $nonce_gen
                    ->set_action( "default_action" )
                    ->generate_nonce_field( "nonce", "referer", "do_not_echo" );
                    
// to print the nonce field you have to set the last param as true
$nonce_gen
    ->generate_nonce_field( "nonce", "referer", "echo" );
```

To  Display 'Are you sure you want to do this?' message
(or the new message set with Nonce_Config#setErrorMessage)
to confirm the action being taken.
```php
Nonce_Generator::show_ays( 'action' );
```
### Nonce_Verifier
To verify a nonce
```php
if( Nonce_Verifier::verify( $nonce, $defaultAction ) ) {
// if is valid
} else {
// if is not valid
}
```

To verify a URL nonce
```php
if( Nonce_Verifier::verify_url( $complete_url, $defaultAction ) ) { 
// if is valid
} else {
// if is not valid
}
```

To tests either if the current request carries a valid nonce,
or if the current request was referred from an administration screen
```php
if( Nonce_Verifier::verify_admin_referer( $defaultAction ) ) {
// if is valid
} else {
// if is not valid
}
```

To verify the AJAX request, to prevent any processing of
requests which are passed in by third-party sites or systems.
```php
if( Nonce_Verifier::verify_ajax_referer( $defaultAction ) ) {
// if is valid
} else {
// if is not valid
}
```

## Contributing

1. Fork it!
2. Create your feature branch: `git checkout -b my-new-feature`
3. Commit your changes: `git commit -am 'Add some feature'`
4. Push to the branch: `git push origin my-new-feature`
5. Submit a pull request :D

## Tests

1. **Install PHPUnit.** WordPress uses PHPUnit, the standard for unit 
testing PHP projects. Installation instructions can be found in
[the PHPUnit manual](https://phpunit.de/manual/current/en/installation.html) 
or on the [PHPUnit Github repository](https://github.com/sebastianbergmann/phpunit#readme).

2. **Check out the test repository.** The WordPress tests live in 
the core development repository, 
at https://develop.svn.wordpress.org/trunk/:
  ```bash
  svn co https://develop.svn.wordpress.org/trunk/ wordpress-develop
  cd wordpress-develop
  ```

3. **Create an empty MySQL database.** The test suite will delete all 
data from all tables for whichever MySQL database it is configured.
Use a separate database.

4. **Set up a config file.** Copy wp-tests-config-sample.php 
to wp-tests-config.php, and enter your database credentials.
Use a separate database.

5. **Change the path of Wordpress project** in the bootstrap.php file of the plugin
  ```php
  /**
  * The path to the WordPress tests checkout.
  */
  define( 'WP_TESTS_DIR', '/home/berredo/Documents/repository/wordpress/wordpress-develop/tests/phpunit/' );
  ```

6. **Go to plugin's folder**
 
  ```bash
  cd vendor/wberredo/nonce
  ```
7. **Run phpunit** to test
  
  ```bash
  phpunit 
  ```

## Thanks to
* [Wordpress Nonces Documentation](https://codex.wordpress.org/WordPress_Nonces)
* [Wordpress Automated Testing Documentation](https://make.wordpress.org/core/handbook/testing/automated-testing/)

## License

[MIT](http://opensource.org/licenses/MIT)
