+++
title = "Comprehensive Theming on Open edX"
date = 2016-09-05
draft = false
[extra]
toc = true
display_published = true 
author = "dehamzah"
+++

How to change the look of Open edX, as far as i know, there are 2 methods possible, stanford theming, and comprehensive theming method. The first one soon will be obesolate, and we should start using the comprehensive method to change how the Open edX looks. The existing documentation for this comprehensive theme is quite good enough but not yet give some clue to newcomer like me to activate the theme (per this post is written).

My post here mostly takes from Open edX documentation [here](http://edx.readthedocs.io/projects/edx-installing-configuring-and-running/en/latest/configuration/theming/index.html) and [here](https://github.com/edx/edx-platform/tree/master/themes), plus my experience when developing a theme for [IndonesiaX](https://indonesiax.co.id).

Comprehensive Theming, like the documentation said, it lets us to customize the appearance of our Open edX installation. We can override Sass/CSS, images, or entire HTML templates. In this guide, i will be using Open edX `eucalyptus` as code base.


## Creating the Theme

### Theme Structure

A theme is a directory of assets. The top directory is our theme name. I use `starter-theme` as our example. The structure theme here, basicly must mimic the edx-platform theme structure. Files that we create on our theme here will overwrite the default files in edx-platform repo. So the concept is, if you want override something, just copy the default files from edx-platform repo to our theme, and start making changes from copied files.

Here is the example file structure :

```
starter-theme
└── lms
    ├── static
    │   ├── images
    │   │   └── logo.png
    │   └── sass
    │       ├── _overrides.scss
    │       ├── lms-main-v1-rtl.scss
    │       └── lms-main-v1.scss
    └── templates
        ├── footer.html
        └── header.html
```

### HTML Templates

As i said above, if want to make changes on html templates, just copy the default files, paste it to our theme templates folder and make some changes from there. Just take some notes, if we upgrade our Open edX to newer version, we may must refactor our custom template to using new Open edX templates.


### SASS/CSS

Styling in Open edX is done with Sass and then compiled to CSS. We can override the default style or add new css rules, by just mimic the file name in `edx-platform/lms/static/sass/` folder. If we want to make some changes/add in css, here is the main sass file that i recommend to override:

```
- `lms-main-v1.scss`		// For override main lms style
- `lms-main-v1-rtl.scss`	// For override main lms right-to-left style
- `lms-course.scss`			// For override style lms course
- `lms-course-rtl.scss`		// For override right-to-left style lms course
```

For example, if we just want to override main style, just create file named `lms-main-v1.scss` on our theme sass folder and `_overrides.scss` for our simple custom sass, then add this line of code on top of the file:

```
// Import the original styles from edx-platform.
@import 'lms/static/sass/lms-main-v1';

// Our custom sass start here
@import 'overrides'
```

Import the original styles corresponding to sass file we overwrite.


### Images

If you want to replace images, just mimic the file name on edx-platform repo, and put it on folder images as structure above. If you want to add new images assets, put it there too. 

Here is the example code to call the images assets on our HTML templates:

```
<img class="img-logo" src="${static.url('starter-theme/images/logo.png')}" height="50" alt="Logo" />
```

Or if you want to call it via CSS, write it like this:

```
background-image: url("../starter-theme/images/logo.png");
```


## Activating the Theme

To use our new theme, we must configure some value and update the assets. Here is the steps:

- Create folder `themes` on `/edx/app/edxapp/`
- Put our new theme on that folder. So for this example our theme location will be `/edx/app/edxapp/themes/starter-theme`
- Make some changes in `lms.env.json` located in `/edx/app/edxapp/`. Then change some variables to this:

		ENABLE_COMPREHENSIVE_THEMING: true,
		COMPREHENSIVE_THEME_DIRS: ["/edx/app/edxapp/themes"],
		DEFAULT_SITE_THEME: "starter-theme",

- Update the assets.

		$ sudo -H -u edxapp bash
		$ source /edx/app/edxapp/edxapp_env
		$ cd /edx/app/edxapp/edx-platform
		$ paver update_assets lms --settings=aws
		$ exit

	*p.s. If you are in development, i recommend you adding `--debug` on command paver. This will compile the css to extended style and showing you the compile process in terminal, so you can debug your code when it failed to compile.

- Restart the lms instance.

		$ sudo /edx/bin/supervisorctl restart edxapp:lms


Thats it. 

If you just want to test out this comprehensive theming, i had create a github repo for you to clone here:

[https://github.com/dehamzah/starter-theme-openedx](https://github.com/dehamzah/starter-theme-openedx)

I also create a simple bash script to update the assets on my gist [here](https://gist.github.com/dehamzah/0e9258ada87705791e4628e0e5aa9621).



If you have some issues just comments below, maybe i can help or maybe other readers can help too.


p.s. What if i already follow all these steps above but my Open edX theme still not changed?

Try to login to django admin on `/admin` with superuser account.
Then go to Theming > Site Themes : `/admin/theming/sitetheme/`.
In there add site theme, and put your theme name there.
If you already compile your assets, you can try refreshing your Open edX homepage, it should be changed now.

----

Note: I'm no longer developing open edx theme, if you have question, you should go to [open edx slack channel](https://openedx.slack.com/).
