---
layout: default
title: The Block System is Finally Useful in Drupal 8
author: Timani Tunduwani
category: Drupal 8
---

[Source](http://drupalize.me/blog/201402/your-first-restful-view-drupal-8 "Permalink to Your First RESTful View in Drupal 8")

# Your First RESTful View in Drupal 8

In a continuation from my first post,&nbsp;[An Introduction to RESTful Web Services in Drupal 8][1], I want to explore how Views interacts with REST in Drupal 8.

As many of you already know, the Views module was added to Drupal 8 Core. With RESTful Web Services also in Core, we now have all the tools we need to create highly customizable solutions out of the box.

In this blog post, I will show you how to create a view that returns a list of content in JSON via the REST API. Let’s get started!

First things first, we want to make sure both the&nbsp;REST&nbsp;and&nbsp;Serialization&nbsp;modules are enabled. Then we need to create some dummy content. To do this, grab the latest Drupal 8 development release of&nbsp;[Devel][2]&nbsp;module, and enable just the Devel generate submodule. Navigate to _Configuration&nbsp;&gt;&nbsp;Development&nbsp;&gt;&nbsp;Generate content,_ and create a bunch of articles.

![][3]

Now let’s create our view. We do not need to create a page or block, so uncheck those options to keep things simple.

![][4]

After creating the view, you’ll notice we only have a Master display. For REST to work, we need to add a REST export display.

![][5]

On the new REST export display, we need to set a path. This is the URL that will be used by clients to return the contents of the view. I’m using the following structure to keep things organized, but you can use anything you like.

![][6]

REST export displays have a single output format, called Serializer It&nbsp;converts output into the format the client requests. In other words, when the client calls the URL we set above, they can specify the desired format output. If a format is not specified, Views returns JSON by default. There are 3 formats available: HAL, JSON, and XML. By default the client can request any of these formats, but if you open the Serializer settings dialog (found in the Format section of the view) you can specify which request formats are accepted.

![][7]

REST export lets you to return either fields or entities. By setting our view to return entities, we'll get the entire serialized node object. And just like any view, selecting fields lets us pick the fields from the entity we wish to return. For the sake of simplicity here, I've opted to return the content title only.

Remember that we’re using Views, and we have access to its awesome features. We can set permissions to access the path, sort results by custom criteria, and limit the number of results. Views caching is still a work in progress, but once its functional we'll also be able to cache results like any other view. It’s brilliant!

Contextual filters work too. For example we could add the content author UID contextual filter, and clients could then append it to the end of the path (e.g., rest/views/articles/1) to filter results by a specific author.All that's left now is to give it a test run. Make sure to save the view, and then try accessing the URL directly in your browser. As we're not specifying a format, JSON is returned by default.


    [{"title":"Eu Populus Wisi"},{"title":"Distineo Gemino Sit Sudo"},{"title":"Tation"},{"title":"Blandit"},{"title":"Consectetuer Lucidus Patria"},{"title":"Quae Valde"},{"title":"Ad Jumentum Quibus"},{"title":"Aliquam Jugis Saluto Ymo"},{"title":"Exputo Sagaciter Te"},{"title":"Bene Causa Neo Torqueo”}]

We can change aliases used for fields by opening the field settings dialog. This is useful if we want something user friendly and don't want to use default field labels.

![][8]

I recommend using the&nbsp;[Dev HTTP Client chrome extension][9] for testing REST APIs. It will allow you to set the&nbsp;_Accept header_&nbsp;to return something other than JSON, and it formats the output to assist with debugging. It’s also worth noting that you can only use GET. Views won’t accept POST, PUT, PATCH, or DELETE operations.

![][10]

I hope this was helpful for those of you who are excited about RESTful Web Services in Drupal 8 Core. Building a REST API for an application has always been tricky. Hopefully with the inclusion of REST and Views in Drupal Core, we’re one step closer to standardizing the process.

In a future blog post, I will dive into the [OAuth][11] Contrib module and how we can use it to improve the authentication security of our REST APIs.&nbsp;As always, feel free to leave comments and questions below. Thanks!

   [1]: http://drupalize.me/blog/201401/introduction-restful-web-services-drupal-8
   [2]: http://drupal.org/project/devel
   [3]: http://drupalize.me/sites/default/files/resize/blog_post_images/generate-775x574.png (Devel generate module)
   [4]: http://drupalize.me/sites/default/files/resize/blog_post_images/add-view-775x518.png
   [5]: http://drupalize.me/sites/default/files/blog_post_images/add-rest.png
   [6]: http://drupalize.me/sites/default/files/resize/blog_post_images/rest-url-775x221.png
   [7]: http://drupalize.me/sites/default/files/blog_post_images/style-options.png
   [8]: http://drupalize.me/sites/default/files/blog_post_images/row-style.png
   [9]: http://www.google.co.uk/url?sa=t&amp;rct=j&amp;q=&amp;esrc=s&amp;source=web&amp;cd=1&amp;cad=rja&amp;ved=0CCwQFjAA&amp;url=https%3A%2F%2Fchrome.google.com%2Fwebstore%2Fdetail%2Fdev-http-client%2Faejoelaoggembcahagimdiliamlcdmfm%3Fhl%3Den&amp;ei=cQb-Up_PD4_K0AXJwYCABA&amp;usg=AFQjCNF12H1i89mr-BXv7ZZk7E7X9Q9s9w&amp;bvm=bv.61190604,d.d2k
   [10]: http://drupalize.me/sites/default/files/resize/blog_post_images/output-775x432.png
   [11]: http://drupal.org/project/oauth
  