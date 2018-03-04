# Twig for WordPress

Twig for WordPress allow you to use the famous Twig template engine https://twig.symfony.com/ but extend it to be able to use the functions provided by WordPress such as `esc_html`, `esc_html__`, `wp_kses` etc...

This package register the escape function as filters and functions because translator functions need to be pass a textdomain.

## Modules

The package provide filters, functions and other stuffs into twig by define `modules`.

Modules are class instances that implements an interface `Unprefix\Twig\Module\Injectable` used by the Provider `Unprefix\Twig\Module\Provider` that allow us to retrieve all modules to be set into `twig` instance.

There is a filter `unprefix_twig_modules` within the `Provider::modules` method that allow third party softwares to hook into the modules list so, they'll be able to add other filters, functions, tags or whatever they want to use to extend the twig instance (read about how to extend twig here [https://twig.symfony.com/doc/2.x/advanced.html](https://twig.symfony.com/doc/2.x/advanced.html))

### Escapers

Unprefix Twig for WordPress define as filters and functions the followings:

- esc_html
- esc_html__
- esc_html_e
- esc_attr
- esc_attr__
- esc_attr_e
- esc_js
- esc_sql
- esc_textarea
- esc_url
- esc_url_raw

### Kses

The kses are defined only as functions.

- wp_kses
- wp_kses_post
- wp_kses_allowed_html

## Provider

The modules are retrieved by a Provider.

Within the provider the modules can be filtered `unprefix_twig_modules`.

So if you want to add a new module you can hook into this filter and return a new instance of `Unprefix\Twig\Module\Injectable`.

For example:

```
class MyModule implements Unprefix\Twig\Module\Injectable {

	public function injectInto(\Twig\Environment $twig): \Twig\Environment {
		// Do your stuffs here.

		return $twig;
	}

}

$provider = new Provider(new \Twig\Environmnet());

add_filter('unprefix_twig_modules', function($modules)
{
	$modules['module_name'] = new MyModule();

	return $modules;
});

$modules = $provider->modules();
```

This is just an example because you'll never need to create an instance of the Module `Provider`. Everything is handled by the `Factory` class.

You just need to add your filter and everything is ok.

## Factory

The package provide a `Factory` class that help you on creating a new instance of the `\Twig\Environment` class.

If you want to create a new Twig instance you can simply create a factory instance by passing a `\Twig\Loader\LoaderInterface` object and the twig options if you want to customize the environment.

```
$twigFactory = new \Unprefix\Twig\Factory(
	new \Twig\Loader\FilesystemLoader(),
	[
		'debug' => false,
    	'charset' => 'UTF-8',
    	'base_template_class' => 'Twig_Template',
    	'strict_variables' => false,
    	'autoescape' => 'html',
    	'cache' => false,
    	'auto_reload' => null,
    	'optimizations' => -1,
	]
);

$twig = $twigFactory->create();
```

Pretty easy, right?


## License
This programm is free software and is licensed using GPL2.

For more info about the license see [https://www.gnu.org/licenses/old-licenses/gpl-2.0.en.html](https://www.gnu.org/licenses/old-licenses/gpl-2.0.en.html)

## Bugs Reporting

To report bugs please refer to [https://github.com/widoz/unprefix-twig-wordpress/issues](https://github.com/widoz/unprefix-twig-wordpress/issues)

## Support

For support just open a new issue [https://github.com/widoz/unprefix-twig-wordpress/issues](https://github.com/widoz/unprefix-twig-wordpress/issues) and apply the label `help wanted`.