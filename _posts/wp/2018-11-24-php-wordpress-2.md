---
title: PhP Wordpress 2
categories: web
tags: [php, website building, mysql, wordpress]
toc: 1
---

I use this note for all I've learned when I build again website math2it.com using Wordpress. Different from [Note of Wordpress 1](/php-wordpress-1), in this note, I focus on what I will use to build the theme.

<div class="see-again">
<i class="material-icons">settings_backup_restore</i>
<span markdown="1">
[Back to PHP Wordpress 1](/php-wordpress-1).
</span>
</div>

## Include Bootstrap CSS

In WP1, use below codes to apply custom css in folder **css**. However, I want to insert _one time_ all files in some folder, e.g. _bootstrap41_, and I wanna use **scss** instead of css. How can I do those?

~~~ php
function math2itwp_scripts() {
	wp_enqueue_style( 'bootstrap', get_template_directory_uri() . '/css/bootstrap.min.css', array(), '4.1.3' );
	wp_enqueue_style( 'style', get_template_directory_uri() . '/style.css' );
	wp_enqueue_script( 'bootstrap', get_template_directory_uri() . '/js/bootstrap.min.js', array( 'jquery' ), '4.1.3', true );
}
add_action( 'wp_enqueue_scripts', 'math2itwp_scripts' );
~~~

- For including a folder: NO
- For using scss/sass (I followed [this guide](https://www.elegantthemes.com/blog/tips-tricks/how-to-use-sass-with-wordpress-a-step-by-step-guide)): 

	1. Install [ruby](https://www.ruby-lang.org/en/downloads/)
	2. Update ruby (if you installed it before) and install **[compass](http://compass-style.org/install/)**,

		~~~ bash
gem update --system
gem install compass
		~~~

	3. `cd` to the current wordpress theme
	4. Create a folder named **sass** in the root of this theme.
	5. Create a file **config.rb** which contains below codes, this file is for _compass_

		~~~ ruby
		http_path = "/" #root level target path
		css_dir = "." #targets our default style.css file at the root level of our theme
		sass_dir = "sass" #targets our sass directory
		images_dir = "images" #targets our pre existing image directory
		javascripts_dir = "js" #targets our JavaScript directory

		# You can select your preferred output style here (can be overridden via the command line):
		# output_style = :expanded or :nested or :compact or :compressed

		# To enable relative paths to assets via compass helper functions.
		# note: this is important in wordpress themes for sprites

		relative_assets = true
		~~~
	
	6. Inside folder **sass**, create a folder named **_partials**, all partial scss files will be located in this folder. For example, we use **_font.scss** to store all styles relating to font for the theme.
	7. Inside folder **sass**, create a file named **style.scss**, after every modification, this file will be generated by compass and overwirte the _style.css_ file in the root theme folder. In the content of this file, you place

		~~~ css
		@import "compass";
		@import "_partials/font";
		~~~

	8. Use terminal and run following command (suppose that you are already in the theme folder). From this step, everytime you modify _style.scss_ or files in _\_partials_ folder, the _compass_ will automatically update the changes and overwrite to the **style.css** file in root theme folder. 

		~~~ bash
		compass watch
		~~~

{:#navigation}
## Navigation

### Default menu

As default, like in [WP1 note](/php-wordpress-1), we can create a "menu bar" with _already-created pages_ by 

~~~ php
<nav class="blog-nav">
	<a class="blog-nav-item active" href="<?php echo get_bloginfo( 'wpurl' );?>">Home</a>
	<?php wp_list_pages( '&title_li=' ); ?>
</nav>
~~~

### Manually

However, I wanna create a **custom navigation** bar in which there are _some fields being not a page_! I also wanna change the color of each field when it's chosen.

You can add manually by something like that in the file **header.php**,

~~~ php
<nav class="navbar navbar-dark nav-bg navbar-custom">
	<div class="container">
		<a class="nav-home" href="<?php echo get_bloginfo( 'wpurl' );?>">
		<i class="icon-home"></i>
			<span>Home</span>
		</a>
		<a class="nav-custom-1" href="<?php echo get_bloginfo( 'wpurl' );?>/custom-1">
			<i class="icon-pi-outline"></i>
			<span>Custom 1</span>
		</a>
		<div class="nav-search">
			// form of search
		</div>
	</div>
</nav>
~~~

### Add custom menu with additional icon field

But I wanna add some field in **wp admin** so that I can add some fields in that (intuitively), the code in the _header.php_ is only some lines, they will automatically get the info I provide in wp-admin to show. But how?

- I wanna add custom link & custom title for that nav.
- I wanna add custom color for each nav.
- I wanna add custom icon for each nav.
- I wanna add custom "other factor" for each nav.

I followed below articles:

- For [create and display a custom menu](https://www.wpblog.com/create-custom-navigation-menu-in-wordpress-themes/) in WP. For more options, cf [wp_nav_menu](https://developer.wordpress.org/reference/functions/wp_nav_menu/)
- Use **Advanced Custom Fields**! (for localhost, download the zip file, extract to a folder and copy it to **wp-content/plugins**) and follow [this instruction](https://www.advancedcustomfields.com/resources/adding-fields-menu-items/) to add custom field to menu, like "icon", and how to display it alongside with other defaut ones.
	1. After install ACF.
	2. **Custom Fields** (in WP admin) > **Add new** (field group) > name it _Nav custom items_
	3. In **Add field**, add **icon** for example
	4. In **Location**, choose **Menu item** in _Show this field group if_ and then **equal to** > **All**
	5. Click on **Update**
	6. Go back to **Appearance** > **Menus** > click on some nav and see the new field "icon".
- Next question, how to use this field in the theme?
- Remove all class in `li` of the menu, follow [this article](https://stackoverflow.com/questions/5222140/remove-li-class-id-for-menu-items-and-pages-list?answertab=oldest#tab-top).
- By default, the menu will have a form like,

	~~~ html
	<div class="custom-nav">
		<ul>
			<li>
				<a>Menu 1</a>
			</li>
			<li>
				<a>Menu 2</a>
			</li>
			<li>
				<a>Menu 3</a>
			</li>
		</ul>
	</div>
	~~~

- Now, we only want below one, just follow [this article](https://www.designtoday.info/removing-li-menu-from-wordpress/).

	~~~ html
	<a>Menu 1</a>
	<a>Menu 2</a>
	<a>Menu 3</a>
	~~~

### Use only one search field file




## Use fontello for custom icon font

Instead of using [Awesome font](https://fontawesome.com/), we use [fontello](http://fontello.com/) for a custom icon font.

- If you need to crop an image, use [this site](https://www269.lunapic.com/editor/?action=quick-crop).
- Convert that image to svg format, use [this site](https://www.pngtosvg.com/).
- Use [fontello](http://fontello.com/) site to convert to font (drag and drop svg file to the site) and then choose the font and download.
- Create a folder named **css/fontello** and copy **fontello.css** in the downloaded zip file to it.
- Copy **font** folder in downloaded zip file to **css/**
- In **functions.php**, add

	~~~ php
	wp_enqueue_style( 'fontello', get_template_directory_uri() . '/css/fontello/fontello.css' );
	~~~

- Use `<i class="icon-math2it"></i>` for the icon where `math2it` is the name you give to fontello site before you download.


## Ad custom color to each category

Using [Advanced Custom Field](https://www.advancedcustomfields.com/resources/adding-fields-taxonomy-term/) like in [section navigation](#navigation). Note that, we need to choose **Taxonomy** in _Show this field group if_.