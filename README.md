Download Link: https://assignmentchef.com/product/solved-cs2030s-problem-set-11-asynchronous-programming
<br>



This problem set aims to let students explore the different methods provided by the CompletableFuture class in Java 8 or later.

<ol>

 <li>Study the given class A below, which uses the methods incr and decr to imitate slow computations.</li>

</ol>

class A { private final int x;

A() {

this(0);

}

A(int x) { this.x = x;

}

void sleep() {

System.out.println(Thread.currentThread().getName() + ” ” + x); try {

Thread.sleep(1000);

} catch (InterruptedException e) {

System.out.println(“interrupted”);

}

}

A incr() {

sleep(); return new A(this.x + 1);

}

A decr() {

sleep();

if (x &lt; 0) {

throw new IllegalStateException();

}

return new A(this.x – 1);

}

public String toString() { return “” + x;

}

}

(a) Suppose we have a method

A foo(A a) {

return a.incr().decr();

}

Convert the method foo above to a method that returns a CompletableFuture so that the body of the method is executed asynchronously. Try different variations by using

<ol>

 <li>supplyAsync only;</li>

 <li>supplyAsync and thenApply;</li>

</ol>

<ul>

 <li>supplyAsync and thenApplyAsync</li>

</ul>

Demonstrate how you would retrieve the result of the computation.

See also: thenRun, thenAccept, runAsync (b) Suppose now we have another method

A bar(A a) {

return a.incr();

}

which we would like to invoke using bar(foo(new A())). Convert the computation within bar to run asynchronously as well. bar should now return a CompletableFuture. In addition, show the equivalent of calling bar(foo(new A())) in an asynchronous fashion, using the method thenCompose. See also: thenCombine

<ul>

 <li>Suppose now we have yet another method</li>

</ul>

A baz(A a, int x) { if (x == 0) {

return new A(0);

} else {

return a.incr().decr();

}

}

Convert the computation within baz in the else clause to run asynchronously. baz should now return a CompletableFuture. You may find the method completedFuture useful.

<ul>

 <li>Let’s now call foo, bar, baz We would like to output the string “done!” when <em>all </em>three method calls complete. Show how you can use the allOf() method to achieve this behavior.</li>

</ul>

See also: anyOf, runAfterBoth, runAfterEither.

<ul>

 <li>Calling new A().decr().decr() would cause an exception to be thrown, even when it is done asynchronously. Show how you would use the handle() method to gracefully handle exceptions thrown (such as printing them out) within a chain of CompletableFuture</li>

</ul>

See also: whenComplete and exceptionally

<ol start="2">

 <li>Modify the following sequences of code such that f, g, h and i are now invoked asynchronously, via CompletableFuture. Assume that a has been initialized as</li>

</ol>

A a = new A();

<ul>

 <li>B b = f(a); C c = g(b);

  <ul>

   <li>d = h(c);</li>

  </ul></li>

 <li>B b = f(a); C c = g(b); h(c); // no return value</li>

 <li>B b = f(a); C c = g(b);

  <ul>

   <li>d = h(b);</li>

   <li>e = i(c, d);</li>

  </ul></li>

</ul>