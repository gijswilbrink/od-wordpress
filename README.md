# od-wordpress
MVC Parent theme for Wordpress by Occhio Design

## Installation
Download and add to your *wp-content/themes/* folder. Then add three folders to your child theme:
- */od/includes*
- */od/classes*
- */od/classes/control*

And get ready for the magic

## Including files or adding procedural code
#### The wordpress way
```
require('dependent-file.php');
```
Or worse: you'd be lazy and add the code to your main *functions.php* file
#### The OD way
Just add a new file to your *od/includes* folder

## Creating a custom post type
#### The wordpress way
```
add_action( 'init', 'create_post_type' );
function create_post_type() {
  register_post_type( 'product',
    array(
      'labels' => array(
        'name' => __( 'Products' ),
        'singular_name' => __( 'Product' )
      ),
      'public' => true,
      'has_archive' => true,
    	'menu_icon' => 'dashicons-cart'
    )
  );
}
```
#### The OD way
```
// Save this to od/classes/class-product.php and the autoloader will find it whenever you call it
class Product extends PostType
{
	protected $label_name = 'Products';
	protected $args = array(
		'menu_icon' => 'dashicons-cart'
	);
}
```

## Getting primary data or meta data from a post
#### The wordpress way
```
$post_title = get_the_title(); // primary data, fairly simple
$price = get_post_meta( get_the_ID(), 'price', true ); // meta, quite a hassle
$color = get_post_meta( get_the_ID(), 'color', true ); // new meta, same hassle, new database call

```
#### The OD way
```
$oProduct = new Product(); // occhio's autoloader will automatically load Product if it's in od/classes/class-product.php
$post_title = $oProduct->Get('post_title'); // simple object oriented syntax
$price = $oProduct->Get('price'); // same syntax for meta data and primary data
$color = $oProduct->Get('color'); // no new db call, all meta fields have been loaded since first call
```

## Posting data to the server
#### The wordpress way
You create a form
```
<form action="./" method="get">
	<input type="hidden" name="action" value="save_product" />
	<input type="text" name="title" />
	<input type="text" name="price" />
</form>
```
And you add some code to your already cluttered *functions.php*
```
if($_GET['action'] == 'save_product') {
	$title = $_GET['title'];
	$price = $_GET['price'];
}
```
#### The OD way
Same form, but add a controller
```
<form action="./" method="get">
	<input type="hidden" name="control" value="Save_Product" />
	<input type="text" name="title" />
	<input type="text" name="price" />
</form>
```
And add a file *od/classes/control/save/class-product.php*, which will automatically be called by the autoloader
```
Class Control_Save_Product {
	public function __construct()
	{
		$title = $_GET['title'];
		$price = $_GET['price'];	
	}
}
```
