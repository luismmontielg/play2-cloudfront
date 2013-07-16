# Cloudfront module for play 2

Cloudfront enables to set up a CDN easily. This module helps to integrate your Cloudfront CDN with play. It also allows to point to specific URLs for particular files when you want to serve them from other CDNs.

## Setting up with sbt

Configure a new resolver:

```scala
resolvers += "Mariot Chauvin repository" at "http://mchv.me/repository"
```

Add the library dependency:

```scala
libraryDependencies += "mchv" %% "play2-cloudfront" % "1.0"
```

## Use a custom controller

Create a new file named `RemoteAssets.scala` that contains:

```scala
package controllers

import controllers._

object RemoteAssets extends Remote {
    def call(file:String) = {
        controllers.routes.RemoteAssets.at(file)
    }
}
```

Update the router file with the call to your custom controller for your assets:

```properties
# Map static resources from the /public folder to the /assets URL path
GET     /assets/*file               controllers.RemoteAssets.at(path="/public", file)
```

Update your views to refer to your controller to fetch the url:

```html
@(title: String)(content: Html)

<!DOCTYPE html>

<html>
    <head>
        <title>@title</title>
        <link rel="stylesheet" media="screen" href="@RemoteAssets.url("stylesheets/main.css")">
        <link rel="shortcut icon" type="image/png" href="@RemoteAssets.url("images/favicon.png")">
        <script src="@RemoteAssets.url("javascripts/jquery-1.9.0.min.js")" type="text/javascript"></script>
    </head>
    <body>
        @content
    </body>
</html>
```


## Configure the module with your cloudfront settings

Add your Cloudfront url and CDN paths for particular files to the `application.conf`:

```properties
remote-assets {
  default-cdn = "http://d7471vfo50fqt.cloudfront.net"
  resources {
    javascripts/jquery-1.9.0.min.js = "//ajax.googleapis.com/ajax/libs/jquery/1.9.0/jquery.min.js"
  }
}
```

## More

The module is based on [James Ward tutorial](http://www.jamesward.com/2012/08/08/edge-caching-with-play2-heroku-cloudfront)
If you want to know more or avoid the dependency, please read it.
