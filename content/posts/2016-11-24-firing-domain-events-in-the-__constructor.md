---
title: Firing Domain Events in the __constructor
author: Carlos Buenosvinos
type: post
date: 2016-11-24T13:24:02+00:00
url: /firing-domain-events-in-the-__constructor/
featured_image: /posts/images/2016/11/past.jpg
dsq_thread_id:
  - "5329046440"
categories:
  - DDD
  - PHP
tags:
  - constructor
  - doctrine
  - named constructor
  - new
  - php
  - repository

---
After my post about <a href="https://carlosbuenosvinos.com/good-orms-do-not-call-__construct/" target="_blank">&#8220;Good ORMs do not call __construct a.k.a. where to fire my Domain Events&#8221;</a>, nice discussions (and trolling) are happening in the Barcelona Engineering Slack Channel. **Thanks to everyone for the feedback, ideas and concerns. You are more than welcome!** The main concerns are:

  1. How do I fetch an Entity from ElasticSearch, using new and without firing the Event?
  2. How do I create stubs on the Entity that is firing the Domain Event?
  3. What are pros and cons of firing the Domain Event in the __constructor or in the named constructor?

So, I&#8217;ll take the original code as a starting point, and I&#8217;ll suggest different strategies to answer those questions. <!--more-->

If you fetch an Entity directly hitting an ElasticSearch server and building such Entity from a JSON document, there is a (simple) option:

**Extract Method + Sub Class + Noop**

<pre class="brush: php; gutter: true">class Video extends AggregateRoot
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

        $this-&gt;raiseEvent($this);
    }

    protected function raiseEvent($video)
    {
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

//...

class ElasticVideo extends Video
{
    protected function raiseEvent($video)
    {
        // noop
    }
}</pre>

Now, your ElasticRepository does fetch ElasticVideos, using new, without firing the Domain Event. Everything is not fantastic, here, we need to remove the _final_ keyword from the class declaration because we need to extend it to do the trick. If you consider this approach, it is more or less what Doctrine is doing when you generate Doctrine Proxies. When you get a Entity from Doctrine ask for its classname ;) The trick, also works for testing. Another alternative is using Reflection.

**&#8220;What are pros and cons of firing the Domain Event in the __constructor or in the named constructor?&#8221;**

To clarify, I&#8217;m talking about a subtle detail. The code from the speakers is good enough to use it in any project you want.

In my honest opinion, I prefer to fire Domain Events as close as possible of the fact that fired it. That&#8217;s why I prefer to fire it in the constructor. Specially if the constructor is public. I don&#8217;t want to have two ways of building an Entity (named and default constructor) and one if firing the event and the other not. It sounds strange to me.

If you make the default constructor private, I&#8217;m happy. However, firing the event is a piece of code that is going to be duplicated in all the named constructors, so I prefer to removed such duplication and put it into the default constructor.

As a comparison, I don&#8217;t like to fire Domain Events in the Application Services. I prefer to fire them in the Entities. Firing in Application Services or Domain Services when you can fire them in the Entities can cause some issues. Some client code could perform the same operation in the Entity without firing the Event. For me, this different behaviour sounds again strange.

For me, the cons are not using _final_ and having to change the visibility of some methods.

Hope it helps.