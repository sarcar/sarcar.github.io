---
layout: post
title: This is test post in markdown
---


<div class="message">
“Programs must be written for people to read, 
and only incidentally for machines to execute.” 

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
Observe that the actual writing to disk happens only if the <archiveFlag> is <strong>true</strong>.


```java
archiver.longTermArchive(createMessageFromGlobalBuffer());
```
However, the function <code> longTermArchive</code> is internally activated only when the <code>archive</code>flag is set to <code>true</code>.

Remember Java passes arguments by value and so the <code>createMEssageFromGlobalBuffer</code> will be first executed, regardless of whether finally we archive to long-term storage. If the function <code>createMessageFromGlobalBuffer</code> is expensive, then we are in for a performance hit.






-----

Want to see something else added? <a href="https://github.com/poole/poole/issues/new">Open an issue.</a>
