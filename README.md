# Behavox | Test Task

## Introduction

Hi, my name is Mihai Ionescu and I've been given a test task to evaluate my ability to adapt to **Behavox** technologies.

Before we start, I have to say that (at first sight) there a lot of new stuff that I'm not really familiar with as I've been used to a **RDBSM** and I haven't used **HBase** as of today.

This isn't really a problem, because I have a great capacity to absorb information and I enjoy learning. But I'll do my best to expand my knowledge during the 4 days I got and I'll try to explain my new findings as simply as I can.

Each section will be divided accordingly in subsections to maintain an _easy-to-read_ document. This will also demonstrate how I approached this task. If you wish to, you can skip to the **JIRA Bug** section via the **Table of Contents**.

For a greater understanding of my thought process, I'll be using a system called **_Mihai's thoughts_** when I want to express some thoughts related to a given subject. This will help you better understand **_what is missing in my comprehension_** and/or **_how I approached a specific subject_**.

Now, in most cases I prefer quality over quantity, but for this given test I'll go for a hybrid. I'll try to cover as many subjects as I can while keeping it simple and straight to the point.

Lastly, as this test has a time limit, I won't be creating a lot of diagrams/images to explain or demonstrate concepts. I'll use images from different sources if I find that those are complementing well my explanations.

Let us start!

## My starting knowledge

Here is a list of subject that I am familiar with _(prior to this task and relative to it)_ :

- RDBSM
- JIRA
- SQL
- JAVA (OOP)
- Distributed Systems
- Clusters
- Logs

# Table of Contents

- [HDFS](#hdfs)
- [NoSQL](#nosql)
- [HBase](#hbase)
- [JIRA Bug](#jira-bug)

# HDFS

As I understand it:

> **HBase** is a **NoSQL** datastore built on top of a **Hadoop Distributed File System** (**HDFS**).

But before going deeper in, let's look at what is a **HDFS**.

## HDFS | What is it?

A **HDFS** works by storing data across a number of separate machines. Each data file is duplicated multiple times (3 times by default) to assure its availability. Because of this, a **HDFS** is fault tolerant, scalable, and extremely simple to expand.

This kind of system uses fast _random read-write_ accesses and is useful only if there are millions/billions of entries to work with.

Also, it is not a _conventional_ **RDBSM**. There are no named colums, triggers, or advanced features. It works more as a data storage system with a defined block size.

## HDFS | How it works

Here's a basic overview of the architecture :

- **DataNode**: Entity that manages the data located on a single _machine_.
- **NameNode**: Higher entity that has access to all **DataNode** entities.

To save and retrieve data, a call to the **NameNode** is required each time. The **DataNode** entities executes all the commands coming from the **NameNode** and they report back to it.

This artichecture is in fact a simple _master-slaves_ setup.

Here is an example I found on the internet that illustrates a more _in-depth_ overview.

![HDFS Architecture](https://www.guru99.com/images/Big_Data/061114_0923_LearnHDFSAB2.png)

> **_Mihai's thoughts_**
>
> I've looked at some tutorials on how to access a **HDFS** through a **Java API**. I've also seen some videos (and some articles) on how a data file is divided into blocks and copied in available blocks throughout multiple **DataNode**s.
>
> I think that I get the idea, but I won't go into more advanced stuff, because I want to keep more of an overview of each technology.
>
> If I need to further demonstrate my knowledge, I'll be using the _**JIRA Bug**_ related section to do so.

# NoSQL

Before jumping in **HBase**, let's take a quick look at **NoSQL** as this is something new to me.

## NoSQL | First let's talk SQL

When we talk about **SQL**, we also talk about planning ahead the structure of the whole database. We see entities, relations, primary keys, foreign keys, properties, etc.

While this approach is considered as a _standard_ in the database world, we get some common proplems :

- What if the data changes in shape?
- What happens when we have too much data and the requests take too long to complete?

A **SQL** approach assumes that the database **schema** was planned correctly from the start and that there won't be any major changes in the **entities** and **relations**.

If the requests take too long to complete, the solution is often to upgrade the hardware to accelerate the process. But this is a pricey solution...

## NoSQL | Structure

An important difference is that there is no database **schema**! The data is saved as **Document**s in groups called **Collection**s.

- A **Collection** is somewhat the equivalent of a _table_ if we would've used **SQL**.
- A **Document** is an entry with an _undefined_ number of _properties_.

Another difference is that there are less computations involved to complete a request, because there are no **relations**. The data is all in one place : in a **Document**.

## NoSQL | Why is it used with HBase?

We saw that with **SQL** we had some problems :

- The bigger the database, the longer it takes to get a result out of a query.
- The data must match a **schema**.

**HBase** is a system to manage an _enormous_ amount of data and it has to access it _fast_. How do we accomplish this?

Now that's where **NoSQL** comes into play!

- The requests are done really fast. This is due to the fact that there is no _huge server_ that must lookup **Relations** and **Schemas**. Instead we are using a **HDFS** under the hood and we have multiple machines querying _easy-to-access_ data individually.
- As stated above, accessing an entry is as simple as requesting a **Document**. This means that the request are yet faster, because there shouldn't be any heavy operations such as _joining tables_, etc.
- An entry can change in shape and it won't affect the rest of the data. Hence, for an enterprise like **Behavox**, the data may vary in shape and there are no major ajustments needed to the database.

Okay, so this is pretty neat! ...But how do we use that **HDFS** efficiently?

Let's look at **HBase** itself.

> **_Mihai's thoughts_**
>
> Before we move on, I saw that in **HBase**+**NoSQL** we've got a concept called _column families_ in the **Database**. I'm not quite familiar with this concept, but I think that this may be quite simple in fact. I'll leave that there for now.

# HBase

As I said before :

> **HBase** is a **NoSQL** datastore built on top of a **Hadoop Distributed File System**.

But what does it do exactly? Well, to put it simply :

> **HBase** organises **NoSQL** related data so it can be served from multiple locations at the same time. In other words, we can use **HBase** to store and retreive a colossal amount of data faster, because there are several machines working together to spread the data randomly throughout **regions** and collect it back when needed.

Let's take a look at how those things are done.

> **_Mihai's thoughts_**
>
> Ok so, the further I learn about **HBase** the more complex it gets. I'm pretty confident that I won't become a **HBase** _expert_ over a few days, but I think I got the basics down. Let's go through some **HBase** core components and see what I can get out of it.

> **_Mihai's thoughts (2)_**
>
> At this point, I'm starting to understand pretty well the **JIRA Bug**. So it's looking good!

## HBase | Architecture

We saw that in the **HDFS** architecture we had a **NodeName** (master) and some **DataNode**s (slaves).

We have a similar architecture here, but there are more components as **HBase**'s scope is different than a simple data storage system.

In **HBase** we have **Region Server**s (slaves), **HMaster**s (masters) and **Zookeeper**s (_higher masters_ if I can say so).

> **_Mihai's thoughts_**
>
> Keep in mind that this is a simple overview so I can acquire the knowledge to approach the **JIRA Bug** report.

## HBase | Region Server

## HBase | HMaster


znode = master

## HBase | ZooKeeper


# JIRA Bug

In this section, we'll be looking at [the bug presented in the test task](https://issues.apache.org/jira/browse/HBASE-14498). I will try to deconstruct the problem into smaller pieces and attempt to interact with it.

## JIRA Bug | Basics

## JIRA Bug | In my words

## JIRA Bug | Root of the bug
