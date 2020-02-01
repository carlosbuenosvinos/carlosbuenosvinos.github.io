---
title: Migrating progressively to Symfony without pain
author: Carlos Buenosvinos
type: post
date: 2015-03-31T09:47:18+00:00
url: /migrating-progressively-to-symfony-without-pain/
categories:
  - PHP
tags:
  - apache
  - dumper
  - migration
  - php
  - router
  - stack
  - symfony

---
<a href="http://www.atrapalo.com" target="_blank">Atrápalo</a> is a travel e-commerce website founded in 2000. Based in Barcelona, Spain, it sells flights, trips, tickets, booking restaurants, car renting, etc. to 10 different countries. It&#8217;s a 9000 world Alexa ranking and it&#8217;s running PHP. Since 2014, we are pushing hard in order to evolve technically using best practices, agile methodologies and distributed architectures. One of the key aspects is the framework.

We are currently migrating to Symfony in order to speed up the development process and reduce the maintenance costs. We are doing it progressively, step by step, without rewriting the whole application, no green-field project, without any dedicated team neither. All developers are involved in this process, and by policy, each new feature is developed using Symfony while the old features remain served by the old framework.

I would say this process is going quite smoothly, without pain. Based on some emails and tweets I have received, here are some tricks about how we are doing it. Hope it helps!

<!--more-->

## tl;dr

In our app, Symfony and our  current framework live together. Some requests are served by the old framework and the new features are served by Symfony, by department policy. An easy approach to do that without pain is to use the Apache dumper feature of the Symfony Router component. When deploying, an Apache config file is generated that sends all the Symfony @Route to the app_*.php file and the rest to the index.php of old framework. Done!

## Entry points

Let&#8217;s assume you have your own custom framework. We have one of those. It has a single entry point, an index.php file that manages all the incoming requests. An example of an Apache configuration file or .htaccess would be like:

<pre class="brush: shell; gutter: true">&lt;IfModule mod_rewrite.c&gt;
    Options -MultiViews

    RewriteEngine On
    # Here some of your custom rewrites...
    RewriteCond %{REQUEST_FILENAME} !-f
    RewriteRule ^ index.php [QSA,L]
&lt;/IfModule&gt;</pre>

Imagine for a sec that we could add some rules that would send to the Symfony app_*.php some request if they match your @Route regexp. For example, the routes related to the profiler:

<pre class="brush: shell; gutter: true">&lt;IfModule mod_rewrite.c&gt;
    Options -MultiViews

    RewriteEngine On
    
    # _profiler_home
    RewriteCond %{REQUEST_URI} ^/_profiler$
    RewriteRule ^ $0/ [QSA,L,R=301]
    RewriteCond %{REQUEST_URI} ^/_profiler/$
    RewriteRule ^ app_dev.php [QSA,L]

    # _profiler_search
    RewriteCond %{REQUEST_URI} ^/_profiler/search$
    RewriteRule ^ app_dev.php [QSA,L]
    # ... more rules

    # Here some of your custom rewrites...
    RewriteCond %{REQUEST_FILENAME} !-f
    RewriteRule ^ index.php [QSA,L]
&lt;/IfModule&gt;</pre>

With this approach is really easy to dispatch requests to one framework or the other, but, how can I generate these rules?

## Generating the Symfony routes

With the <a href="http://symfony.com/doc/current/cookbook/configuration/apache_router.html" target="_blank">Apache dumper feature of the Symfony Route Component</a>. This feature is <a href="https://github.com/symfony/symfony/issues/2432" target="_blank">deprecated</a> but you should be able to copy&paste the command and tune it for your goal if you need it.

If you don&#8217;t want to use this command, you can write your own, but it&#8217;s going to be a bit tricky.

There is no nginx dumper, but it shouldn&#8217;t be difficult to write one.

The existing command was thought to improve the performance of the Symfony apps running on Apache. Now, with PHP and Symfony improvements, it&#8217;s not so necessary this approach, but interesting for the migration one.

## Other details

We are using Hexagonal Architecture, sharing the same Symfony Container, so reusing the Business logic between frameworks is easy for us. Using this technique is almost mandatory in a step by step migration process. Each time we deploy, we generate the Apache file.

## Alternatives

I have seen people trying to integrate Symfony routing with its own routing and failing. It could work, but it&#8217;s quite work. With the approach explained here, it&#8217;s just a matter of your webserver. Another option, should be Symfony making curl requests to the old framework. The last one seen, is using both frameworks as a middleware of <a href="http://stackphp.com/" target="_blank">StackPHP</a>. We have tried the last one but no success.

## Show me some code

Just as an example. First of all the &#8220;copy&paste&#8221; of the router:dump of Symfony with some tweaks. You have the original version <a href="https://github.com/symfony/symfony/blob/2.7/src/Symfony/Bundle/FrameworkBundle/Command/RouterApacheDumperCommand.php" target="_blank">here</a>. It uses another dumper and customises the path to generate the configuration file.

