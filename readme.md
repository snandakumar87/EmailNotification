
Email Notification in Red Hat Process Automation Manager 7
Configuration
The following one time configuration needs to be set so that we can configure socket binding and then the actual JNDI resource of mail session type. In this case we are assuming a gmail account, the following configuration was executed using the jboss-cli. 
(note: to bring up the jboss cli on an EAP standalone, navigate to $JBoss_home/bin/jboss-cli.sh)
/socket-binding-group=standard-sockets/remote-destination-outbound-socket-binding=jbpm-mail-smtp/:add(host=smtp.gmail.com, port=465)

/subsystem=mail/mail-session=jbpm/:add(jndi-name=java:/jbpmMailSession, from=<fromemail>@gmail.com)
/subsystem=mail/mail-session=jbpm/server=smtp/:add(outbound-socket-binding-ref=jbpm-mail-smtp, ssl=true, username=<toEmail>@gmail.com, password=xxxxxxxx)

When starting EAP server, you have to provide the JNDI name to be used by jBPM to find the mail session, this is given as system property:

./standalone.sh .... -Dorg.kie.mail.session=java:/jbpmMailSession
Creating an EMAIL task and project level configuration
Import the mortgages_process from samples.
Navigate to Settings -> Deployments -> WorkItemHandler. 
Create a new Work item handler for Email 

Doing this ensures we are registering org.jbpm.process.workitem.email.EmailWorkItemHandler and make sure all these values are taken from JNDI mail session of the application server.
Let us now add a Email task as a first step of the mortgage process. Choose the Email task from under Service task.


We need pass the from, to, subject and body for the email.



In this example, i have configured an email to send out the application object of the mortgage app. Once the configuration parameters are set, we can start the process.
We can also add in html for the body of the email message.
Consider the simple html body: <!DOCTYPE html>
<html>
<body style="background-color:powderblue;">

<h1>This is a heading</h1>
<p>This is a paragraph.</p>

</body>
</html>

You could also multiple receivers, by adding ; separated email addresses.
The above example can be found at:
https://github.com/snandakumar87/EmailNotification.git



