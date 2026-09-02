
# Users

Title  
Log in to the system

User Story  
As a user, I need to log in with my credentials so that I can access the functions permitted for my role.

Acceptance Criteria

- Can log in with a registered email address and password
- Cannot log in with an unregistered email address or an incorrect password
- Is told that the credentials are invalid, without being told which field was wrong
- Is taken to the view appropriate to their role after logging in
- Password is masked as it is enter.
- GIVEN a user enters an incorrect password WHEN they submit the login form THEN a message advises them to check their credentials and no session is created
---
Title  
Log out of the system

User Story  
As an internal staff member, I need to log out of the system so that my account is not left accessible on a shared or mobile device used during venue setup.

Acceptance Criteria

- Can log out from any page in the system
    
- Is returned to the login page after logging out
    
- Cannot return to a protected page using the browser back button after logging out
    
- Is logged out automatically after a period of inactivity defined by ConnectSphere
    
---
Title  
Role-appropriate navigation

User Story  
As a user, I need to see only the functions available to my role so that I am not presented with actions I am not permitted to perform.

Acceptance Criteria

- Can see navigation options for the functions permitted to their role
    
- Cannot see navigation options for functions not permitted to their role
    
- Navigation is usable on both desktop and mobile screen sizes
    
- GIVEN a user's role is changed WHEN they next log in THEN the navigation reflects the functions permitted to their new role
    

---
Title  
Clear access-denied response

User Story  
As a user, I need to be told clearly when I do not have access to something so that I know to contact the right person rather than assume a system fault.

Acceptance Criteria

- Is shown a message stating that they do not have access when they attempt to reach a resource not permitted to them
    
- The message does not reveal whether the requested record exists or any of its details
    
- The message indicates who to contact to request access
    
- Can return to a permitted page from the access-denied message
    
**
# Equipment
**

TITLE: Update status of returned equipment

USER STORY: As a Technical Support Staff, I need to update the status of a piece of equipment so that I can release it for the follow up actions whether available, under maintenance etc

ACCEPTANCE CRITERIA:

- Can view a piece of equipment's current status
    
- Can change the status to one of the defined statuses (available, under maintenance, etc.)
    
- Only Technical Support Staff can change equipment status
    
- Updated status is immediately reflected wherever the equipment is displayed
    

---

TITLE: Reserve suitable equipment for an event request

USER STORY: As a Technical Support Staff, I need to see a list of suitable equipment for an event request so that I can reserve the available equipment.

ACCEPTANCE CRITERIA:

- Can view the equipment requirements recorded for an event request
    
- Can view possible equipment matches the requested type for the event's date and time
    
- List excludes equipment that is not available or already reserved for an overlapping period
    
- Each listed item shows its type, available quantity, location, and status
    
- Can reserve listed equipment against the event request, up to the available quantity
    

---

TITLE: Clarify requirements with Event Coordinator

USER STORY: As a Technical Support Staff, I need to highlight unmet equipment requirements to the Event Coordinator so that I can find out if the requirements will be changed

ACCEPTANCE CRITERIA:

- Can flag one or more equipment requirements of an event as unmet
    
- Must provide a reason when flagging a requirement as unmet
    
- Can optionally propose an alternative arrangement
    
- Flagged requirements and reasons are visible to the assigned Event Coordinator on the event record
    
- Assigned Event Coordinator is notified when a requirement is flagged
    

  
# Venue
**

|   |
|---|
|Title <br><br>Automatic Venue Availability Update upon Booking Confirmation|
|User Story<br><br>  <br><br>As a venue staff member, I want the system to automatically mark a venue as unavailable once a booking is confirmed, so that we prevent double bookings.|
|Acceptance Criteria<br><br>- Given there is a pending venue booking request for a specific room (e.g., "Seminar Room A"). <br>    <br>- The Venue Staff member approves and confirms the booking request.<br>    <br>- The system shall automatically set the venue's status for those dates and times to "Unavailable" in the Venue Availability Calendar.<br>    <br>- The system shall block any other booking requests from being submitted for the same venue during the overlapping time slot.|

  

|   |
|---|
|Title <br><br>Cancel Venue Booking and Free Up Availability|
|User Story<br><br>  <br><br>As a venue staff member, I want to cancel a venue booking, so that the venue’s time slot is immediately freed up for other event coordinators to book.|
|Acceptance Criteria<br><br>- Venue staff is notified that the event coordinator wants to cancel the booking.<br>    <br>- Venue staff goes to that booking and changes status from “Confirmed” to "Cancelled".<br>    <br>- The Venue Availability Calendar must immediately update the venue's status back to "Available" for that date and time range.|

  

|   |
|---|
|Title <br><br>Update Venue Specifications in Catalogue|
|User Story<br><br>  <br><br>As a venue staff member, I want to update a venue’s details in the catalogue, so that the event coordinator always search and filter based on accurate specifications.|
|Acceptance Criteria<br><br>- Venue Staff views a specific venue's profile in the Venue Catalogue.<br>    <br><br>- They can edit and save changes to fields including maximum capacity, supported layouts, accessibility features, and available facilities.<br>    <br>- The updated details must immediately be reflected on the Venue Catalogue.|

  