<pre class="brush: php; gutter: true">namespace Atrapalo\Bundle\CoreBundle\Command;

use Atrapalo\Bundle\CoreBundle\Routing\Matcher\Dumper\DirectMatcherDumper;
use InvalidArgumentException;
use Symfony\Bundle\FrameworkBundle\Command\ContainerAwareCommand;
use Symfony\Component\Console\Input\InputArgument;
use Symfony\Component\Console\Input\InputOption;
use Symfony\Component\Console\Input\InputInterface;
use Symfony\Component\Console\Output\OutputInterface;
use Symfony\Component\Routing\RouterInterface;

class DumpMigratedRoutesCommand extends ContainerAwareCommand
{
    /**
     * {@inheritdoc}
     */
    public function isEnabled()
    {
        if (!$this-&gt;getContainer()-&gt;has(&#039;router&#039;)) {
            return false;
        }
        $router = $this-&gt;getContainer()-&gt;get(&#039;router&#039;);
        if (!$router instanceof RouterInterface) {
            return false;
        }

        return parent::isEnabled();
    }

    /**
     * {@inheritdoc}
     */
    protected function configure()
    {
        $this
            -&gt;setName(&#039;router:dump:migrated&#039;)
            -&gt;setDefinition([
                new InputArgument(
                    &#039;script-name&#039;,
                    InputArgument::OPTIONAL,
                    &#039;The script name of the application\&#039;s front controller.&#039;
                ),
                new InputArgument(
                    &#039;output-file&#039;,
                    InputArgument::OPTIONAL,
                    &#039;The path to the output file where the routes will be dumped.&#039;,
                    getcwd() . &#039;/resources/routes.conf&#039;
                ),
                new InputOption(&#039;base-uri&#039;, null, InputOption::VALUE_REQUIRED, &#039;The base URI&#039;),
            ])
            -&gt;setDescription(&#039;Dumps all migrated routes as Apache rewrite rules&#039;)
            -&gt;setHelp(
                &lt;&lt;&lt;EOF
The &lt;info&gt;%command.name%&lt;/info&gt; dumps all routes as Apache rewrite rules.

  &lt;info&gt;php %command.full_name%&lt;/info&gt;

EOF
            )
        ;
    }

    /**
     * {@inheritdoc}
     */
    protected function execute(InputInterface $input, OutputInterface $output)
    {
        $router = $this-&gt;getContainer()-&gt;get(&#039;router&#039;);

        $dumpOptions = [];

        if ($input-&gt;getArgument(&#039;script-name&#039;)) {
            $dumpOptions[&#039;script_name&#039;] = $input-&gt;getArgument(&#039;script-name&#039;);
        }

        if ($input-&gt;getOption(&#039;base-uri&#039;)) {
            $dumpOptions[&#039;base_uri&#039;] = $input-&gt;getOption(&#039;base-uri&#039;);
        }

        $outputFilePath = $input-&gt;getArgument(&#039;output-file&#039;);

        if (!is_writable(dirname($outputFilePath))) {
            throw new InvalidArgumentException(
                sprintf(&#039;The path "%s" is not writable!&#039;, dirname($outputFilePath))
            );
        }

        $dumper = new DirectMatcherDumper(
            $input-&gt;getParameterOption([&#039;--env&#039;, &#039;-e&#039;], &#039;dev&#039;),
            $router-&gt;getRouteCollection()
        );

        $output-&gt;writeln(
            sprintf(&#039;&lt;info&gt;Dumping routes to &lt;comment&gt;%s&lt;/comment&gt;&#039;, $outputFilePath)
        );

        file_put_contents(
            $outputFilePath,
            $dumper-&gt;dump($dumpOptions)
        );
    }
}
</pre>

And now the dumper that generates the configuration file with all the routes.

<pre class="brush: php; gutter: true">namespace Atrapalo\Bundle\CoreBundle\Routing\Matcher\Dumper;

use LogicException;
use Symfony\Component\Routing\CompiledRoute;
use Symfony\Component\Routing\Matcher\Dumper\MatcherDumper;
use Symfony\Component\Routing\Route;
use Symfony\Component\Routing\RouteCollection;

class DirectMatcherDumper extends MatcherDumper
{
    public function __construct($env, RouteCollection $routes)
    {
        $this-&gt;env = $env;

        parent::__construct($routes);
    }

    /**
     * Dumps a set of routes to a string representation of executable code
     * that can then be used to match a request against these routes.
     *
     * @param array $options An array of options
     *
     * @return string Executable code
     */
    public function dump(array $options = [])
    {
        if ($this-&gt;getRoutes()-&gt;count() == 0) {
            return &#039;&#039;;
        }

        $options = array_merge([
            &#039;script_name&#039; =&gt; &#039;&lt;your-default-path&gt;/app_dev.php&#039;,
            &#039;base_uri&#039;    =&gt; &#039;&#039;,
        ], $options);

        $options[&#039;script_name&#039;] = self::escape($options[&#039;script_name&#039;], &#039; &#039;, &#039;\\&#039;);

        $rules = [];
        $processedRoutes = [];

        foreach ($this-&gt;getRoutes()-&gt;all() as $name =&gt; $route) {
            if ($this-&gt;isAlreadyDumped($route, $processedRoutes)) {
                continue;
            }

            if ($route-&gt;hasOption(&#039;exclude-env&#039;)) {
                $excludedEnvironments = array_map(&#039;trim&#039;, explode(&#039;,&#039;, $route-&gt;getOption(&#039;exclude-env&#039;)));

                if (in_array($this-&gt;env, $excludedEnvironments)) {
                    continue;
                }
            }

            if ($route-&gt;getCondition()) {
                throw new LogicException(
                    sprintf(&#039;Unable to dump the routes for Apache as route "%s" has a condition.&#039;, $name)
                );
            }

            $rules[] = $this-&gt;dumpRoute($name, $route, $options, $processedRoutes);
            $processedRoutes[] = $route-&gt;getPath();
        }

        return implode("\n\n", $rules)."\n";
    }

    /**
     * Dumps a single route
     *
     * @param string $name    Route name
     * @param Route  $route   The route
     * @param array  $options Options
     *
     * @return string The compiled route
     */
    private function dumpRoute($name, $route, array $options, array &$processedRoutes)
    {
        $compiledRoute = $route-&gt;compile();

        // prepare the apache regex
        $regex = $this-&gt;routeRegexp($compiledRoute);
        $regex = &#039;^&#039; . self::escape(preg_quote($options[&#039;base_uri&#039;]) . substr($regex, 1), &#039; &#039;, &#039;\\&#039;);

        $hasTrailingSlash = &#039;/$&#039; === substr($regex, -2) && &#039;^/$&#039; !== $regex;

        $rule = ["# $name"];

        $hostRegex = $compiledRoute-&gt;getHostRegex();

        // redirect with trailing slash appended
        if ($hasTrailingSlash) {

            if (null !== $hostRegex) {
                $rule[] = $this-&gt;buildHostRewriteCond($hostRegex);
            }

            $rule[] = &#039;RewriteCond %{REQUEST_URI} &#039;.substr($regex, 0, -2).&#039;$&#039;;
            $rule[] = &#039;RewriteRule .* $0/ [QSA,L,R=301]&#039;;
        }

        if (null !== $hostRegex) {
            $rule[] = $this-&gt;buildHostRewriteCond($hostRegex);
        }

        // the main rule
        $rule[] = "RewriteCond %{REQUEST_URI} $regex";
        $rule[] = "RewriteRule .* {$options[&#039;script_name&#039;]} [QSA,L]";

        $processedRoutes[$hostRegex][] = $regex;

        return implode("\n", $rule);
    }

    /**
     * Converts a regex to make it suitable for mod_rewrite
     *
     * @param string $regex The regex
     *
     * @return string The converted regex
     */
    private function regexToApacheRegex($regex)
    {
        $regexPatternEnd = strrpos($regex, $regex[0]);

        return preg_replace(&#039;/\?P&lt;.+?&gt;/&#039;, &#039;&#039;, substr($regex, 1, $regexPatternEnd - 1));
    }

    /**
     * Escapes a string.
     *
     * @param string $string The string to be escaped
     * @param string $char   The character to be escaped
     * @param string $with   The character to be used for escaping
     *
     * @return string The escaped string
     */
    private static function escape($string, $char, $with)
    {
        $escaped = false;
        $output = &#039;&#039;;
        foreach (str_split($string) as $symbol) {
            if ($escaped) {
                $output .= $symbol;
                $escaped = false;
                continue;
            }
            if ($symbol === $char) {
                $output .= $with.$char;
                continue;
            }
            if ($symbol === $with) {
                $escaped = true;
            }
            $output .= $symbol;
        }

        return $output;
    }

    private function buildHostRewriteCond($hostRegex)
    {
        $apacheHostRegex = $this-&gt;regexToApacheRegex($hostRegex);
        $apacheHostRegex = self::escape($apacheHostRegex, &#039; &#039;, &#039;\\&#039;);

        return sprintf(&#039;RewriteCond %%{HTTP_HOST} %s&#039;, $apacheHostRegex);
    }

    public function isAlreadyDumped(Route $route, array &$processedPaths)
    {
        $compiledRoute = $route-&gt;compile();

        $hostRegexp = $compiledRoute-&gt;getHostRegex();

        if (!isset($processedPaths[$hostRegexp])) {
            $processedPaths[$hostRegexp] = [];

            return false;
        }

        return in_array($this-&gt;routeRegexp($compiledRoute), $processedPaths[$hostRegexp], true);
    }

    private function routeRegexp(CompiledRoute $compiledRoute)
    {
        return $this-&gt;regexToApacheRegex($compiledRoute-&gt;getRegex());
    }
}</pre>