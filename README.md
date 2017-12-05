## Description

In this lesson you will learn what a Child Theme is and when should you use one. The step-by-step walkthrough will give you an understanding of what a Child Theme is and how to create your own Child Theme. Testing PRS!

## Objectives

At the end of this lesson, you will be able to:

*   Explain what a Child Theme is.
*   Summarize why you should use a Child Themes.
*   Create a Child Theme.

## Prerequisite Skills

You will be better equipped to work through this lesson if you have experience in and familiarity with:

*   Basic knowledge of HTML/[CSS](https://make.wordpress.org/training/handbook/theme-school/intro-to-css/)
*   Basic knowledge of [installing and activating WordPress themes](https://make.wordpress.org/training/handbook/user-lessons/choosing-and-installing-a-theme/)
*   Understanding of how folders and files are structured
*   Ability to edit files with a text editor

## Assets

*   [Twenty Seventeen theme](https://wordpress.org/themes/twentyseventeen/ "Twenty Sixteen Theme")
*   [Sample screenshot png file](https://make.wordpress.org/training/files/2017/05/screenshot.png)

## Screening Questions

*   Are you familiar with installing and activating themes via the WordPress Dashboard?
*   Do you have at least a basic knowledge of HTML/CSS?
*   Do you feel comfortable using a text editor to edit code?
*   Will you have a locally or remotely hosted sandbox WordPress site to use during class?

## Teacher Notes

*   **Time Estimate: **45 minutes
*   Performing a live demo while teaching the steps to make a child theme is crucial to having the material "click" for students.
*   It is easiest for students to work on a locally installed copy of WordPress. Set some time aside before class to assist students with installing WordPress locally if they need it. For more information on how to install WordPress locally, please visit our [Teacher Resources page](http://make.wordpress.org/training/teacher-resources/).
*   The preferred answers to the screening questions is "yes." Participants who reply "no" to all 4 questions may not be ready for this lesson.
*   Built using WordPress 4.7.4.

## Hands-on Walkthrough

### Introduction: What is a Child Theme

Welcome to Child Themes! Today, you are going to learn how to make a Child Theme for WordPress. One of the first things front-end designers want to do is modify the design of an existing WordPress theme. After a little investigation, they discover where the theme files live, then directly edit the files. After the next theme update, they are horrified to discover that the update completely erased all of their modifications. Many designers have learned this lesson the hard way. How do you prevent this from happening? By using a Child Theme! A Child Theme is a theme that overrides and adds elements to another theme (the "Parent" theme) without touching any of the Parent Theme's code. When the Parent Theme is updated, your changes in the Child Theme will be preserved.

* * *

### Why use a Child Theme?

The #1 Rule of WordPress development is to **never directly modify WordPress files**. **This means do not edit:**

*   WordPress core files
*   Plugin files
*   Theme files*

*Exception: starter themes that have been intentionally created by theme builders for you to modify. **Why?**

*   *   **Updates wipe out customization changes**

If you update a modified theme, the update will overwrite all customizations. Similarly, when you update a plugin, the update will overwrite any edits you've made. Same for WordPress core files.

*   *   **Themes can get broken**

If you edit theme files directly and make a mistake that cannot be undone, you are stuck with a broken theme.

*   *   **WordPress and WordPress Plugins may not work with theme hacks**

You may inadvertently remove elements that WordPress and WordPress plugins look for in a theme, so they no longer work. **So how do you safely customize a WordPress theme?** Create a **Child Theme** that is a "**child**" of another theme (the Parent Theme).

*   The Child Theme overrides selected design elements and otherwise falls back to the Parent.
*   The Child Theme can also override or add functionality to the Parent Theme.

* * *

### How Child Themes work

A Child Theme loads first, before the Parent, and only contains overrides and additions to the Parent Theme.

![](https://make.wordpress.org/training/files/2017/05/file-flow-diagram-1-1024x248.png)

All of your CSS, templates, images, and other files are kept in the Child Theme's folder while the original Parent Theme's files are left intact. If something breaks, you can simply delete or fix the offending file in the Child Theme folder.

* * *

### Creating a Child Theme

You are going to create a Child Theme of the default WordPress theme [Twenty Seventeen](https://wordpress.org/themes/twentyseventeen/ "Twenty Sixteen Theme"). We'll name this Child Theme "**MyChildTheme**". A Child Theme only needs a few things to get up and running: a theme folder, a CSS file, and a screenshot file.

#### Step 1: A Theme Folder

Every theme for WordPress needs its own folder. Take a look at the folder structure of WordPress. You can see each installed theme's folder in `/wp-content/themes` 

![](https://make.wordpress.org/training/files/2017/05/folders-one-new.png)

Create a folder for your Child Theme. The folder name should be all lowercase with no spaces. In our example the Child Theme's folder is called "**mychildtheme**." 

![](https://make.wordpress.org/training/files/2017/05/childthemes-file-path-new-folder.png)

#### Step 2: A style.css file

At a minimum, your Child Theme needs a `style.css` file. The `style.css` file tells WordPress to load the Parent Theme's files after the Child. Place this file inside the Child Theme's folder. Make sure it is in the root level of the Child Theme folder and not inside a subfolder. The `style.css` file needs the following code at the top:

```css
/*
 Theme Name: [Your Theme Name]
 Description: The custom theme [Your Theme Name] using the parent theme Twenty Seventeen.
 Author: [You]
 Author URI: [Your URL]
 Template: <span style="color: #000000">twentyseventeen</span>
 Version: 1
 */
```

There are other variables you can include, but these are the most important ones you should include to identify your Child Theme. Here is an explanation of the different variables in `style.css`:

*   **Theme Name:** The Name of the theme. This is the name that shows up in the WordPress Dashboard under Appearance > Themes.
*   **Description:** A short description for the theme. You can put anything you like here. This shows up in the WordPress Dashboard under Appearance > Themes once the theme is activated.
*   **Author:** The author of the Child Theme, which can be a person's name or company name.
*   **Author URI:** The URL for the author of the Child Theme.
*   **Template:** <span style="color: #ff0000">**Very Important!**</span> This is **the folder name of the parent theme**. If this variable is not correct the Child Theme will not work.
*   **Version:** The version of the Child Theme

All of these variables are optional, with the exception of `**Template:**`. If this line is not present or contains typos the Child Theme will not work.

### Step 3: Enqueue parent and child theme style sheets

Your Child Theme needs to call the `style.css` files using a method called "enqueueing scripts." To add the calls for the parent and child theme stylesheets to your child theme, first you need to create a `functions.php` file. Place this file inside the Child Theme's folder. Make sure it is in the root level of the Child Theme folder and not inside a subfolder. [tip]Note that `functions.php` in the child theme does not replace `functions.php` in the parent theme. This is where you can put hooks, actions, and filters that modify or add functionality to the parent theme, rather than replacing it.[/tip] The first line of your child theme's `functions.php` will be an opening PHP tag (`<?php`), after which you can enqueue your parent and child theme stylesheets. The correct method of enqueuing the parent theme stylesheet is to add a `wp_enqueue_scripts` action and use `wp_enqueue_style()` in your child theme's `functions.php`. The following example function will only work if your Parent Theme uses only one main `style.css` to hold all of the css. If your theme has more than one .css file (eg. `ie.css`, `style.css`, `main.css`) then you will have to make sure to maintain all of the Parent Theme dependencies.

```php
<?php
function mychildtheme_enqueue_styles() {
   wp_enqueue_style( 'parent-style', get_template_directory_uri() . '/style.css' );
}
add_action( 'wp_enqueue_scripts', 'mychildtheme_enqueue_styles' ); 
?>
```

This line needs to point to the Parent Theme’s `style.css` file. Your Child Theme's `style.css` file can be empty. But if it contains CSS code, as it usually will, you will need to enqueue it as well. Setting 'parent-style' as a dependency will ensure that the child theme stylesheet loads after it.

```php
function mychildtheme_enqueue_styles() {
    $parent_style = 'parent-style';

    wp_enqueue_style( $parent_style, get_template_directory_uri() . '/style.css' );
    wp_enqueue_style( 'child-style',
        get_stylesheet_directory_uri() . '/style.css',
        array( $parent_style )
    );
}
add_action( 'wp_enqueue_scripts', 'mychildtheme_enqueue_styles' );
```

This is the recommended way to enque the styles for your Child Theme. [warning]The old way of enqueuing scripts and styles was to use `@import url("../parentfolder/style.css");`, and you'll still see old articles online that show that technique. But this is very inefficient, so a better way is to use the `wp_enqueue_style()` method covered here.[/warning]

### Step 4: A screenshot.png file

A theme's screenshot is the thumbnail image that shows up under Appearance > Themes in the WordPress Dashboard. A screenshot image is not required for a Child Theme, but it will look sad without one. 

![](https://make.wordpress.org/training/files/2017/05/screenshot-1-300x225.png)

The recommended image size is 880x660. The screenshot will only be shown as 387x290, but the larger image allows for high-resolution viewing on HiDPI displays. Create a 880px by 660px image file, name it "screenshot.png", and place it into the child theme's folder. If you don't have an image editor, download a [sample screenshot.png file](https://make.wordpress.org/training/files/2017/05/screenshot.png).

* * *

### Activate the Child Theme

You now have everything you need to use a Child Theme! Make sure the Child Theme folder containing at least `style.css` is uploaded or pushed to `/wp-content/themes` on the webserver, or your computer if you are working on a local WordPress install. After you add the theme folder to `/wp-content/themes`, go to Appearance > Themes in the Dashboard. You should see your theme listed. 

![](https://make.wordpress.org/training/files/2017/05/my-child-theme-activate-1024x574.png)

Hover over your theme to reveal the "Activate" button. Click to activate your theme. Once activated, the site will not look any different on the front-end, but the Child Theme will be the theme in charge. You can now see your theme labeled as "active". 

![](https://make.wordpress.org/training/files/2017/05/my-child-theme-active-1024x574.png)

* * *

### Child Theme Files

The files in the example Child Theme illustrate how a Child Theme's files affect the Parent's files: they either override elements and add functionality to its identically named file, or completely replaces it. 

![](https://make.wordpress.org/training/files/2017/05/file-flow-diagram-2.png)

`style.css` in MyChildTheme overrides elements and adds to `style.css` in Twenty Seventeen, while `screenshot.png` completely replaces the copy of `screenshot.png` in Twenty Seventeen.

* * *

### Overriding the Parent Theme's CSS

The Child Theme's `style.css` file will override any styles in the Parent Theme's `style.css` file that have the same selectors. Let's say you wanted to change the size of the Site Title in the header. Inspecting that element reveals the CSS selector `.site-title` shows that the Parent Theme sets the font size at 2.25rem. (This is using the inspector in Google Chrome, but all the major browsers show this information in different formats.) 

![](https://make.wordpress.org/training/files/2017/05/Inspect-Site-Title-1024x574.png)

In the Child Theme's `style.css` file, add the selector and the font-size you want to change the Site Title to:

```css
.site-title {
    font-size: 4.75rem;
    line-height: 1.25;
}
```

Now the Site Title is 4.75rem instead of 1.75rem. 

![](https://make.wordpress.org/training/files/2017/05/Inspect-Site-Title-Enlarged-1024x574.png)

* * *

### Overriding the Parent Theme's Templates

[Templates](http://codex.wordpress.org/Templates) are the files that control how your WordPress site will be displayed on the Web. Inside the `twentyseventeen` folder are all of Twenty Seventeen's template files. You can create your own versions of these files in your Child Theme. 

![](https://make.wordpress.org/training/files/2017/05/twenty-seventeen-files.png)

Let's say you want to replace the text "Proudly powered by WordPress" in the footer with a copyright. Here's how it looks now: 

![](https://make.wordpress.org/training/files/2017/05/twenty-seventeen-default-footer.png)

Open `footer.php` in the `twentyseventeen` folder. You can see the following line of code that needs to be edited. This statement pulls in a template with the "Proudly powered by WordPress" message. You'll no longer need to use that template file, so this is the statement you'll replace.

```php 
get_template_part( 'template-parts/footer/site', 'info' );
 ?>
```

Save a copy of `footer.php` into the Child Theme folder. You can safely edit this Child Theme file, leaving the original copy of `footer.php` in `wp-content/themes/twentyseventeen` intact. Just like your other Child Theme files, `wp-content/themes/mychildtheme/footer.php` will override the Parent copy. To display a Copyright line, replace the content above in `footer.php` in `wp-content/themes/mychildtheme` with the following code:

```php 
?>
 <div class="site-info">
    Copyright &copy; <?php echo date('Y'); ?>
 </div><!-- .site-info -->
```

The result on the front-end of the site: 

![](https://make.wordpress.org/training/files/2017/05/mychildtheme-footer.png)

* * *

### Adding new Templates

In addition to being able to override existing templates with a Child Theme, you can also create new templates. Let's say you want to add a new Template without a sidebar. Make a copy of `index.php` in your Child Theme and rename it `index-nosidebar.php`. Edit the existing code at the top:

```php
<?php
/**
* The main template file
*
* This is the most generic template file in a WordPress theme
* and one of the two required files for a theme (the other being style.css).
* It is used to display a page when nothing more specific matches a query.
* E.g., it puts together the home page when no home.php file exists.
*
* @link https://codex.wordpress.org/Template_Hierarchy
*
* @package WordPress
* @subpackage Twenty_Seventeen
* @since 1.0
* @version 1.0
*/

get_header(); ?>
```

to the following:

```php 
<?php
 /*
 Template Name: Page with no sidebar
 */

 get_header(); ?>
```

The name of the template goes after the variable `Template Name:`. Finally, find and remove the line of code near the end of the file which loads the sidebar. This is called `get_sidebar();`:

```php
</main><!-- #main --> 
    </div><!-- #primary --> 
    <?php get_sidebar(); ?>  // DELETE this line!
</div><!-- .wrap -->
<?php get_footer();  // leave this line in!
```

The new template will now appear under **Page Attributes** on the Edit Page screen: 

![](https://make.wordpress.org/training/files/2017/05/no-sidebar-template.png)

* * *

### Summary

Your Child Theme now has one file that overrides elements in its Parent Theme's file, two files that replace files in the Parent Theme, and one brand new file that does not exist in the Parent Theme.

![](https://make.wordpress.org/training/files/2017/05/file-flow-diagram-3.png)

When you update the Parent Theme with a new version, none of these Child Theme files will be modified. Child Themes are a safe and powerful way to override and add elements to an existing theme. Now that you know how to make one, go forth and make some awesome looking sites!

## Exercises

**Create a child theme of Twenty Seventeen**

*   What folders and files did you need to create?
*   Did your theme show up under Appearance > Themes in the Dashboard? If not, what steps did you take to troubleshoot?

**Change all the links in your theme to red, active links to green, and visited links to orange**

*   What file did you need to edit to do accomplish this?
*   What selectors did you apply the new color styles to?
*   How did you determine the selectors to use?

**Change the "Proudly powered by WordPress" message in the site footer**

*   What file did you need to create?
*   What steps did you take to create the file and change the message?

**Make a new template for the home page with no sidebar.**

*   What file did you create?
*   What steps did you take to create the file?
*   What code did you add to make it a new template?
*   How did you remove the sidebar?
*   How did you assign the new template to the home page of your site?

## Quiz

**When developing for WordPress, what files should you never edit?**

1.  Core WordPress files
2.  Plugin files
3.  Theme files
4.  All of the above

**Answer:** 4\. All of the above

* * *

**What file _must_ you create to make a child theme, in addition to the child theme folder?**

1.  functions.php
2.  index.html
3.  style.css
4.  readme.html
5.  license.txt

**Answer:** 3\. style.css

* * *

**What items do you have to include in the style.css file of your Child Theme in order for it to work properly?**

1.  `Theme Name: [Name] and Template: [parentfolder]`
2.  `Theme Name: [Name] and Version: [#]`
3.  `Description: [Theme description] and wp_enqueue_style( 'parent-style', get_template_directory_uri() . '/style.css' );`
4.  `Template: [parentfolder] and wp_enqueue_style( 'parent-style', get_template_directory_uri() . '/style.css' );`
5.  `Theme Name: [Name] and wp_enqueue_style( 'parent-style', get_template_directory_uri() . '/style.css' );`

**Answer:** 4\. `Template: [parentfolder] and wp_enqueue_style( 'parent-style', get_template_directory_uri() . '/style.css' );`

* * *

**How do you get a picture of your theme to show up under Appearance > Themes in the Dashboard?**

1.  Upload screenshot.png to the child theme folder
2.  Upload screenshot.png to the the parent theme folder
3.  Upload screenshot.png to the /images folder of /wp-admin/
4.  Upload screenshot.png to the /images folder in /wp-content/

**Answer:** 1\. Upload screenshot.png to the child theme folder

## Additional Resources

[Child Themes](https://codex.wordpress.org/Child_Themes) in the WordPress Codex
