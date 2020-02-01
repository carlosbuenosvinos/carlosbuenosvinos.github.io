---
title: Good ORMs do not call __construct a.k.a. where to fire my Domain Events
author: Carlos Buenosvinos
type: post
date: 2016-11-24T08:00:31+00:00
url: /good-orms-do-not-call-__construct/
featured_image: /posts/images/2016/11/past.jpg
dsq_thread_id:
  - "5328441684"
categories:
  - Databases
  - DDD
  - PHP
tags:
  - constructor
  - domain events
  - domain-driven design
  - named constructor

---
**tl;dr: Entity constructor should be called once in its entire life. That&#8217;s why good ORMs don&#8217;t use constructor to reconstitute objects from the database. Good ORMs use reflection, deserialization or proxies. So, fire your Domain Events in the constructor.**

Last week, I saw a presentation about going from <a href="http://codely.tv/screencasts/codigo-acoplado-framework-microservicios-ddd/" target="_blank">coupled-to-framework code to a Domain-Driven Design approach</a> in a sample application. It was a good talk. However, in my honest opinion, there were some mistakes that are important to address. In order not to troll, I have checked with the speakers to write this post ;).

One of the mistakes, that I would like to point is _where a Domain Event about &#8220;a new product was created&#8221;_ should be trigger specifically inside the Product Entity.

Speakers were showing a &#8220;Video&#8221; Entity with a named constructor called &#8220;create&#8221;. They were calling _new _inside the named constructor **and then** firing the event. I mean, the Domain Event was not fired inside of the real constructor. Let&#8217;s see the code we&#8217;re talking about (the whole repository can be found <a href="https://github.com/CodelyTV/cqrs-ddd-example" target="_blank">here</a>):<!--more-->

<pre class="brush: php; gutter: true">final class Video extends AggregateRoot
{
    private $id;
    private $title;
    private $url;
    private $courseId;

    public function __construct(VideoId $id, VideoTitle $title, VideoUrl $url, CourseId $courseId)
    {
        $this-&gt;id       = $id;
        $this-&gt;title    = $title;
        $this-&gt;url      = $url;
        $this-&gt;courseId = $courseId;
    }

    public static function create(VideoId $id, VideoTitle $title, VideoUrl $url, CourseId $courseId)
    {
        $video = new self($id, $title, $url, $courseId);

        $video-&gt;raise(
            new VideoCreatedDomainEvent(
                $id,
                [
                    &#039;title&#039;    =&gt; $title-&gt;value(),
                    &#039;url&#039;      =&gt; $url-&gt;value(),
                    &#039;courseId&#039; =&gt; $courseId-&gt;value(),
                ]
            )
        );

        return $video;
    }

    // ...
}</pre>

The reason about this approach was because when dealing with databases, _new_ can be called multiple times when fetching back the object. I think here&#8217;s is the misunderstanding. :)

Consider the following version that fires the event inside the __constructor:

<pre class="brush: php; gutter: true">final class Video extends AggregateRoot
{
    private $id;
    private $title;
    private $url;
    private $courseId;

    public function __construct(VideoId $id, VideoTitle $title, VideoUrl $url, CourseId $courseId)
    {
        $this-&gt;id       = $id;
        $this-&gt;title    = $title;
        $this-&gt;url      = $url;
        $this-&gt;courseId = $courseId;

        $this-&gt;raise(
            new VideoCreatedDomainEvent(
                $id,
                [
                    &#039;title&#039; =&gt; $title-&gt;value(),
                    &#039;url&#039; =&gt; $url-&gt;value(),
                    &#039;courseId&#039; =&gt; $courseId-&gt;value(),
                ]
            )
        );
    }

    public static function create(VideoId $id, VideoTitle $title, VideoUrl $url, CourseId $courseId)
    {
        return new self($id, $title, $url, $courseId);
    }

//...</pre>

In terms of DDD, the second approach is better. The Domain Event is fired when the Entity is created. But if you consider the first approach, a part from the mistake of the constructor being _public_ (it&#8217;s a good practice to make it private with named constructors), the Domain Event is fired **right after** the Entity is created.

However, let&#8217;s argue about the possible problem with the last approach with ORMs. They were defending that the first approach is better because when you fetch an instance from the Database it doesn&#8217;t call the constructor so the Domain Event is not fired again. That&#8217;s not right.

Let&#8217;s see some Doctrine code.

**Doctrine depends on &#8220;doctrine/instatiator&#8221; package on composer.json**

<pre class="brush: actionscript3; gutter: true">...
"doctrine/instantiator": "~1.0.1",
"doctrine/common": "&gt;=2.5-dev,&lt;2.7-dev",
"doctrine/cache": "~1.4",
...</pre>

**Doctrine uses Instantiator to build new instances without calling _new_. Check it on <a href="https://github.com/doctrine/doctrine2/blob/master/lib/Doctrine/ORM/Mapping/ClassMetadataInfo.php#L906" target="_blank">github</a>.**

<pre class="brush: php; gutter: true">/**
     * Creates a new instance of the mapped class, without invoking the constructor.
     *
     * @return object
     */
    public function newInstance()
    {
        return $this-&gt;instantiator-&gt;instantiate($this-&gt;name);
    }</pre>

This was not the behaviour with Doctrine 1. But it is with Doctrine 2. In fact, six years ago, in 2010, the internal implementation was not using the Instantiator object, but a PHP internal trick. Let&#8217;s see it.

<pre class="brush: php; gutter: true">public function newInstance()
{
    if ($this-&gt;_prototype === null) {
        $this-&gt;_prototype = unserialize(sprintf(&#039;O:%d:"%s":0:{}&#039;, strlen($this-&gt;name), $this-&gt;name));
    }
    return clone $this-&gt;_prototype;
}</pre>

In my personal experience, in the <a href="https://github.com/dddinphp/last-wishes/blob/master/src/Lw/Domain/Model/User/User.php#L62" target="_blank">LastWishes</a> project, I sign up a user once and then sign in multiple times. The Domain Event is fired in the constructor and there is only one creation event fired ever.

> How many times _**new**_ should be called in the life of an Entity?

Beyond this specific topic, I would like to point another even more general idea around this topic. How many times **new** should be called in the life of an Entity? One. Correct! It doesn&#8217;t matter how this Entity is persisted and retrieved, an Entity is created once. Then it remains alive persisted in memory or in a database. No-one should invoke **new** again.

Hope it helps.