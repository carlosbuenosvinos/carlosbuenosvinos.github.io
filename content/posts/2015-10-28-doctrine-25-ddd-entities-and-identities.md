---
title: Doctrine 2.5, DDD, Entities and Identities
author: Carlos Buenosvinos
type: post
date: 2015-10-28T08:00:22+00:00
url: /doctrine-25-ddd-entities-and-identities/
dsq_thread_id:
  - "4654039315"
categories:
  - Databases
  - DDD
  - PHP
tags:
  - custom types
  - ddd
  - doctrine
  - domain-driven design
  - embeddables
  - entities
  - id

---
If you&#8217;re developing applications in a Domain-Driven Design style and using Doctrine 2.5, you may be struggling with implementing your entities identities, I mean, UserId, ProductId, and so on. Embeddables are cool, but limited. Surrogates are not necessary anymore. Here are some short tips and examples about how to proceed.

<!--more-->

### tl;dr

If using Doctrine 2.5, <a href="http://doctrine-orm.readthedocs.org/projects/doctrine-orm/en/latest/tutorials/embeddables.html" target="_blank">embeddables</a> are not an option yet for implementing your identities. The best option is using <a href="http://doctrine-orm.readthedocs.org/projects/doctrine-orm/en/latest/cookbook/custom-mapping-types.html" target="_blank">Custom Types</a>.

### What&#8217;s new in Doctrine 2.5

An interesting new feature about Doctrine 2.5, is that now, it&#8217;s possible to use <a href="http://doctrine-orm.readthedocs.org/projects/doctrine-orm/en/latest/changelog/migration_2_5.html#support-for-objects-as-identifiers" target="_blank">Objects as identifiers for Entities</a> as long as they implement the magic method <code class="docutils literal">&lt;span class="pre">__toString()&lt;/span></code>. So we can add __toString to our Identities Value Objects and use them in our mappings. Let&#8217;s see.

### Introduction

Consider the context of <a href="https://github.com/dddinphp/last-wishes" target="_blank">Last Wishes</a>. To sum up, users that have wishes. Users are identified by a UserId, a value object that holds a UUID. Wishes are identified by a WishId and have a reference to its owner, a UserId.

### Our Identity VO

First of all, let&#8217;s see our current identity objects. Remember, we need to add the __toString() method. First, UserId.

<pre class="brush: php; gutter: true">namespace Lw\Domain\Model\User;

use Rhumsaa\Uuid\Uuid;

/**
 * Class UserId.
 */
class UserId
{
    /**
     * @var string
     */
    private $id;

    /**
     * @param string $id
     */
    public function __construct($id = null)
    {
        $this-&gt;id = null === $id ? Uuid::uuid4()-&gt;toString() : $id;
    }

    /**
     * @return string
     */
    public function id()
    {
        return $this-&gt;id;
    }

    /**
     * @param UserId $userId
     *
     * @return bool
     */
    public function equals(UserId $userId)
    {
        return $this-&gt;id() === $userId-&gt;id();
    }

    /**
     * @return string
     */
    public function __toString()
    {
        return $this-&gt;id();
    }
}</pre>

Now, WishId.

<pre class="brush: php; gutter: true">namespace Lw\Domain\Model\Wish;

use Rhumsaa\Uuid\Uuid;

/**
 * Class WishId.
 */
class WishId
{
    /**
     * @var string
     */
    private $id;

    /**
     * @param string $id
     */
    public function __construct($id = null)
    {
        $this-&gt;id = $id ?: Uuid::uuid4()-&gt;toString();
    }

    /**
     * @return string
     */
    public function id()
    {
        return $this-&gt;id;
    }

    /**
     * @param WishId $wishId
     *
     * @return bool
     */
    public function equals(WishId $wishId)
    {
        return $this-&gt;id() === $wishId-&gt;id();
    }

    /**
     * @return string
     */
    public function __toString()
    {
        return $this-&gt;id();
    }
}</pre>

### Custom Types

Check the implementation of the Doctrine Custom Types. They inherit from GuidType, so their internal representation will be an UUID. We need to specify what&#8217;s the database back and forward conversion. Last, we need to register our custom types before use them.

First, a base type for the UserId and WishId custom types.

<pre class="brush: php; gutter: true">namespace Lw\Infrastructure\Domain\Model;

use Doctrine\DBAL\Platforms\AbstractPlatform;
use Doctrine\DBAL\Types\GuidType;

class DoctrineEntityId extends GuidType
{
    public function convertToDatabaseValue($value, AbstractPlatform $platform)
    {
        return $value-&gt;id();
    }

    public function convertToPHPValue($value, AbstractPlatform $platform)
    {
        $className = $this-&gt;getNamespace().&#039;\\&#039;.$this-&gt;getName();

        return new $className($value);
    }
}</pre>

Now, the UserId custom type.

<pre class="brush: php; gutter: true">namespace Lw\Infrastructure\Domain\Model\User;

use Lw\Infrastructure\Domain\Model\DoctrineEntityId;

class DoctrineUserId extends DoctrineEntityId
{
    public function getName()
    {
        return &#039;UserId&#039;;
    }

    protected function getNamespace()
    {
        return &#039;Lw\Domain\Model\User&#039;;
    }
}</pre>

Time for the WishId custom type.

<pre class="brush: php; gutter: true">namespace Lw\Infrastructure\Domain\Model\Wish;

use Lw\Infrastructure\Domain\Model\DoctrineEntityId;

class DoctrineWishId extends DoctrineEntityId
{
    public function getName()
    {
        return &#039;WishId&#039;;
    }

    protected function getNamespace()
    {
        return &#039;Lw\Domain\Model\Wish&#039;;
    }
}</pre>

Last, custom types registration.

<pre class="brush: php; gutter: true">namespace Lw\Infrastructure\Persistence\Doctrine;

use Doctrine\ORM\EntityManager;
use Doctrine\ORM\Tools\Setup;

class EntityManagerFactory
{
    /**
     * @return EntityManager
     */
    public function build()
    {
        \Doctrine\DBAL\Types\Type::addType(&#039;UserId&#039;, &#039;Lw\Infrastructure\Domain\Model\User\DoctrineUserId&#039;);
        \Doctrine\DBAL\Types\Type::addType(&#039;WishId&#039;, &#039;Lw\Infrastructure\Domain\Model\Wish\DoctrineWishId&#039;);

        return EntityManager::create(
            array(
                &#039;driver&#039; =&gt; &#039;pdo_sqlite&#039;,
                &#039;path&#039; =&gt; __DIR__.&#039;/../../../../../db.sqlite&#039;,
            ),
            Setup::createYAMLMetadataConfiguration([__DIR__.&#039;/config&#039;], true)
        );
    }
}
</pre>

### Mappings

Check now Doctrine mappings. See how we&#8217;re using our custom types in the &#8220;type&#8221; field.

<pre class="brush: php; gutter: true">Lw\Domain\Model\User\User:
  type: entity
  id:
    userId:
      column: id
      type: UserId
  table: lw_user
  repositoryClass: Lw\Infrastructure\Domain\Model\User\DoctrineUserRepository
  fields:
    email:
      type: string
    password:
      type: string

Lw\Domain\Model\Wish\Wish:
  type: entity
  table: lw_wish
  repositoryClass: Lw\Infrastructure\Domain\Model\Wish\DoctrineWishRepository
  id:
    wishId:
      column: id
      type: WishId
  fields:
    address:
      type: string
    content:
      type: text
    userId:
      type: UserId
      column: user_id
</pre>

Hope it helps to all the DDD lovers!