---
title: 'Some “basuritas”: PHP, list() and objects'
author: Carlos Buenosvinos
type: post
date: 2016-07-29T11:27:28+00:00
url: /some-basuritas-php-list-and-objects/
dsq_thread_id:
  - "5023778780"
categories:
  - PHP
tags:
  - list()
  - objects
  - php
  - phpunit

---
I was just playing around with the <a href="http://symfony.com/doc/current/components/expression_language.html" target="_blank">Symfony Expression Language Component</a>. Suddenly, I started to play with <a href="http://php.net/manual/en/function.list.php" target="_blank"><em>list()</em></a> and some objects and this example came up. You can use **_list()_** and _**$this**_ for assigning fields. I would not recommend it, but it just funny.

<pre class="brush: php; gutter: true">class BasuritasTest extends \PHPUnit_Framework_TestCase
{
    /**
     * @test
     * @dataProvider validUsersDataProvider
     * @param $id
     * @param $name
     */
    public function listForObjects(int $id, string $name)
    {
        $user = new User($id, $name); 
        $this-&gt;assertSame($id, $user-&gt;id());
        $this-&gt;assertSame($name, $user-&gt;name());
    }

    public function validUsersDataProvider()
    {
        return [
            [1, "Carlos"],
            [2, "Christian"],
            [3, "Keyvan"],
            [4, "Ricard"]
        ];
    }
}

class User
{
    private $id;
    private $name;

    public function __construct($id, $name)
    {
        list($this-&gt;id, $this-&gt;name) = [$id, $name];
    }

    public function id() : int 
    {
        return $this-&gt;id;
    }

    public function name() : string
    {
        return $this-&gt;name;
    }
}</pre>

The interesting thing is the line 34. The output:

<pre class="brush: bash; gutter: false">PHPUnit 5.4.8 by Sebastian Bergmann and contributors.

....                                          4 / 4 (100%)

Time: 56 ms, Memory: 4.00MB

OK (4 tests, 8 assertions)</pre>

That&#8217;s it. Happy Friday.