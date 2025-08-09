When a bullet is fired at a crime scene it leaves behind a casing and the bullet itself. The bullet size varies depending on the gun that is being used, but each bullet contains a unique identification that can be traced back to a specific gun that it was fired with. There is currently a relatively outdated process in place, whereby an examiner will have to manually measure and identify markings. While more advanced versions of this technique can be used (standards determined by NIST), it still heavily relies on an examiner going through. While human input can be deemed necessary, it is an inefficient process that can be streamlined if the bullets were able to be somehow pre-sorted. This would save time and make the process more efficient. 
	
Designing a new device would also require engineers to justify the need for such a product, and also delve into how it can be used in other applications. There are similar existing problems in other applications. An example of such an application would be coin sorting. If there were 1000 assorted coins of differing currencies, it would be possible to sort by hand albeit through a lengthy procedure. Another example is in an agricultural setting, where one would have to maybe sort through 2-3 different types of beans efficiently. Other examples include but are not limited to: various assorted electrical components (resistors, op-amps, capacitors, etc), assorted LEGOs, and assorted perler beads. 

An ideal device would be able to use an image of the product, identify what it is, and sort it based on the context of what would be coming into the device. This type of device can be designed using machine learning (ML), which is trained using images of a certain category. Note that this is a simplified version of the pipeline, and a more detailed pipeline will be ironed out in later phases of the design and prototyping process. There are various pros and cons to using ML in such an application, with the biggest pro being the ability to make an inefficient process more efficient and the biggest con being that the ability of the device functioning properly depends entirely on how well the ML implementation is done. 

To my knowledge, there is not such a device in the market, but there are aspects of the solution that are working in the market, and serve as a proof of concept. The first of these are a series of apps that are able to use the image of a coin to be able to give relevant information to the user. An example of such an app is “CoinSnap”, which allows users to scan coins and be provided with information and facts about that specific coin. This idea can be extended to similar apps on a smartphone for different purposes, such as identifying stars in the night sky or even forms of wildlife. 

A second working proof of concept serves as a working example of such a device. A company called Bulher has created a sorting device that can sort things such as agricultural seeds, coffee beans, etc in their “SORTEX” lineup. The device uses an optical sensor to be able to sort out a sample given the respective categories. 

But what if there was a solution that was more modular, something that could plug into a laptop and leverage its webcam to be able to physically sort through objects, not just identify them?

<div><br></div>

**_References:_**

Market Research:

* [Want to “change” your fortune? Coin apps might help](https://www.fourstateshomepage.com/news/local/do-you-want-to-change-your-fortune-coin-apps-might-help/#:~:text=CoinFacts%20(PCGS%20mobile%20app)&text=Many%20coin%20collectors%20say%20the,coins%2C%20and%20most%20users%20agree.) &rarr; Apps that take image of coins and are able to figure out its value

* [GitHub - thim0o/CoinDetector](https://github.com/thim0o/CoinDetector) &rarr; An image based coin counter using Python and Tensorflow
Coin Detector on GitHub

* [Optical Sorting Machines](https://www.buhlergroup.com/global/en/process-technologies/Optical-Sorting.html) &rarr; Working Concept within Agriculture

Publications:

* [Cell.com |  Intelligent Image-Activated Cell Sorting](https://www.cell.com/cell/pdf/S0092-8674(18)31044-4.pdf)
* [Nanocellect | Image-guided cell sorting technical overview](https://nanocellect.com/image-guided-cell-sorting/)