|   |
|---|
|Title <br><br>Indicate venue is unavailable due to maintenance|
|User Story<br><br>  <br><br>As a venue staff member, I want to block a venue’s availability for operational reasons such as maintenance or renovation, so that coordinators cannot request bookings during those periods.|
|Acceptance Criteria<br><br>- Venue Staff is on the Venue Unavailability Management screen.<br>    <br>- They create an unavailability block specifying a venue, start date/time, end date/time, and reason (e.g., "Maintenance").<br>    <br>- Then the Venue Availability Calendar must display this period as "Blocked".<br>    <br>- The system must prevent Event Coordinators from completing a booking request for this venue if it overlaps with the blocked maintenance period.|

  
  

|   |
|---|
|Title <br><br>Review and Decide on Venue Booking Requests|
|User Story<br><br>  <br><br>As a venue staff member, I want to review pending booking requests and provide approval or rejection details, so that coordinators are clear on the outcome of their requests.|
|Acceptance Criteria<br><br>- The venue staff gets notified of a new "Venue Booking Request" submitted by an event coordinator.<br>    <br>- The Venue Staff reviews the request and can indicate whether to approve or reject it.<br>    <br>- If rejected, they can input a reason for rejection or suggest an alternative arrangement.<br>    <br>- System will change the status of the booking request accordingly- either ‘Approved’ or ‘Reject’<br>    <br>- System notifies the Event Coordinator the result of their request.|

  

|   |
|---|
|Title <br><br>  <br><br>Configure Venue Turnaround and Setup Buffer|
|User Story<br><br>  <br><br>As a venue staff member, I want to review pending booking requests and provide approval or rejection details, so that coordinators are clear on the outcome of their requests.|
|Acceptance Criteria<br><br>- The venue staff gets notified of a new "Venue Booking Request" submitted by an event coordinator.<br>    <br>- The Venue Staff reviews the request and can indicate whether to approve or reject it.<br>    <br>- If rejected, they can input a reason for rejection or suggest an alternative arrangement.<br>    <br>- System will change the status of the booking request accordingly- either ‘Approved’ or ‘Reject’<br>    <br>- System notifies the Event Coordinator the result of their request.|

  
  
**
## Registration
**
**

|   |   |
|---|---|
|Title|Register for an event|
|User Story|As an event attendee, I want to register for an event so that I can secure my place and receive event information|
|Acceptance Criteria|- Given that an event is not confirmed, when I search for it, the event is unavailable for registration <br>    <br>- Given that an event is confirmed, within registration period and event capacity, when I submit a valid registration then a registration is created and shown to me <br>    <br>- Receive event confirmation details (event name, date, time, venue) upon successful registration <br>    <br>- Registration is refused if mandatory fields are missing or invalid|

  

|   |   |
|---|---|
|Title|Enable registration for an event|
|User Story|As an event coordinator, I want to enable/disable registration for an event so that only events that require attendee sign-up collect registration|
|Acceptance Criteria|- Given I am the Event Coordinator assigned to an event, when I open its registration settings, then I can set registration to enabled or disabled.<br>    <br>- Given registration is disabled, when an Attendee views the event, then they cannot register for the event<br>    <br>- Given I am the Event Coordinator, when the change is saved, then the actor, the old value, the new value and the timestamp are written to the activity history.|

  

|   |   |
|---|---|
|Title|Set registration capacity for an event|
|User Story|As an Event Coordinator, I want to set the maximum number of registrations for an event, so that sign-ups stay within what the venue and programme can accommodate to|
|Acceptance Criteria|1. Given registration is enabled, when I enter a capacity, then the value is stored and shown on the event record.<br><br>2. Given the event has a confirmed venue, when I enter a capacity above the venue's maximum capacity, then the system warns me before saving.<br><br>3. Given a capacity is set, when I attempt to lower it below the current number of confirmed registrations, then the system warns me and requires confirmation.<br><br>4. Given the capacity is reached, when the Coordinator views the event, then the registration count and remaining places are visible.<br><br>5. Given a capacity value is changed, when it is saved, then the change is recorded in the change history.|

  
**
  
**
## Event

|   |
|---|
|Title <br><br>  <br><br>Assignment of event requests|
|User Story<br><br>  <br><br>As an Event Operations Manager, I want to see all pending event requests, so I can assign an Event Coordinator to each event.|
|Acceptance Criteria<br><br>  <br><br>Event Operations Manager should:<br><br>- Receive a notification that a new event has been requested<br>    <br>- Be able to see all the pending event requests<br>    <br>- Be able to see all the Event Coordinators that are eligible for assignment<br>    <br>- Be able assign an Event Coordinator to an event<br>    <br>- Be informed if assignment of Event Coordinator has failed|

  
  

|   |
|---|
|Title <br><br>  <br><br>Submission of event requests|
|User Story<br><br>  <br><br>As an Event Organiser, I want to be able to indicate the initial event requirements, so I can successfully submit an event request.|
|Acceptance Criteria<br><br>  <br><br>Event Organiser is able to:<br><br>- Fill in necessary information for their event<br>    <br>- Submit an event request when they have finished filling in the necessary information  <br>      <br>    <br><br>  <br><br>- Given invalid or incomplete information when the Event Organiser submits an event request, then they should be informed of exactly what is missing or invalid<br>    <br>- Given complete and valid information when the Event Organiser submits an event request, then they should be informed that their event has been successfully requested|

  

  

|   |
|---|
|Title <br><br>  <br><br>Review and clarification of event requirements|
|User Story<br><br>  <br><br>As an Event Coordinator, I want to see the details of the event(s) I have been assigned, so I can determine if sufficient information has been provided.|
|Acceptance Criteria<br><br>  <br><br>Event Coordinator should:<br><br>- Receive a notification when an event has been assigned to him/her<br>    <br>- Be able to see the details and requirements of the event|

  
**