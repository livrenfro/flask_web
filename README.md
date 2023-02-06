Lab

This is a "hello world" assignment for flask/docker that just ensures you have a sane working environment.

Part 0: terminal-based web client

Any task that you can do without the terminal can be done with the terminal, including browsing the web. The most popular command line web browser is called links. Login to the lambda server, and run the command

$ links http://www.phrack.org
to browse to the phrack magazine. Phrack is an old-school hacker zine. Issue 7, article 3 has the famous "hacker manifesto", and you should try to browse to it and open it in links.

HINT: The up/down arrows take you to the next link, but there's a lot of links on a page, so moving to the correct link can take a long time. Use the / key to search for text to navigate the webpage faster.

Browsing the web this way is unfortunately rather inconvenient, and so you may be tempted to ask why would anyone do it? The simplest answer is that many people must use command line web browsers due to physical disability. For example, the famous physicist Stephen Hawking



could not use a mouse, and so could not use a traditional web browser. In order to make your webpages accessible to people like Hawking, it is good practice to test your webpages in the links browser. And the Americans with Disabilities Act (ADA) actually requires that large companies and government agencies do this.

Another reason to use the links browser is that we can run it on remote machines and access web servers that our laptop doesn't have direct access to. For example, run the command

$ links http://localhost:5000/
You should see a simple hello world webpage get displayed in the links browser. But if you try to access this url from firefox on your laptop, you will get an error message. This webpage is running on port 5000 of the lambda server, but you can't connect to this webpage directly due to various firewalls that the CMC IT folks have installed. The links program is the easiest way to bypass these firewalls.

Part 1: port forwarding

For most of, however, browsing the web with links would be very inconvenient, and so we would rather be able to use firefox to access the webpage. (But definitely not chrome/safari... bleh... that spyware crap is gross.) Port forwarding is a way for ssh to make web applications that are only available on remote servers available locally on your computer.

Port forwarding gets enabled when you login to the lambda server. Depending on how you login, the instructions will be slightly different.

Mac/Linux/Windows (with power shell): log on to the lambda server with the following command (changing username to your username):

$ ssh username@134.173.191.241 -p 5055 -L 8080:localhost:5000
This tells ssh to forward all requests to port 8080 on your computer (localhost) to port 5000 on the lambda server.

Windows (with Putty): You will have to select the appropriate checkboxes in putty to get local port forwarding enabled. You can follow these instructions to get pictures of where the checkboxes are located. You want port 8080 for the local side and port 5000 for the remote side of the connection.

Once you're logged into the lambda server, visit the url http://localhost:8080 in your web browser. You should see a page showing the date.

Part 2: a terminal-based web server

Now we will create a simple web server using shell commands. You will need a partner to complete this part of the lab.

RECALL: It is an academic integrity violation to work with a partner on these assignments if you are not either in class or in the QCL.

On the lambda server, run the following command:

$ while true ; do echo "$MESSAGE" | nc -lq1 -p $PORT ; done
where $MESSAGE is a message you want to send to your partner and $PORT is a number number between 2**10 and 2**16.

NOTE: nc is short for "net cat" and is like the familiar cat command but outputs to the network instead of the terminal. So in the command above, we're using the pipe to send the results of echo to the network. When a web browser connects to lambda-server:$PORT By piping in more complicated commands into nc, we can easily create simple web services in one line of shell that would take hundreds of lines of python.

Now, your partner should try to connect to your webserver using port forwarding. (And you should try to connect to theirs.) Don't move onto the last step until you get this working.

Part 3: docker

Follow these instructions to create a simple flask app running in a docker container: https://runnable.com/docker/python/dockerize-your-flask-application.

These instructions were not designed specifically with this class in mind, and thus you will have to modify parts of the instructions in order to get them to work. This is intentional in order to get you more practice adapting tutorials into different computational environments. There are two main modifications you'll have to make:

In the docker run command, you will have to change the port that docker exposes to a port other than 5000. (This is because you're all running this code at the same time, and you can't all use the same port.) I recommend using your user id as a port number, as this will guarantee that you don't run into conflicts with other students. Your userid is stored in the environment variable $UID.

In order to view your webpage from your laptop, you will have to connect to the lambda server with local port forwarding enabled. The command will look something like

$ ssh username@134.173.191.241 -p 5055 -L 8080:localhost:DOCKERPORT
where DOCKERPORT is whatever port you specified.

Finally, there's a handful of errors that you'll get when you build the project. You'll find that fixing these errors only takes a very small change to the project files, but figuring out exactly what this change is will be quite difficult. The fundamental problem is that various libraries/packages have introduced breaking changes since the author of the tutorial wrote the tutorial. The easiest way to figure out how to get the right versions is to open up a working container with the docker run command, then manually try installing all the different versions of the libraries until you get something that successfully creates the webpage. Once you've figured out the correct sequence of commands, then you should modify the Dockerfile to reflect these new commands.

Submission:

Put all your files from Part 3 into a github repo, and upload the url to sakai.

Homework
No homework this week :)

Just work on the twitter/MapReduce homework.
