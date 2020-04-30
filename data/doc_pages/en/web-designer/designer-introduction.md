## Requirement

In order to build the asset you need to install yarn

- Please install it : [https://classic.yarnpkg.com/fr/docs/install](https://classic.yarnpkg.com/fr/docs/install)

If you are not familiar with yarn here are the main cmd:

- `yarn start`         => start yarn and rebuild asset after each change
- `yarn build:dev`     => build you asset in dev mode (without any compression, optimisation)
- `yarn build:prod`    => build your asset in production mode (use it before deploying)

[comment]: <> TODO we should build a tools that wrapper the docker image and install it here

## Introduction

As we strongly believe in OpenSource, we provide an OpenSource Template that you can use for all your new project.
This template is the one used to power up the demo Shopinvader website.

In your folder of the clone repo from shopinvader-getting-started.git run the following command:

```
git clone https://github.com/shopinvader/shopinvader-template template
```


To start developping a theme for shopinvader you need to understand 2 things:

- the template structure
- what is wagon


## Template structure

A LocomotiveCMS template is

```
- app
  + assets/*
  + content_types/*
  + views
    + pages/*
    + sections/*
    + snippets/*
- config
  + deploy.yml
  + site.yml
  + translation.yml
  + metafields_schema.yml
- data
  + customers.yml
  + posts_groups.yml
  + posts.yml
- public
```

- app/assets: Your css and javascript

- app/content_types: The custom model that you want for your website

- app/views: pages, sections, snippets for your html

- config: the configuration of your site

- data: your fake demo data


## Wagon cli

### Introduction

Wagon is the best friend of the webdesigner !

You should see this like

- a mini local locomotive server
- a tool for deploying your website
- a command line cli for interacting with your locomotive


```
docker-compose run --service-port wagon
```

### Yarn

Before playing with wagon you need to build your asset.
If you forget this step the rendering will be broken.

In you terminal inside the template repository

Lauch the following command to install javascript dependency

```
yarn install
```

Then you can launch the start command to build and watch change (with auto-rebuild)

```
yarn start
```

Or you can build the dev or prod asset

```
yarn build:dev
```

```
yarn build:prod
```

### Wagon serve

Let's try to render your website like a webdesigner !

In the docker wagon shell launch

`wagon serve`

Now go on `http://wagon.localhost`


You have a local rendering of the website.

Everything you see just come from your templating and your demo data

Try to open the file `/data/posts.yml` modify some text and go to the blog page to see the result


Try to open the file `/app/views/pages/layouts/default.liquid`
and add the following line


```
{% extends 'layouts/base' %}
{% block 'body' %}
  &lt;header>
      {% block 'header' %}
+      <div class="alert alert-success"> you did it! </div>
      {% global_section 'header', id:'page_header',  placement: 'top'%}
    {% endblock %}
  &lt;/header>
```


Save it and refresh you wagon page. You should see an alter in your header

### wagon deploy

Now that you know how to customise your template, let's deploy it !

But first take a look to the `deploy.yml` file in the config directory.

```
test:
  host: http://locomotive.localhost
  handle: shopinvader
  email: demo@shopinvader.com
  api_key: d49cd50f6f0d2b163f48fc73cb249f0244c37074
```

This file is for storing the API key of your different environment.
You can have as many key you want (test, preprod, production...)

Let's deploy, now run into your docker wagon (-v is for verbose mode)

`wagon deploy test -v`


Check the result on [http://my-website-1.localhost](http://my-website-1.localhost)

The alert is here but not the change you have done on the blog post.

This is normal because we only have pushed the template and not the data.
In production you should not push data (because user will create the blog post,...)

But for testing it can be really usefull to push the data.

You can push all the data (even the site.yml data) by using.

`wagon deploy test -v -d`

Or just the content_entries data by pushing

`wagon deploy test -v -d -r content_entries`


## Conclusion

Now you know the basic information of the template configuration and how to use wagon.

For more information you can

- learn more on wagon here: [Wagon official doc](https://doc.locomotivecms.com/docs/deploy)
- learn more on the structure of the template: [Shopinvader Template](/docs/file-structure)
