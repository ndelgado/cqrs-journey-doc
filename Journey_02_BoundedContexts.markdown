## Chapter 2
# Decomposing the Domain 

*Planning the journey: What will be the main stopping places along the way?*

# Definitions Used in This Chapter 

The following definitions are used for the remainder of this chapter. 
For more detail, and possible alternative definitions see [CQRS in 
Context][r_chapter1] in the Reference Guide. 

## Domain 

The **Domain** refers to the business domain for the Contoso Conference 
Management System (the reference implmentation). The previous chapter 
[The Contoso Conference Management System][j_chapter1] provides an 
overview of this domain. 

## Bounded Context 

The term **Bounded Context** comes from the book "Domain-Driven Design: 
Tackling Complexity in the Heart of Software" by Eric Evans. In brief, 
this concept is introduced as a way to decompose a large, complex system 
into more manageable pieces: a large system is composed of multiple 
bounded contexts. Each bounded context is the context for its own 
self-contained domain model, and has its own **Ubiquitous Language**.
You can also view a bounded context as autonomous business component 
defining clear consistency boundaries: one bounded context typically 
communicates with another bounded context by raising events.

> **BharathPersona:** When you use the CQRS pattern, you often use
> events to communicate between bounded contexts. There are alternative
> approaches to integration such as sharing data at the database level.

## Context Map

According to Eric Evans in "Domain-Driven Design: Tackling Complexity in
the Heart of Software" you should:

> Describe the points of contact between the models, outlining explicit
> translation for any communication and highlighting any sharing.

This **Context Map** serves several purposes that include providing 
an overview of the whole system and helping people to understand the 
details of how different bounded contexts interact with each other. 

# Bounded Contexts in the Conference Management System 

## The Orders and Registrations Bounded Context

When a registrant interacts with the system, the system creates an 
**Order** to manage the **Reservations**, payment, and 
**Registrations**. An Order contains one of more Order Items. 

A **Reservation** is a temporary reservation of one or more seats at 
conference. When a registrant begins the ordering process to purchase a 
number of seats at conference, the system creates **Reservations** for 
the number of seats requested by the registrant. These seats are then 
not available for other registrants to reserve. The **Reservations** are 
held for _n_ minutes (_n_ is one of the properties of the conference 
defined by the **Business Customer**) during which the registrant can 
complete the ordering process by making a payment for those seats. If 
the registrant does not pay for the tickets within _n_ minutes, the 
system deletes the **Reservation** and the seats become available to 
other registrants to reserve. 

<div style="margin-left:20px;margin-right:20px;">
  <span style="background-color:yellow;">
    <b>Comment [DRB]:</b>
    At the moment the value of _n_ is hardcoded - this may change in the future and become a parameter than can be set by the Business Customer.
  </span>
</div>

## The Conference Management Bounded Context

A business customer can create new conferences and manage them. After a 
business customer creates a new conference, he can access the details of 
the conference by using his email address and conference locator access 
code. The system generates the access code when the business customer 
creates the conference. 

The business customer can specify the following information about a 
conference: 

* The name, description, and slug (part of the URL used to access the
  conference).
* The start and end dates of the conference.
* The different types and quotas of seats available at the conference.

Additionally, the business customer can control the visibility of the 
conference on the public web-site by either publishing or un-publishing 
the conference. 

The business customer can use the conference management web-site to view 
a list of orders and attendees. 

## The On-site Client Application Bounded Context 

## The Payments Bounded Context 

The Payments bounded context is responsible for managing the 
interactions between the Conference Management System and external 
payments systems. It forwards the necessary payment information to the 
external system and receives an acknowledgement that the payment was 
either accepted or rejected. It reports the success or failure of the 
payment back to the Conference Management System. 

Initially, the Payments bounded context will assume that the Business 
Customer has an account with the third-party payment system (although 
not necessarily a merchant account), or that the Business Customer will 
accept payment by invoice. 

## The Conference Feedback Bounded Context

## The Context Map for the Contoso Conference Management System

Figure 1 and the table that follows it provides a **Context Map** that 
shows the relationships between the different bounded contexts that make 
the complete system and as such it provides a high-level overview of how 
the system is put together. 

> **BharathPersona:** A frequent comment about CQRS projects is the
> difficulty in understanding how all of the pieces fit together,
> especially if there is a large number of commands and events in the
> system. Often, you can perform some static analysis on the code to
> determine where events and commands are handled, but it is more
> difficult to automatically determine where they originate. At a
> high-level, a context map can help to understand the the integration
> between the different bounded contexts and the events involved.
> Maintaining up to date documentation about the commands and events can
> offer a more detailed view. Additionally, if you have tests that use
> commands as inputs and then check for events, you can examine the
> tests to understand the expected consequences of particular commands
> (see the section on testing in [Extending and Enhancing the Orders and
> Registrations Bounded Contexts][j_chapter4] for an example of this
> style of test.

Figure 1 shows the six bounded contexts that make up the Contoso 
Conference Management System. The arrows on the diagram indicate the 
flow of data as events between them. 

![Figure 1][fig1]

**Bounded Contexts in the Contoso Conference Management System**

The following list provides more information about the arrows in figure 1. You can find additional detail in the chapters that discuss the individual bounded contexts. 

1. Events that report when conferences have been created, updated, and
   published. Events that report when seat types have been created or
   updated.
2. Events that report when orders have been created and updated. Events
   that report when attendees have been assigned to seats.
3. Requests for a payment to be made.
4. Acknowledgement of the success or failure of the payment.
5. Events that report when conferences have been created, updated, and
   published. Events that report when seat types have been created or
   updated.
6. Events that report when orders have been created and updated.
7. Events that report when orders have been created and updated.
8. Events that report when discounts have been created or modified.
9. Events that report the calculation of totals based on discounts.
10. Events that report when conferences have been created, updated, and
   published.

> **BharathPersona:** Some of the events raised from the Conference
> Management bounded context are coarse-grained and contain multiple
> fields. Remember that Conference Management is a CRUD-style bounded
> context and does not raise fine-grained domain-style events. For more
> information, see
> [Chapter 5, Preparing for the V1 Release][j_chapter5]

# Why Did We Choose These Bounded Contexts? 


[j_chapter1]:     Journey_01_Introduction.markdown
[j_chapter4]:     Journey_04_ExtendingEnhancing.markdown
[j_chapter5]:     Journey_05_PaymentsBC.markdown
[r_chapter1]:     Reference_01_CQRSInContext.markdown

[fig1]:           images/Journey_02_BCs.png?raw=true