---
title: PHP Trait testing
author: Carlos Buenosvinos
type: post
date: 2013-11-03T14:23:19+00:00
url: /php-trait-testing/
dsq_thread_id:
  - "4655192262"
categories:
  - PHP
  - 'TDD &amp; Katas'
tags:
  - tdd
  - testing
  - trait

---
<span style="color: #888888;">I&#8217;d like to know how you test your traits and what&#8217;s your favourite way to do it. I have found two different approaches that I want to share with you. Please comment if you apply different techniques or have different opinions.</span>

<!--more-->

Imagine a Salesman entity that we want to change it&#8217;s name, the name is not allowed to be 255 longer. If so, we&#8217;ll throw an exception. We have decided to implemented with a trait (we&#8217;ll see why and what pattern is related to in another post), ValidationTrait. When want to unit test it, of course. Here is the trait:

<pre class="brush: php; gutter: true">&lt;?php

namespace Domain\Model\Validation;

/**
 * Trait ValidationTrait
 * @package Domain\Model\Validation
 */
trait ValidationTrait
{
    /**
     * @param  string                 $value
     * @param  int                    $maxLength
     * @throws StringTooLongException
     */
    private function checkMaxLength($value, $maxLength)
    {
        if (strlen($value) &gt; $maxLength) {
            throw new StringTooLongException();
        }
    }
}</pre>

The two different approaches are:

  1. Direct &#8220;use&#8221; in TestCase
  2. Dummy class &#8220;using&#8221; Trait

## Direct &#8220;use&#8221; in TestCase

The first approach is using the Trait in the TestCase:

<pre class="brush: php; gutter: true">&lt;?php
namespace Domain\Model\Validation;

use Domain\Model\Validation\StringTooLongException;

class ValidationTraitTest extends \PHPUnit_Framework_TestCase
{
    use ValidationTrait;

    /**
     * @test
     * @dataProvider validStringsLengthLimitsDataProvider
     */
    public function validStringsLengthLimits($string, $maxlength)
    {
        $result = $this-&gt;checkMaxLength($string, $maxlength);
        $this-&gt;assertNull($result);
    }

    public function validStringsLengthLimitsDataProvider()
    {
        return [
            [null, 0],
            [&#039;&#039;, 0],
            [&#039;#&#039;, 1],
            [&#039;#&#039;, 2],
        ];
    }

    /**
     * @test
     * @dataProvider invalidStringsLengthLimitsDataProvider
     * @expectedException StringTooLongException
     */
    public function invalidStringsLengthLimits($string, $maxlength)
    {
        $this-&gt;checkMaxLength($string, $maxlength);
    }

    public function invalidStringsLengthLimitsDataProvider()
    {
        return [
            [null, -1],
            [&#039;&#039;, -1],
            [&#039;#&#039;, 0],
            [&#039;##&#039;, 1],
        ];
    }
}</pre>

Easy, elegant and no more elements needed. The only problem I see here, it&#8217;s a matter of naming collision with the methods in the tests and the Trait that could be resolved with the trait aliasing capabilities.

## Dummy class &#8220;using&#8221; Trait

The second approach means creating a dummy class that will use the trait and then unit test the dummy class. Let&#8217;s check a possible solution:

<pre class="brush: php; gutter: true">&lt;?php
namespace Domain\Model\Validation;

use Domain\Model\Validation\StringTooLongException;

/**
 * Class ValidationTraitDummyClass
 * @package Domain\Model\Validation
 */
class ValidationTraitDummyClass
{
    use ValidationTrait;
}

class ValidationTraitTest extends \PHPUnit_Framework_TestCase
{
    /**
     * @var ValidationTraitDummyClass
     */
    private $dummy;

    protected function setUp()
    {
        $this-&gt;dummy = new ValidationTraitDummyClass();
    }

    /**
     * @test
     * @dataProvider validStringsLengthLimitsDataProvider
     */
    public function testValidStringsLengthLimits($string, $maxlength)
    {
        $result = $this-&gt;dummy-&gt;checkMaxLength($string, $maxlength);
        $this-&gt;assertNull($result);
    }

    public function validStringsLengthLimitsDataProvider()
    {
        return [
            [null, 0],
            [&#039;&#039;, 0],
            [&#039;#&#039;, 1],
            [&#039;#&#039;, 2],
        ];
    }

    /**
     * @test
     * @dataProvider invalidStringsLengthLimitsDataProvider
     * @expectedException StringTooLongException
     */
    public function testInvalidStringsLengthLimits($string, $maxlength)
    {
        $this-&gt;dummy-&gt;checkMaxLength($string, $maxlength);
    }

    public function invalidStringsLengthLimitsDataProvider()
    {
        return [
            [null, -1],
            [&#039;&#039;, -1],
            [&#039;#&#039;, 0],
            [&#039;##&#039;, 1],
        ];
    }
}</pre>

What&#8217;s the problem here? **Our trait has a private method**, so we have to tune a bit our example. Remark that with a public method, our example would be ready. So, let&#8217;s try to update our dummy class to make the work:

<pre class="brush: php; gutter: true">/**
 * Class ValidationTraitDummyClass
 * @package Domain\Model\Validation
 */
class ValidationTraitDummyClass
{
    use ValidationTrait;

    public function checkMaxLength($string, $maxlength)
    {
        $this-&gt;checkMaxLength($string, $maxlength);
    }
}</pre>

Ok, that do the trick, but it&#8217;s not as elegant as the first approache, isn&#8217;t it?

## Conclusion

Based on the different details and cases, in my opinion, the first approach, Direct &#8220;use&#8221; in the test case is better than the other one. It has no drawbacks and it&#8217;s more elegant for me. Thoughts?