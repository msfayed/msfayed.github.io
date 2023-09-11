# So What is Opinio ?

Opinio is one of the best surveys systems , it supports multiple users, multilingual surveys, survey collaboration, powerful features such as skip logic and piping, and a highly scalable architecture so it will cover the survey needs for everyone in your organization.

I found my self enforced to install and use it as my company already buy license for it , Opinio is a web application written in Java so it needs a webserver that supports Java ( I used Tomcat ) .
after download Opinio File ( .zip ) and Tomcat installation file ( be aware to download the .exe file not .zip file for easy setup ) also you must download Java SE ( JRE )

1- Frist thing I installed JRE , very easy steps Next .. Next .. Finish ( I Like this type of installations ).

2- Then I add new environment variable ( JAVA_HOME ) , you make this by right click on computer under start menu then choose Properties



from the new window choose Advanced system settings –> Environment Variables , then from the opened window click on New in the bottom of the window , enter JAVA_HOME as variable name and the Path to Java installation folder in the value.





3- then I installed Tomcat it was also very easy just you need to enter user name and password for Tomcat Administrator and the Port that the webserver (Tomcat) will listen to . ( in my case I make this port 8080 as my server already contain IIS which listen to Port 80 ).



4- to test that Tomcat is working fine I opened the browser and typed ( http://localhost:8080 ) I found tomcat default page which is good .

5- Then I extracted opinio.war from Opinio zip file to [Tomcat Folder]\webapps then I restarted the Tomcat Service ( From Start menu there is shortcut for managing Tomcat ) and it automatically create opinio folder and configure it

6- by testing local URL ( http://localhost:8080/opinio ) I found Opinio is working just fine. but when I accessed this URL from my PC over the internet ( by replacing <localhost> with the server IP ) it didn’t work , finally I figure it out it’s the firewall  I just opened the 8080 port and it works .
