---
layout: post
title: "Javascript Memory Management"
description: "Quick peek in Garbage Collection in Javascript and some common sources of memory leaks"
tags: [javascript, performance tuning, web development]
keywords: [javascript, performance tuning, web development]
---

As the web evolved, the static pages turned into Ajax driven single-page applications. These SPA's enabled web application to have better performance and re-usable code. Memory management is quite important when writing these applications (Eg: Angular JS, Backbone, Ember) as they never get refreshed. This means that memory leaks can become apparent quite quickly, specially when using them on mobile devices which have limited memory.

## How is the memory managed in Javascript ?

Like many other advanced programming languages, javascript also has a Garbage Collectors. There are 2 main algorithms which are used by the browsers:

1. Reference Counting
2. Mark Sweep Collection

### Reference Counting

When an object is created, Javascript automatically allocates an appropriate amount of memory for that object. The object is continuously evaluated by the garbage collector to see if is still used. The garbage collector runs at regular interval, it sweeps through the object graph and counts the number of other objects that references each object. Later the objects with reference count 0 are considered as not unused and object's memory can be reclaimed.

### Mark and Sweep Collection

This is a more effective algorithm that is used by most if the modern browsers. In this method, when a variable comes into context, such as a variable is declared inside a function it is marked as in context. When it goes out of context it is marked as out of context. This is the Mark step.

Later the garbage collector identifies all the variables which are not in context or not referenced by variables in context. These variables are considered as ready for deletion as they can't be reached by any in-context variables. When the collector does the memory sweep, it destroys all the objects which are marked ready for deletion.

This is one of the main reasons why we should avoid global variables because global variables are always in context and they are never destroyed by the garbage collector and hence globals are only cleaned up then you refresh the page.

## Important facts about Memory Leak and Javascript Performance

### De-Referencing

Mank developers think that they can force a GC and reclaim memory by using the delete keyword. This is not the case. Let us see what happens when we use the delete keyword in the below example:

<pre>
<code class="language-javascript">
var object = {
	x: 1,
	y: 2
};

delete o.x;//true
o.x; //undefined
</code>
</pre>

In this case this code, the delete key word changes the structure of the object which in-turn changes the hidden class and makes it a generic slow object. This makes the access to the other properties of the object slow. The better solution would be setting the value to null. Setting the object reference to null does not mean "null" the object. It sets the object reference to null.

<pre>
	<code class="language-javascript">
o.x = null;
	</code>
</pre>

### Functions and Garbage Collection

Consider the following code example:

<pre>
<code class="language-javascript">
function foo() {
	var bar = new SomeHugeObject();

	bar.someMethod();
}
</code>
</pre>

When foo returns, the object bar no longer remains in the context and hence is eligible for garbage collection. But if we have the following case:

<pre>
<code class="language-javascript">
function foo() {
	var bar = new SomeHugeObject();

	bar.someMethod();
	return bar;
}

//Calling function
var a = foo();

</code>
</pre>

In this case, we now have a reference to the object which is in the context. Hence this object is not eligible for garbage collection till the reference a points to some other object or a goes out of scope.

### Memory leak due to closure

A closure is created when there is a function and it returns an inner function which uses variables from the outer function.
For example:

<pre>
<code class="language-javascript">
function outer () {
	var a = "Long String";

	function inner (newString) {
		a = a + newString;
	}

	return inner;
}

var fun1 = outer();
</code>
</pre>

In this case the variable 'a' will remain in memory even after the execution of outer is complete because of a closure created. This will remain in memory till fun1 is not assigned some other object or it is in scope.



