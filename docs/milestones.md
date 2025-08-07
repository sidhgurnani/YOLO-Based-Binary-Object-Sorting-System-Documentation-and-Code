# Milestones

This page will contain a bit of information about each milestone as shown on the main page, including some of the goals of each, and if applicable, anything I have done to ensure that the milestone will be marked as "complete". For example, for testing, this page will contain information about any initially planned test runs, but will not cover the results. For more details descriptions of the architecture, design, testing, etc, view those respective pages in the navigation. 

## 1. Refresh Programming Knowledge

Based on my personal experiences, this phase of the project was more of a refresher for me. However, someone with less background with coding, in my opinion, should go through some coding lessons online and/or take a class. 

Prior to starting this project, I had used the following:

* Java
* MATLAB
* Python
* Arduino IDE

Java was the first programming language I had learned, and I had done so in a more formal way by taking a course during my second semester of my undergrad. This class taught me the basics of Object Oriented Programming, and I was tasked with a variety of assignments with each one getting more complicated. I had learned the basics of everything from writing my first program to loops to client and server code. In addition, I had to work through a project at the end of the semester, where I worked with a team to code a basic learning interface similar to Canvas and Brightspace. 

I next learned MATLAB, which I used to complete the bulk of my engineering assignments during the first year engineering (FYE) curriculum that Purdue has in place. 

From there, I learned Python, also by taking a class at Purdue (EBEC 101). Here, I used Python in a traditional sense, by completing relatively simple assignments that taught me the basics of coding, but instead entirely in Python. 

After that, I learned how to program Arduinos using the Arduino IDE, albeit in a more nontraditional sense where I was introduced to everything in a more practical sense, and have since fine tuned my abilities through project-based learning. 

That being said, I had a decent background when it came to coding, so a few lessons in Python from YouTube were enough to get me going again and ready to learn more.

For someone looking to replicate this, I would strongly suggest learning Python, as that is the programming language that is used throughout this project, as well as learning the basics behind working with Arduinos and familiarity with the IDE. I will also be using PlatformIO to run my project entirely in VSCode instead of using two different IDEs.

## 2. Understand Basics of Machine Learning

Prior to starting this project, I had zero experience with Machine Learning. This is what I did to familiarize myself with the basics of ML. I ended up taking free online classes to refresh my Python knowledge and expand that understanding into machine learning. Links to the classes are below, with certificates on my LinkedIn page.

* [Machine Learning with Python course | Cognitive Class](https://cognitiveclass.ai/courses/machine-learning-with-python)
* [Data Visualization with Python course | Cognitive Class](https://cognitiveclass.ai/courses/data-visualization-python)
* [Python for Data Science course | Cognitive Class](https://cognitiveclass.ai/courses/python-for-data-science)
* [Predictions: Regression for Car Mileage and Diamond Price](https://cognitiveclass.ai/courses/predictions-regression-for-car-mileage-and-diamond-price)
* [Customer Clustering with KMeans to Boost Business Strategy](https://cognitiveclass.ai/courses/customer-clustering-with-kmeans-to-boost-business-strategy)
* [Precise Predictions: Classification for Flower and Tumors](https://cognitiveclass.ai/courses/precise-predictions-classification-for-flower-and-tumors)

While this is not by any means a guide to learning machine learning, the emphasis for this project was to familiarize myself with it and get better at implementing it as I progress through this project. 

## 3. Develop Basic Software

Developing basic software entails having code up and running for the software side of this project, which entails mainly getting YOLO working as well as setting up the interface and all basic functions that need to be in place for this to be successful. At the minimum, here is what is expected:

* Be intuitive to use, and look somewhat polished
* Allow user to perform basic functions
* Allow user control over the process
* At the minimum, recognize when an Arduino is connected via USB to laptop
* Leverage webcam to detect objects
* Have some error handling in place and prevent user from going too far out of order

## 4. 3D Model and Design Physical Product

The goal here is to design a minimum viable prototype, where the parts and system is able to work adequately well and a well established proof of concept is present. At the minimum, here is what is expected:

* Be simple to use for the end user
* Allow user to perform basic functions, notably loading and sorting objects under supervision with minimal human intervention
* Allow user to load a small to medium number of items for sorting, mainly for testing purposes
* Objects should get sorted into 2 bins, where one is the target and the other is not the target, hence the "binary" aspect of the sorter
* Keep costs as low as possible, with the aim to be reproducible

## 5. Integrate Software and Hardware

Once basic software and working prototype are set up, work needs to be done to integrate the software and prototype made earlier. Here, the different modes will be properly established:

* Object Detection Mode: Here, objects will pass through the mechanical system in place and not do any kind of sorting. The goal is to detect a list of unique objects so that the number of passes the system requires can be established.
* Object Sorting Mode: Here, objects will be sorted through the system, with motors/actuators being used to facilitate the sorting process.

At this point, small scale testing will be carried out. The aim here is to get around 10-20 objects with at least 3 unique classes of objects properly sorted. 

## 6. Final Testing

At this point, the system is well established, and the testing can be slightly scaled up. In addition, different models will be tested to ensure that the system is more "plug and play", where the end user will be able to train or load in a pytorch model to be used with YOLO.

## 7. Reflection and Next Steps

Reflections will be made here to discuss flaws of working prototype as well as a path forward should the project timeline be extended.