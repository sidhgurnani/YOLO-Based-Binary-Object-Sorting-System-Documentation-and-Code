## Software Considerations

Early in the project timeline, the focus for any architectural decisions was heavily biased towards the software, as there are many possibilities for the software, and significantly fewer physical prototype solutions. 

### Early Software Decisions

Given that the project solution requires a cyberphysical system, it is best to work with some kind of microcontroller. In addition, since laptops can vary with processing power, age, graphics performance, etc, it is best to utilize a solution that is lighter on a laptop. The goal here is to have as many devices as possible be able to run the software. 

This is where the first major pathway to a solution arrives: using WebUSB. WebUSB is an API that allows certain compatible microcontrollers to be able to communicate to a web browser. This would open up the potential for users to be able to interact with the Arduino using an HTML file. Keeping this solution on a web browser would be able to allow the system to run much lighter.

Such a solution can be seen with [Google's Tiny Sorter](https://experiments.withgoogle.com/tiny-sorter/view/), which is primarily used as a teaching tool to help people learn about computer vision and sorting. One of the main drawbacks about this solution is that it doesn't accomodate for a variety of object types, and would be difficult to sort larger and heavier objects. It also does not allow a user to input a pretrained model, and instead relies on the end user to train the system. If done improperly, or in bad lighting, then the model produced is not as accurate.

This led me down a rabbit hole of using WebUSB and getting my hands on an Arduino Leonardo, which is a WebUSB-compatible Arduino. My findings about WebUSB are laid out in more detail [here](WebUSB_Findings.md).

Long story short, WebUSB is not ideal (from a simplicity point of view) due to the communication protocol that needs to be put in place. With any webpage, a client and server is needed. However, this communication would have to be extended to the Arduino as well. With a large amount of communication happening between the user, client, server, and Arduino, there is bound to be a loss of data somewhere due to data being sent and recieved from multiple sources, essentially overloading the system. While the ideal scenario would be to run something like this, my prototype will not be using a web-based system to reduce the complexity of communicating information. 

### Final Software Decisions

Based on the information outlined above, I have chosen to run the program locally. A graphical user interface (GUI) will be built via Python, which will also handle communication to and from the Arduino via a serial communication port. 

The GUI will allow the user to dictate what gets done and when as well as monitor the whole system and provide that information to the user. A GUI can be constructed in many ways, however, I have chosen to use tkinter for Python. 

Communication to and from the Arduino via a serial communication port is a good option as it allows data to be sent to and from using one channel, whereas using WebUSB would mean data has to transfer from client to server to Arduino. 

More technical details can be seen on the development pages, where the software versions are outlined as I iterate through them towards a working solution. 

## Prototype Considerations (WORK IN PROGRESS)

### Early Prototype Decisions

Early on, the choice had to be made to decide whether to design a prototype that works well with one kind of object or womething that won't work as well but can support a varierty of objects. 

Initially, I thought about going with the former. Coins are something that a lot of people have at their homes, including myself, and I thought testing using that would be great. I had initially wanted to design a system that works really well with coins, but coins only. However, it is more ideal to use a system that can support a variety of objects, even at reduced ability and capacity, as it allows for a proper proof of concept to be established with a relatively universal solution.

There are more things that need to be figured out before designing any prototype:

* How are objects fed into system?
* What happens if the user cannot feed objects one-by-one? How does the system handle this?
* How can the system detect the objects in a webcam? Should a box be built around the webcam and an LED to illuminate it or is there another solution?
* What mode of movement should objects use when sending an object to the webcam and from the webcam to the bin?
* etc...

### Final Prototype Decisions

Here are some answers to some of the questions I posed above?

* **How are objects fed into system?** &rarr; They can be fed one by one or ideally, through some kind of feeder, either vibration based or centrifugal.
* **What happens if the user cannot feed objects one-by-one? How does the system handle this?** &rarr; Using something like a feeder can allow objects to be fed more uniformly, regardless of how the user would load the objects.
* **How can the system detect the objects in a webcam? Should a box be built around the webcam and an LED to illuminate it or is there another solution?** &rarr; ....
* **What mode of movement should objects use when sending an object to the webcam and from the webcam to the bin?** &rarr; ....