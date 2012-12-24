# HTML Navigation Module for Kohana 3.3

Simplify rendering, building and maintenance of simple, dynamic navigation menus. We don't want _this_ in our HTML.

```php
<? if ($user->get_role() === Role::ANONYMOUS):?>
<ul>
	<li><a href="/" <?= $page === 'home' ? 'class="active"' : NULL?>>Home</a></li>
	<li><a href="/about" <?= $page === 'about' ? 'class="active"' : NULL?>>About</a></li>
</ul>
<? // elseif (...)?>
```

**Some aspects of the module might not work since they are untested.**

## Basics

You define your menus in Kohana configuration files (see [config/menu/bootstrap.php](https://github.com/anroots/kohana-menu/blob/master/config/menu/bootstrap.php)). For example, a WordPress type blog might have...

Then, in your (main) controller (or view), you determine which menu configuration to use (based on user role or other factors),
use the factory method to construct a new Menu object, set the active link and render it in your view. Done.

* Public main navigation menu
* Public footer menu
* Admin-only menu on the public pages, when admin is logged in
* Admin-only menu on the administrator interface

Normally, you'd build HTML views with `ul` and `li` elements and then write some PHP to highlight the active link. This is
difficult to maintain (DRY) and too much hassle (not to mention ugly).

### Example, rendering the default menu

```php
<?php
// APPPATH/views/template.php

echo Menu::factory('bootstrap')->render();
// echo Menu::factory()
```

## Installation

### Place the files in your modules directory.

#### As a Git submodule:

```bash
git clone git://github.com/anroots/kohana-menu.git modules/menu
```
#### As a [Composer dependency](http://getcomposer.org)

```javascript
{
	"require": {
		"php": ">=5.4.0",
		"composer/installers": "*",
		"anroots/menu":"2.*"
	}
}
```

### Create a folder `menu` in your applications config directory, copy the `menu/bootstrap.php` into it,
and adjust it to fit your navigation.

```bash
mkdir -p application/config/menu
cp modules/menu/config/menu/bootstrap.php application/config/menu/bootstrap.php
# edit application/config/menu/bootstrap.php
```

### Activate the module in the `bootstrap.php` file.

```php
<?php
Kohana::modules(array(
	...
	'menu' => MODPATH.'menu',
));
```

### Echo the output in your template

```html
<div class="navbar navbar-fixed-top">
	<div class="navbar-inner">
		<div class="container">
			<?=(string)Menu::factory()?>
		</div>
	</div>
</div>
```

You might wish to instantiate the menu in your main controller, since this gives you a way to interact with the Menu object
before it's rendered.

## Config files

You can use different config files by setting the factory's `$config` parameter.

### Example: Load menu configuration based on user role

```php
	$menu = Menu::factory($role); // this could use `config/menu/(user|admin).php`
```

## Marking the current menu item

The config setting `current_class` defines the css class, which will be used by the `set_current()` method, to mark the current menu item:
```php
	$menu->set_current('article/show');
```
The parameter of `set_current()` is the URL value of the respective item or its ID.

# Licence

The Kohana module started out as a fork of the original Kohana Menu module by
[Bastian Bräu](http://github.com/b263/kohana-menu), but is now independently developed.

The MIT License (MIT)
Copyright (c) 2012 Ando Roots

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.