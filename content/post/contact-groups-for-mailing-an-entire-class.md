+++
title = "Contact Groups for Mailing an Entire Class"
date = "2015-10-18T13:46:50.140000"
+++

(For those of you just interested in the script and instructions on how to use it, you can just scroll to the bottom of the post)

**Developemnt**

Recently, I needed to create a contact group in Gmail to allow me to mass-email students files for some lab sessions. My tutorials contain no less than 24 students, so I was initially exasperated at the thought of manually inserting 24+ email addresses into a contact group. However, then it dawned upon me - I' a computer scientist ! Why do something I can make machine do for me instead. :P

Driven by laziness, I began to research means of scripting for Gmail, and soon enough, I came across Apps Script. Apps Script is a variant of JavaScript tailored to interact with several of Google's productivity services. Apps Script provides facilities for operations such as filtering emails, creating contact groups, and adding contacts to contact groups. Brimming with joy at the realisation of not having to add students to a list ad nauseam, I scanned through Apps Script documentation to identify the APIs I needed to automate contact group creation.  

First and foremost, I needed to define how I wanted to interact with the script. Obviously, I want to able to use this script for different classes with minimal effort. I decided that the contact group for the class and the subject of the email requesting to join the class ought to be the same. I then delegated the actual work to another function. Wrapped up into a *main* function, I got the following:

<script src="https://gist.github.com/InzamamRahaman/a83e76b6775a8bba8c12.js"></script>

As you can probably tell, *extractAndAddToGroup* does the heavy lifting. Its tasks comprised the following:

* Finding all emails with the group name as a subject
* Extracting the email addresses
* Extracting the contact's information

From the above, I also created the specification for my students. Send an email to the subject with only their first and last name separated by a space.

Getting the list of emails involved finding all threads with that subject and then collapsing them together to yield a single array of messages from which my students' email addresses and names could be extracted.

<script src="https://gist.github.com/InzamamRahaman/93c42ea00b7b54d9479d.js"></script>

Getting the contact information was also pretty straightforward. 

<script src="https://gist.github.com/InzamamRahaman/831865fdf4c4d2754cee.js"></script>

Unfortunately, there was some difficulty in extracting the correct email address. I naively assumed that the *getFrom()* method on *GmailMessage* objects would return a string containing only the email address. Instead, the *getFrom()* message usually returned a string containing additional user information. I suspect that email forwarding might be partially responsible this. Luckily, the actual email addresses are surrounded by either spaces or angle brackets. Drawing from this generalisation, I used a regular expression to extract the email address from the returned string.

<script src="https://gist.github.com/InzamamRahaman/318c7337c9e38c44dced.js"></script>

Lastly, we need to write the *extractAndAddToGroup* function. Initially, I encountered a bit of issue with the amount of processing my script was performing. To prevent DOS attacks, Google limits the number of times a service can be called per second. Hence, only some emails were being added to the contact group before the script crashed. To prevent my script from mimicking the behaviour of a DOS attack, I used *Utilities.sleep* to pause the script for 2 seconds.

<script src="https://gist.github.com/InzamamRahaman/c3ec12f83a81b7687db9.js"></script>

**Usage**

In [script.google.com](http://script.google.com), start a new project, inserting the following code:


**Very Important:** By sure to change the groupName variable to the name that you want to provide for the contact group.

Instruct your students to send you an email containing the contact group name as the subject, and the body containing only their first and last name separated by spaces. 

When that is finished, run the script to create an contact group for your class.
