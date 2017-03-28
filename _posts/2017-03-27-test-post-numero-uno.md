---
layout: post
title: Pass by behavior, not value
---


<div class="message">
“Programs must be written for people to read, 
and only incidentally for machines to execute.” <br/>

~  Harold Abelson, Structure and Interpretation of Computer Programs
</div>

Java 8 introduced <strong>Lambdas</strong>. It was a significant departure in terms of code writing style, a departure form the imperative style or programming, and code reading that many of us were used to. Because one thing that is certain about lambdas is that they do not read well.

```java
system.out.println(Utilities::abbr);
```

However, once you start to peel the layers off, the true beauty of <strong>lambdas</strong> start to unfold. 

## Behavior, not value

Imagine a high-speed communication hub, where messages arrive on a global buffer from various other actors in the system. The messages are of various lengths, various formats and headers.

```java

/* Store a message in long term archive */
public void longTermArchive(Message msg) {

    /* Implementation pseudocode */
    if (archiveFlag) {
        writeToDisk(msg);
    }
}

```
Observe that the actual writing to disk happens only when the <code>archive</code> flag is set to <strong>true</strong>.
Now assume that the <code> Message </code> object being passed is created by another function <code>createMessageFromBuffer</code>,
which is - <em>and here is the where things get interesting</em> is <strong>expensive</strong>. 

It takes a long time to execute. 


```java

/*createMessageFromGlobalBuffer() is a time-intensive function */
longTermArchive(createMessageFromGlobalBuffer());

```

Remember, Java passes arguments by value and so the <code>createMessageFromGlobalBuffer()</code> function will be first executed, the value put on the stack and then copied over to the <code>longTermArchive</code> function. This is regardless of the state of the <code> archiveFlag</code> ; to which our message creating function may not even have visibility.

The problem is obvious. We are spending time and resources executing a function and passing the value into another function, when it may not even have been needed.

Instead, if we could just pass the behaviour, that is a reference to the function to be called ! Ah, now that is what a <em>pointer-to-a-function</em> was in the good old C/C++ days. Chances are, it would look like this

```java

longTermArchive(reference_to_the_expensive_message_creating_function);
// Now, we know that will not compile. So, how about:

longTermArchive(&createMessageFromGlobalBuffer);
// well, this is how it woudl be in C, passing the addr of a fn


longTermArchive(() -> createMessageFromGlobalBuffer());
// In Java .... and I never said it would look elegant

```






-----

Want to see something else added? <a href="https://github.com/poole/poole/issues/new">Open an issue.</a>
