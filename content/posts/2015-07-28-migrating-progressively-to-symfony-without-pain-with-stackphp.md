---
title: Migrating progressively to Symfony without pain with StackPHP
author: Carlos Buenosvinos
type: post
date: 2015-07-28T03:00:09+00:00
url: /migrating-progressively-to-symfony-without-pain-with-stackphp/
dsq_thread_id:
  - "4659317429"
categories:
  - PHP
tags:
  - apache dumper
  - atrapalo
  - framework
  - httpkernelinterface
  - migrating
  - php
  - stackphp
  - symfony

---
In the previous post, I talked about <a href="http://carlosbuenosvinos.com/migrating-progressively-to-symfony-without-pain/" target="_blank">how to migrated to Symfony without pain using Apache Dumper</a>. The idea was to generate Symfony routes in an Apache configuration file or .htaccess so it can be included in your virtual host. By including a fallback route to your current framework entry point, you can create new routes in Symfony without touching your previous framework. You can develop normally your new Symfony app, just defining new routes or the same old ones and regenerating the routes file.

This approach has some small pitfalls. Each time a new Symfony route is created, the Apache configuration file with the routes must be regenerated. If you&#8217;re creating many routes, this can be annoying. As explained in the previous post, there is another option that fixes this issue and have more features, <a href="http://stackphp.com/" target="_blank">Stack</a>. Now, it&#8217;s like in <a href="http://www.atrapalo.com" target="_blank">Atrápalo</a>, let&#8217;s see how it&#8217;s working.<!--more-->

### What is Stack?

Stack is a library to merge different applications that know how to handle Symfony Requests and Responses. A Request gets into the stack and each middleware in the stack can modify the Request, generate a new Response, directly serve the Response, set the authentication for all the middlewares, etc.

Quoting its web, &#8220;The [HttpKernelInterface][1] models web request and response as PHP objects, giving them value semantics. Stack is a convention for composing HttpKernelInterface middlewares. By wrapping your application in decorators you can add new behaviour from the outside. Current supported frameworks are Symfony, Silex, Laravel, Drupal, and much more.&#8221;

In this case, we are going to use it in order to migrate from a custom framework to Symfony without pain.

### How have Atrápalo integrated its previous framework?

To sum up, the idea is making your current framework stackable and stack the new Symfony app. If Symfony can resolve the url, it goes for it, if not, the next middleware, our previous app, takes the responsibility. With this situation, we don&#8217;t need to update any routing file, everything works as a Symfony Developer may expect.

### What are the steps?

Despite being a custom framework, Atrápalo has a single entry point and a bootstrapping process. If it&#8217;s not your situation, sorry dude. The trick is making your bootstrap implement the HttpKernelInterface. That means, receiving a HttpFoundation Request and returning a HttpFoundation Response.

### Making any framework HttpKernelInterface compatible

It does not matter what framework you have, in the 90% of the cases, you can create a HttpFoundation Response using a simple technique. Let&#8217;s see an example.

<pre class="brush: php; gutter: true">ob_start();

try {
    // Your framework initialization and dispatching
    $this-&gt;initializeApplication()-&gt;run();
} catch (Exception $e) {
    // serviceUnvailablePage($e);
    // Die, page 404, etc.
}

// Getting the headers sent
$headers = headers_list();

// Remove them from the buffer not to send them duplicated
header_remove();

// Create the Symfony Response with the ob content
$response = Response::create(ob_get_clean(), http_response_code());

// Adding the headers
foreach ($headers as $header) {
    $pieces = explode(&#039;:&#039;, $header);
    $headerName = array_shift($pieces);
    $response-&gt;headers-&gt;set($headerName, trim(implode(&#039;:&#039;, $pieces)), false);
}

return $response;
</pre>

The main idea is using output buffer to capture whatever happens in your framework, and build a Response with the content, response code, headers and cookies. With this trick, your framework is now compatible.In order to stack it, we need to make our Bootstrap class implement HttpKernelInterface and TerminableInterface, in case you have some shut down step.

<pre class="brush: php; gutter: true">class Bootstrap implements HttpKernelInterface
{
    public function initializeApplication()
    {
        // ...
    }

    public function run()
    {
        // ...
    }

    public function handle(Request $request, $type = self::MASTER_REQUEST, $catch = true)
    {
        ob_start();

        try {
            $this-&gt;initializeApplication()-&gt;run();
        } catch (Exception $e) {
            // serviceUnvailablePage($e);
        }

        $headers = headers_list();
        header_remove();

        $response = Response::create(ob_get_clean(), http_response_code());
        foreach ($headers as $header) {
            $pieces = explode(&#039;:&#039;, $header);
            $headerName = array_shift($pieces);
            $response-&gt;headers-&gt;set($headerName, trim(implode(&#039;:&#039;, $pieces)), false);
        }

        return $response;
    }
}
</pre>

Now, our Bootstrap class is **stackable**.

### The new entry point

As a Symfony Developer, when you use StackPHP, your entry point changes a bit. Let&#8217;s see our new app.php entry point.

<pre class="brush: php; gutter: true">use MyOldApp\Bootstrap;
use Mouf\StackPhp\SymfonyMiddleware;
use Symfony\Component\HttpFoundation\Request;
 
$loader = require __DIR__.&#039;/../app/bootstrap.php.cache&#039;;
 
require_once __DIR__.&#039;/../app/AppKernel.php&#039;;
$kernel = new AppKernel($_SERVER[&#039;SYMFONY_ENV&#039;], $_SERVER[&#039;SYMFONY_DEBUG&#039;]);
$kernel-&gt;loadClassCache();
 
// Let&#039;s create the Stack
$stack = new Stack\Builder();
 
// Let&#039;s stack our Symfony Application
$stack-&gt;push(SymfonyMiddleware::class, $kernel);
 
// Let&#039;s run the stack with our old app in a lazy load (so it does not get bootstrapped)
Stack\run(
    $stack-&gt;resolve(
        Stack\lazy(function() {
            return new Bootstrap();
        })
    )
);
</pre>

Done! You can develop your new routes in Symfony without restarting or generating any file. Let me know if it works for you. For your information, we are using stack for other features such as mobile redirection, caching, and much more. Hope it helps someone! Thanks to <a href="https://twitter.com/theUniC" target="_blank">@theUniC</a> for discovering to me StackPHP :)

### References

<a href="http://stackphp.com" target="_blank">http://stackphp.com</a>
  
<a href="https://github.com/thecodingmachine/symfony-middleware/" target="_blank">https://github.com/thecodingmachine/symfony-middleware/</a>
  
<a href="http://carlosbuenosvinos.com/migrating-progressively-to-symfony-without-pain/" target="_blank">http://carlosbuenosvinos.com/migrating-progressively-to-symfony-without-pain/</a>

 [1]: https://github.com/symfony/symfony/blob/master/src/Symfony/Component/HttpKernel/HttpKernelInterface.php