# JIRA Ticket Delivery Tool - Web Application

### Summary

The software development cycle is a delicate process. You plan everything out,
code it, release it, and it doesn't just end there. We may find further bugs in
our code or the user may want additional features, and therefore, this whole
process is an iterative cycle.

The JIRA software helps simplify this process by allowing software engineers
to plan and prioritize tasks, as well as track which stage each task is in.

However, at Thales specifically, when a software engineer finishes a task, we
would tag each commit respectively to help us further keep track of issues we
have solved. Then we must run a series of test on the code to ensure it compiles
and runs before submitting it to SCM for further testing and validation. This
is a fairly long process that requires user attention for most of the process.
Furthermore, the SCM script only runs at night, meaning when a user submits a
JIRA ticket to be built and deliver, this is not reflected on the JIRA server
until the next morning.

As a result, our goal for this project is to automate the entire process of
tagging, building, and delivering JIRA ticket on a centralize network. This
allows software engineers to select their desired tickets, send it for delivery,
and focus their attention towards their own work, leaving the automated delivery
tool web application to deal with the laborious task of tagging, building, and
delivering JIRA tickets.

* * *

### Tools Used

* **Web Framework**
  * Django
* **Programming Language**
  * Python
  * Bash
  * HTML
  * CSS
  * JavaScript
* **Database**
  * PostgreSQL

* * *

### Application Usage and Backend Explanation

#### Login and Authorization

  ![](/demo/DeliveryTool/login_gerrit.png)

  When a user first comes onto the delivery tool webpage, they are greeted with a
  login page. The page is authorized using their JIRA login credential. They would
  also need to grant Gerrit permission in order for the tool to access the git
  repository under their name.

  * Benefits
    * Show user tickets assigned to them
    * Keep track of which ticket user has submitted
    * Each user's git environment is sandbox
    * Can submit to SCM under their name


#### Home Page

  ![](/demo/DeliveryTool/home_combined.png)

  Upon coming to the home screen, the user is greeted with a page that shows them all the
  JIRA tickets that are assigned to them. They can also search for tickets that are not
  assigned to them for which they wish to deliver.

  Tickets here are designed to be minimalistic. It is color coded to show the stages
  in which the ticket is currently in. Orange means the ticket still require
  user to complete it. Green means ticket are ready to be delivered to SCM. Yellow
  means ticket are passed the delivery stage and needs no further action. Here,
  the user can only select the green tickets because those are the only one that
  can be delivered.

  When a user hovers over the ticket, they can see further information such as the
  ticket's assignee, brief description, current stage, and link to that ticket's
  JIRA webpage.


#### Selecting Tickets For Delivery

  ![](/demo/DeliveryTool/build_combined.png)

  When a user selects a ticket to delivery, they would have to choose which branch
  and build type they would like to deliver the ticket to. Upon selecting the
  available options from the dropdown menu, the user is shown a confirmation page
  with their selected choices, and a proposed tag for the commit.

  The ticket the user have selected is automatically chosen. They are also shown
  tickets that have been committed since the last tag and asked if they would like
  to deliver those tickets as well.

  Upon submitting the delivery request, the tool will keep track that the ticket
  is currently in queue for delivery by the current user. If a different user
  search for the same ticket and try to deliver it, they will be notified that
  the ticket is is already in queue for delivery or have been delivered to SCM.


#### Status Page

  ![](/demo/DeliveryTool/status1.png)

  The status page is where the user can see a simplified version of the stages
  in which the ticket they have requested to deliver is located. Yellow indicates
  that the ticket is still in the process of being delivered. Green means the
  ticket has successfully been delivered to SCM. Red indicate that the delivery
  process has failed, as well as in which stage the failure happened.

  The tool will also email the user a detailed log of the entire process upon
  successfully or unsuccessfully delivering their selected tickets.
