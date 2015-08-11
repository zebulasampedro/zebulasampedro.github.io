---
layout: post
title: An explanation of Apache Spark - Part 1
summary: A detailed look at Apache Spark in standalone deploy mode
tags: tutorial, apache, spark
---

Recently, a colleague and I have been using [Apache Spark](http://spark.apache.org/) to perform a series of analyses on an
extremely large, and constantly growing, set of system logs. Given the sheer size of logs and our reliance on a few of the
algorithms Apache kindly implemented in [MLLib](http://spark.apache.org/mllib/), Spark seemed the clear and time-saving
choice.

Ultimately, this assessment of Spark's potential turned out to be spot-on. What had previously taken days to run
with Pandas/SK-Learn now runs in seconds using fewer lines of code. Getting to this point though, was a difficult
endeavor. Spark is a deceptively complex framework, filled with show-stopping details that were not immediately 
apparent to my colleague and I.

In this multi-part post, my goal is to explain how to get Spark running, and what's
happening under the hood with your first few lines of code. Throughout, I will alternate between concept and step-by-step
instruction, highlighting those things that I wish I'd known earlier.

<div class="message">
  <strong>tl;dr</strong> Spark is awesome. Installing and understanding it isn't. Allow me to explain the things I 
  wish I'd known earlier.
</div>

##Spark vs. MapReduce

First off, it should be explained where Spark fits in relative to the Hadoop ecosystem. Hadoop consists of three primary
projects: Yarn, HDFS, and MapReduce. Spark is a potential replacement for MapReduce, and much of its design is informed
by shortcomings of the MapReduce programming model.

The most significant improvement Spark makes over MapReduce is allowing intermediate results to be persisted in-memory
(instead of writing out to disk between computations), which makes Spark viable for iterative workflows.

In-memory persistence also means that understanding how Spark allocates memory across the cluster is critical at every 
step, from installation to programming.

##Configuring Spark Standalone

Spark is supported by four different deploy modes, each of which alters the architecture in a non-trivial way:

- Local Mode
- **Standalone**
- Yarn
- Mesos

Throughout this series, we will be focusing on **Standalone deploy mode**, as it is the easiest to install and run on just
about anything.