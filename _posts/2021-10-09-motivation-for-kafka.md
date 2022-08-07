---
layout: post
title:  "Motivation for Kafka"
author: Gaurav
categories: [ Kafka ]
image: assets/images/motivation-for-kafka/header-1.png
---
If you wonder what are different industries doing with Apache Kafka, here are some insights.

Remember good old days when you would call your friend.

Tom             calling                Ryan   

<img src="/assets/images/motivation-for-kafka/Screenshot-1.png" height="200px" width="300px"/>

The above conversation would let you know how your friend is doing, what are the life events he/she is experiencing and how much they love their newly bought car.

But there is a paradigm shift from such static view to a new world of social media.

Tom              views live feed of            Ryan

<img src="/assets/images/motivation-for-kafka/Screenshot-2.png" height="200px" width="300px"/>

With the help of social media application, we can now see a constant feed of all the activities of our friends.

Same view is true for how we consume news.

Reading newspaper              to         Consuming news feeds

<img src="/assets/images/motivation-for-kafka/Screenshot-3.png" height="200px" width="300px"/>
<img src="/assets/images/motivation-for-kafka/Screenshot-4.png" height="200px" width="300px"/>


If we try to understand the changes above from an application data perspective, it means that rather than showing a static view/snapshot/summary of the data at a particular time, there is a need to constantly showing data flowing continuously as a stream of discrete events. Such a need has caused a paradigm shift known as Event Stream Processing in the world of application development. Another important aspect of these applications is to address the need of Real time processing of these events to show the current state of the data and also store these events to show historical view of the data. If we think about all the latest applications installed in our mobile phones like Facebook, Instagram, twitter, LinkedIn, Netflix, Uber, Uber eats etc. they all are inherently event-driven. Think about seeing continuous delivery updates you get when order your food item, dynamic suggestions you see when you scroll through Netflix and so many other such use cases on all successful digital businesses applications.

To address all such need, a Single Platform was required which could do real time stream processing and store events and connect all the features/use-cases together for any application. Apache Kafka arrived in the scene as one such platform and has now become a de facto standard  for real time event processing.

![Screenshot 5](/assets/images/motivation-for-kafka/Screenshot-5.png)

Here is a set of companies who use Kafka as their Event Streaming Platform.

![Screenshot 6](/assets/images/motivation-for-kafka/Screenshot-6.png)

Its a mix of companies which exists from day one of digital world to very fresh startups.

### Example Use-cases 

**Real time fraud detection**

For any financial services company that issues credit card, they need to find the fraud happening in real time rather than finding it out next day after running some batch job, and notifying the end customer within seconds of the any potential fraudulent transaction. 

**Automotive**

<img src="/assets/images/motivation-for-kafka/Screenshot-7.png" height="200px" width="300px"/>

All the high end cars these days have lot of Internet Of Things (IOT) devices which will have connectivity to a data network. All those devices and computers record enormous amount of data for reporting purpose and relay it back to a central head quarter or sometimes allow two way communication where services can respond to the data reported giving a very pleasant experience overall. Fundamentally, all these things have been built on top an event streaming platform.

**Ecommerce**

<img src="/assets/images/motivation-for-kafka/Screenshot-8.png" height="200px" width="300px"/>

E-commerce is full of events where people navigate through the website, search, checkout, provide reviews and all these events can be analysed in real time to determine how products are performing, behaviour of the customer and much more which is hard to do with traditional databases and where Event streaming platforms help in quickly implementing such features with less engineering efforts.

**Customer 360**

<img src="/assets/images/motivation-for-kafka/Screenshot-9.png" height="200px" width="300px"/>

Typically there will be many customer database tables in an organisation to store data about customers which will be scattered among lines of business and departments providing different views to each of them. Even streaming platform solves the problem of getting an integrated view of all the customer which can help in up-sell and cross-sell and save lot of costs.

**Core Banking**

<img src="/assets/images/motivation-for-kafka/Screenshot-10.png" height="200px" width="300px"/>

If you are thinking how come banks are real time when we it takes 2 days for banks to transfer payments between accounts, then yes, the payment gateways are getting smarter day by day and very soon Kafka based solutions will solve the problem of delays in bank account payment transfer.

**Health Care**

<img src="/assets/images/motivation-for-kafka/Screenshot-11.png" height="200px" width="300px"/>
<img src="/assets/images/motivation-for-kafka/Screenshot-12.png" height="200px" width="300px"/>

Just like automotive industry (Cars with IOT devices) , health care industry is also using a lot of IOT devices for monitoring and using Kafka to do data analysis and generate information that helps healthcare professionals to take better care of patients.

**Online Gaming**

<img src="/assets/images/motivation-for-kafka/Screenshot-15.png" height="200px" width="300px"/>

Online gaming is fundamentally event driven as it generates a lot events when a player moves or presses a button. An Event streaming platform can consume these events and do a lot of things like optimisation, enabling in-game purchases by determining who is doing what.

**Government**

<img src="/assets/images/motivation-for-kafka/Screenshot-14.png" height="200px" width="300px"/>

Government agencies leveraging Event streaming platforms to make more effective use of their data which is critical to accountability, public policies, program effectiveness, and mission success.

**Financial Services**

<img src="/assets/images/motivation-for-kafka/Screenshot-13.png" height="200px" width="300px"/>

Financial services require to provide enhanced customer experience by enabling a single strategy for communications with customers using mobile devices, securing environment using improved fraud detection engines. Kafka helps migrating all legacy systems by providing solutions around event processing.

Hope all these examples give a fair idea of what industries are doing with Apache Kafka.










