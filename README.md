# portfolio-revamp
Revised and improved version of my personal portfolio


### To chec the version of Hugo in CMD 
From teh path C:\Windows\System
run hugo version




## Useful commands 

This builds the website in the designated folder - no need for Netlify
> blogdown:::build_site()

This is to preview the website - it does not need have been built first, you can preview what#s there
> blogdown:::preview_site()

The web server has been stopped and cannot be previewed
> blogdown::stop_server()

config.toml  
content/ 
static/ 
themes/ 
layouts/ 

config
config directory is not created by default.

content
All content for your website will live inside this directory. Each top-level folder in Hugo is considered a content section. For example, if your site has three main sections—blog, articles, and tutorials—you will have three directories at content/blog, content/articles, and content/tutorials. Hugo uses sections to assign default content types.

data
This directory is used to store configuration files that can be used by Hugo when generating your website. You can write these files in YAML, JSON, or TOML format. In addition to the files you add to this folder, you can also create data templates that pull from dynamic content.
layouts
Stores templates in the form of .html files that specify how views of your content will be rendered into a static website. Templates include list pages, your homepage, taxonomy templates, partials, single page templates, and more.

static
Stores all the static content: images, CSS, JavaScript, etc.