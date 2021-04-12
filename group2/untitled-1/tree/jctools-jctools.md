# JCTools/JCTools

[![Total alerts](https://camo.githubusercontent.com/9474678877409097d26b5c1647abbd911fec808804b3c8b606dd4edb9c3469e6/68747470733a2f2f696d672e736869656c64732e696f2f6c67746d2f616c657274732f672f4a43546f6f6c732f4a43546f6f6c732e7376673f6c6f676f3d6c67746d266c6f676f57696474683d3138)](https://lgtm.com/projects/g/JCTools/JCTools/alerts/) [![Coverage Status](https://camo.githubusercontent.com/d0cdc3f4e273ab716616274b916262b5cda1f987d7673de72a71556109ffc7b0/68747470733a2f2f636f766572616c6c732e696f2f7265706f732f6769746875622f4a43546f6f6c732f4a43546f6f6c732f62616467652e7376673f6272616e63683d6d6173746572)](https://coveralls.io/github/JCTools/JCTools?branch=master) [![Build Status](https://camo.githubusercontent.com/1e39f1dd898c35f8ad63cb2885ee29c0fd2abcc57adebe869d6d4a2934628451/68747470733a2f2f7472617669732d63692e6f72672f4a43546f6f6c732f4a43546f6f6c732e7376673f6272616e63683d6d6173746572)](https://travis-ci.org/JCTools/JCTools)

## JCTools

Java Concurrency Tools for the JVM. This project aims to offer some concurrent data structures currently missing from the JDK:

* SPSC/MPSC/SPMC/MPMC variations for concurrent queues:
  * SPSC - Single Producer Single Consumer \(Wait Free, bounded and unbounded\)
  * MPSC - Multi Producer Single Consumer \(Lock less, bounded and unbounded\)
  * SPMC - Single Producer Multi Consumer \(Lock less, bounded\)
  * MPMC - Multi Producer Multi Consumer \(Lock less, bounded\)
* SPSC/MPSC linked array queues \(bounded and unbounded\) offer a balance between performance, allocation and footprint
* MPSC/MPMC XAdd based, unbounded linked array queues offer reduced contention costs for producers \(using XADD instead of a CAS loop\), and pooled queue chunks for reduced allocation.
* An expanded queue interface \(MessagePassingQueue\):
  * relaxedOffer/Peek/Poll: trade off conflated guarantee on full/empty queue state with improved performance.
  * drain/fill: batch read and write methods for increased throughput and reduced contention

There's more to come and contributions/suggestions are most welcome. JCTools has enjoyed support from the community and contributions in the form of issues/tests/documentation/code have helped it grow. JCTools offers excellent performance at a reasonable price \(FREE! under the Apache 2.0 License\). It's stable and in use by such distinguished frameworks as Netty, RxJava and others. JCTools is also used by commercial products to great result.

## Get it NOW!

Add the latest version as a dependency using Maven:

```text
        <dependency>
            <groupId>org.jctoolsgroupId>
            <artifactId>jctools-coreartifactId>
            <version>3.3.0version>
        dependency>
```

Or use the awesome, built from source, [https://jitpack.io/](https://jitpack.io/) version, you'll need to add the Jitpack repository:

```text
        <repository>
          <id>jitpack.ioid>
           <url>https://jitpack.iourl>
        repository>
```

And setup the following dependency:

```text
        <dependency>
            <groupId>com.github.JCTools.JCToolsgroupId>
            <artifactId>jctools-coreartifactId>
            <version>v3.3.0version>
        dependency>
```

You can also depend on latest snapshot from this repository \(live on the edge\) by setting the version to '3.1.1-SNAPSHOT'.

## Build it from source

JCTools is maven built and requires an existing Maven installation and JDK8 \(only for building, runtime is 1.6 compliant\).

With 'MAVEN\_HOME/bin' on the path and JDK8 set to your 'JAVA\_HOME' you should be able to run "mvn install" from this directory.

## But I have a zero-dependency/single-jar project

While you are free to copy & extend JCTools, we would much prefer it if you have a versioned dependency on JCTools to enable better support, upgrade paths and discussion. The shade plugin for Maven/Gradle is the preferred way to get JCTools fused with your library. Examples are available in the [ShadeJCToolsSamples](https://github.com/JCTools/ShadeJCToolsSamples) project.

## Benchmarks

JCTools is benchmarked using both JMH benchmarks and handrolled harnesses. The benchmarks and related instructions can be found in the jctools-benchmarks module [README](https://github.com/JCTools/JCTools/blob/66e6cbc9b88e1440a597c803b7df9bd1d60219f6/jctools-benchmarks/README.md). Go wild and please let us know how it did on your hardware.

## Concurrency Testing

```text
mvn package
cd jctools-concurrency-test
java -jar target/concurrency-test.jar -v
```

## Come up to the lab...

Experimental work is available under the jctools-experimental module. Most of the stuff is developed with an eye to eventually porting it to the core where it will be stabilized and released, but some implementations are kept purely for reference and some may never graduate. Beware the Jabberwock my child.

## Have Questions? Suggestions?

The best way to discuss JCTools is on the GitHub issues system. Any question is good, and GitHub provides a better platform for knowledge sharing than twitter/mailing-list/gitter \(or at least that's what we think\).

## Thanks!!!

We have kindly been awarded [IntelliJ IDEA](https://www.jetbrains.com/idea/) licences by [JetBrains](https://www.jetbrains.com/) to aid in the development of JCTools. It's a great suite of tools which has benefited the developers and ultimately the community.

It's an awesome and inspiring company, [**BUY THEIR PRODUCTS NOW!!!**](https://www.jetbrains.com/store/#edition=commercial)

JCTools has enjoyed a steady stream of PRs, suggestions and user feedback. It's a community! Thank you all for getting involved!

