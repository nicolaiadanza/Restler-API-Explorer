> **Note:-** Newer version of API Explorer that currently targets swagger 1.2
> specification is currently bundled with [Restler RC6](https://github.com/Luracast/Restler/tree/v3/vendor/Luracast/Restler/explorer).
> Use the `Explorer` class instead of `Resources` class in the below example and
> ignore all the steps for building and copying the explorer folder
> as it is no longer required.

Restler API Explorer
====================

Restler API Explorer is tweaked from Swagger UI which is part of [Swagger]
(http://swagger.wordnik.com/) project which allows you to produce,
visualize and consume your OWN RESTful services.
No proxy or 3rd party services required.  Do it your own way.

We modified Swagger UI so that it can be placed along with a [Restler](https://github.com/Luracast/Restler/) based API Server for API Discovery and Exploration.

Swagger UI is a dependency-free collection of HTML, Javascript, and CSS assets that dynamically
generate beautiful documentation and sandbox from a [Swagger-compliant](https://github.com/wordnik/swagger-core/wiki) API. Because Swagger UI has no
dependencies, you can host it in any server environment, or on your local machine.

Rester API Explorer is currently targeting swagger 1.1 specification.

How to Use It
-------------

### Download
You can use the Restler API Explorer code AS-IS!  No need to build or recompile--just [download](https://github.com/Luracast/Restler-API-Explorer/zipball/master) the distribution,
unpack and take the content inside `dist` folder and start using it.  If you like Restler API Explorer as-is, stop here.

### Build
You can rebuild swagger-ui on your own to tweak it or just so you can say you did.  To do so, follow these steps:

1. Clone this repository. Downloading as Zip or Tarball won't include the source
2. Install [CoffeeScript](http://coffeescript.org/#installation) which will give you [cake](http://coffeescript.org/#cake)
3. Install [handlebars](http://handlebarsjs.com/) using `npm install handlebars -g`
4. Run `cake dist` from the root of this project
5. You should see the distribution under the dist folder. Open `./dist/index .html` to launch API Explorer in a browser

### Use
1. Copy the dist folder to the Restler (version 3 or above) based API root, and rename it to **explorer** (or whatever you find suitable)
2. Add `Luracast\Restler\Resourses` as an API class as shown below
3. Open the API Root/explorer in the browser. It should be listing the resources
4. Add proper PHPDoc comments to your API methods and see them magically appear in the explorer :)

```php
<?php
use Luracast\Restler\Restler;
require_once '../../vendor/restler.php';

$r = new Restler();
$r->addAPIClass('Luracast\\Restler\\Resources'); //this creates resources.json at API Root
$r->addAPIClass('YourOwnCustomAPIClass');
//... add more api classes if needed
$r->handle();
```

### Customize
You may choose to customize API Explorer for your organization. Here is an overview of whats in its various directories:

-    dist: Contains a distribution which you can deploy on a server or load from your local machine.
-    bin: Contains files used by swagger-ui for its build/test. These are not required by the distribution.
-    lib: Contains javascript dependencies which swagger-ui depends on
-    node_modules: Contains node modules which swagger-ui uses for its development.
-    src
    -    src/main/coffeescript: main code in CoffeeScript
    -    src/main/templates: [handlebars](http://handlebarsjs.com/) templates used to render swagger-ui
    -    src/main/html: the html files, some images and css
    -    src/main/javascript: some legacy javascript referenced by CofffeeScript code

### SwaggerUI
To use swagger-ui you should take a look at the [source of swagger-ui html page](https://github.com/wordnik/swagger-ui/tree/master/src/main/html) and customize it. This basically requires you to instantiate a SwaggerUi object and call load() on it as below:

```javascript
    window.swaggerUi = new SwaggerUi({
        discoveryUrl:"http://petstore.swagger.wordnik.com/api/resources.json",
        dom_id:"swagger-ui-container",
        apiKey:"special-key",
        supportHeaderParams: false,
        headers: { "Authorization": "XXXX", "someOtherHeader": "YYYY" },
        supportedSubmitMethods: ['get', 'post', 'put']
    });

    window.swaggerUi.load();
```

* *discoveryUrl* parameter should point to a resource listing url as per [Swagger Spec](https://github.com/wordnik/swagger-core/wiki)
* *dom_id parameter* is the the id of a dom element inside which SwaggerUi will put the user interface for swagger
* *booleanValues* SwaggerUI renders boolean data types as a dropdown. By default it provides a 'true' and 'false' string as the possible choices. You can use this parameter to change the values in dropdown to be something else, for example 0 and 1 by setting booleanValues to new Array(0, 1)
* *docExpansion* controls how the API listing is displayed. It can be set to 'none' (default), 'list' (shows operations for each resource), or 'full' (fully expanded: shows operations and their details)
* *onComplete* is a callback function parameter which can be passed to be notified of when SwaggerUI has completed rendering successfully.
* *onFailure* is a callback function parameter which can be passed to be notified of when SwaggerUI encountered a failure was unable to render.
* All other parameters are explained in greater detail below


### HTTP Methods and API Invocation
swagger-ui supports invocation of all HTTP methods APIs but only GET methods APIs are enabled by default. You can choose to enable other HTTP methods like POST, PUT and DELETE. This can be enabled by [setting the supportedSubmitMethods parameter when creating SwaggerUI instance](https://github.com/wordnik/swagger-ui/blob/f2e63c65a759421aad590b7275371cd0c06c74ea/src/main/html/index.html#L49).

For example if you wanted to enable GET, POST and PUT but not for DELETE, you'd set this as:

    supportedSubmitMethods: ['get', 'post', 'put']

_Note that for POST/PUT body, you'd need to paste in the request data in an appropriate format which your service can unmarshall_

### Header Parameters
header parameters are supported. However because of [Cross-Origin Resource Sharing](http://www.w3.org/TR/cors/) restrictions, swagger-ui, by default, does not send header parameters. This can be enabled by [setting the supportHeaderParams to true when creating SwaggerUI instance](https://github.com/wordnik/swagger-ui/blob/f2e63c65a759421aad590b7275371cd0c06c74ea/src/main/html/index.html#L48) as below:

    supportHeaderParams: true

### Custom Header Parameters - (For Basic auth etc)
If you have some header parameters which you need to send with every request, use the headers as below:

     headers: { "Authorization": "XXXX", "someOtherHeader": "YYYY" }

### Api Key Parameter
If you enter an api key in swagger-ui, it sends a parameter named 'api\_key' as a query (or as a header param if you've enabled it as described above). You may not want to use the name 'api\_key' as the name of this parameter. You can change its name by setting the _apiKeyName_ parameter when you instantiate a SwaggerUI instance. For example to call it 'sessionId'

    apiKeyName: "sessionId"

How to Improve It
-----------------

Create your own fork of [Luracast/Restler-API-Explorer](https://github.com/Luracast/Restler-API-Explorer).

To share your changes, [submit a pull request](https://github.com/Luracast/Restler-API-Explorer/pull/new/master).

License
-------

Copyright 2011-2012 Wordnik, Inc.

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at [apache.org/licenses/LICENSE-2.0](http://www.apache.org/licenses/LICENSE-2.0)

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
