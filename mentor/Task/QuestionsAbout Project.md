# Mentor Application – 5-Minute Technical Interview Preparation

## Project Details

| Category | Details |
|---|---|
| Project Name | Mentor Application |
| Application Type | Web Application |
| Programming Language | C# |
| Backend | ASP.NET Core |
| Frontend | ASP.NET Core MVC / Razor |
| UI Design | HTML, CSS, Bootstrap |
| Database Connectivity | Entity Framework Core |
| Database | SQL Server |
| Authentication | ASP.NET Core Identity, Google / Facebook OAuth |
| Email | SMTP / Email Service |
| Architecture | Layered Architecture |
| Development Tool | Visual Studio |

---

# 1. How to Explain Your Project in a Technical Interview

When an interviewer asks:

**"Tell me about your project."**

You should explain your project in the following order.

| Duration | Topic |
|---|---|
| 30 seconds | Project Introduction |
| 45 seconds | Problem Statement |
| 45 seconds | Application Architecture |
| 45 seconds | Modules and Features |
| 60 seconds | Technical Implementation |
| 45 seconds | Database Design |
| 30 seconds | Challenges and Conclusion |
| **Total** | **5 Minutes** |

---

# 2. Five-Minute Interview Explanation

## Part 1: Project Introduction (30 Seconds)

**Interviewer: Tell me about your project.**

**Student Answer:**

"My project is a Mentor Application developed using ASP.NET Core, C#, Entity Framework Core, ASP.NET Core Identity, and SQL Server.

It is a web platform where users can create an account, become mentors, define their availability, search for mentors, and book appointments.

The main objective of this application is to make it easy to find a suitable mentor and arrange a meeting without exchanging manual messages.

I was involved in developing the application, designing the UI, implementing the booking logic, and integrating the database and email service."

## Part 2: Problem Statement (45 Seconds)

**Interviewer: What problem does your project solve?**

**Student Answer:**

"Let me explain with a real-time example.

Imagine a student who wants guidance on ASP.NET Core.

Without a platform, the student has to find a mentor, ask about their free time through messages, and wait for a reply.

Mentors may miss requests, and two students may ask for the same time.

For example, assume a mentor has only one free slot on Monday at 10:00 AM.

If two users try to book that slot at the same time, the system should not allow both bookings.

To solve this problem, we developed a centralized application where mentors publish their availability, users see only the available slots, and the system prevents double booking.

Users and mentors also receive email notifications, so nobody misses an appointment."

## Part 3: Application Architecture (45 Seconds)

**Interviewer: Explain your application architecture.**

**Student Answer:**

"We designed the application using a layered architecture.

We divided the application into four main layers.

1. Presentation Layer – ASP.NET Core MVC controllers and Razor views with Bootstrap are used for the user interface.
2. Business Layer – Contains booking rules, slot calculation, mentor validation, and email logic.
3. Data Access Layer – Uses Entity Framework Core to communicate with SQL Server.
4. Database Layer – SQL Server stores users, mentor profiles, skills, availability, appointments, and email logs.

ASP.NET Core Identity handles registration, login, and password hashing, and Google and Facebook OAuth provide social login.

This separation makes the application easier to maintain, test, and extend.

All users access the same central database through the web application."

### Architecture Diagram

```text
+---------------------------+
|     Presentation Layer    |
|  ASP.NET Core MVC / Razor |
|   HTML / CSS / Bootstrap  |
+---------------------------+
             |
             v
+---------------------------+
|       Business Layer      |
|  Booking / Slot Logic     |
|  Mentor / Email Services  |
+---------------------------+
             |
             v
+---------------------------+
|      Data Access Layer    |
|    Entity Framework Core  |
|  DbContext / Repositories |
+---------------------------+
             |
             v
+---------------------------+
|       Database Layer      |
|        SQL Server         |
+---------------------------+
```

## Part 4: Application Modules (45 Seconds)

**Interviewer: What are the main modules in your application?**

**Student Answer:**

"Our application has two main user roles.

### 1. User Module

A user can:

- Register and log in using email, Google, or Facebook.
- Manage the profile.
- Search mentors by name, skill, technology, or title.
- View mentor details and availability.
- Book an appointment.
- View and cancel their own appointments.
- Receive email confirmations.

### 2. Mentor Module

Any user can enable mentor mode. A mentor can:

- Create a mentor profile.
- Add skills.
- Define weekly availability.
- View appointments booked with them.
- Confirm or reject appointment requests.

A mentor is simply a user with IsMentor set to true.

We implemented ownership checks so that users can see only their own appointments, and only the mentor of an appointment can confirm or reject it."

## Part 5: Technical Implementation (60 Seconds)

**Interviewer: Explain how booking works internally.**

**Student Answer:**

"When a user opens a mentor's profile and selects a date, the application finds the mentor's availability for that day of the week.

For example, if the selected date is Monday and the mentor is available from 9:00 AM to 11:00 AM, the application creates the time slots inside that window.

Then it removes the slots that already have a Pending or Confirmed appointment on that date.

The remaining slots are shown to the user.

The user selects a slot, enters a message, and clicks Book Appointment.

The application validates again in the database that the slot is still free and that the user is not booking their own mentor profile.

Then we save the appointment with the Pending status and send emails to both the user and the mentor.

As the final protection against double booking, we created a filtered unique index on mentor, date, and start time for active appointments.

If two users book the same slot at the same time, the second save fails with a unique index error, and we show a message that the slot is no longer available.

When the mentor confirms or rejects the request, the status is updated and the user receives an email."

### Booking Workflow

```text
User Login
     |
     v
Find Mentor
     |
     v
Search by Name / Skill / Title
     |
     v
View Mentor Details
     |
     v
Select Date
     |
     v
Show Available Time Slots
     |
     v
Select Time and Enter Message
     |
     v
Book Appointment
     |
     v
Validate Slot and Self Booking
     |
     v
Save Appointment (Pending)
     |
     v
Send Email to User and Mentor
     |
     v
Mentor Confirms or Rejects
```

## Part 6: Database Design (45 Seconds)

**Interviewer: How many tables did you create?**

**Student Answer:**

"We designed ten main database tables.

| Table Name | Purpose |
|---|---|
| Roles | Stores application roles |
| Users | Stores user accounts and profile information |
| UserRoles | Stores the roles assigned to users |
| MentorProfiles | Stores mentor-specific information |
| Skills | Stores the list of skills |
| MentorSkills | Stores the skills of each mentor |
| MentorAvailabilities | Stores mentor weekly availability |
| Appointments | Stores appointment bookings |
| ExternalLogins | Stores Google and Facebook login links |
| EmailLogs | Stores email notification history |

We established primary key and foreign key relationships between these tables.

For example, one mentor can have multiple availability records and multiple appointments. Therefore, MentorProfiles has a one-to-many relationship with MentorAvailabilities and Appointments.

Mentors and skills have a many-to-many relationship, so we used the MentorSkills table as the link table."

### Important Database Relationships

```text
Users
  |
  | 1 : 1
  |
MentorProfiles
  |
  |-- 1 : Many --> MentorAvailabilities
  |
  |-- 1 : Many --> Appointments <-- Many : 1 -- Users
  |
  |-- Many : Many (through MentorSkills) --> Skills
```

## Part 7: Technical Challenges and Conclusion (30 Seconds)

**Interviewer: What challenges did you face during development?**

**Student Answer:**

"One important challenge was preventing double booking when multiple users were booking the same mentor at the same time.

We addressed this using a slot check before saving and a filtered unique index in SQL Server as the final protection.

Another challenge was calculating the available slots from the mentor's weekly availability and the existing appointments.

We also worked on configuring Google and Facebook login, protecting secrets such as client IDs and SMTP passwords, and handling email failures without breaking the booking.

Through this project, I gained practical experience in ASP.NET Core, Entity Framework Core, Identity, SQL Server, database relationships, and real-world application development."

---

# 3. Important Technical Interview Questions and Answers

Students should prepare these questions after their project explanation.

## A. ASP.NET Core and C# Questions

### 1. What is ASP.NET Core?

ASP.NET Core is a cross-platform, high-performance framework from Microsoft used to build web applications and APIs.

### 2. What is MVC?

MVC stands for Model-View-Controller.

- Model: Represents application data and business rules.
- View: Represents the UI (Razor views).
- Controller: Handles the request, calls the services, and returns a view or result.

### 3. What is the difference between a Controller and a View?

- A Controller receives the request, processes it, and decides what to return.
- A View displays the data to the user.

For example, `MentorController` gets the mentor list, and `Index.cshtml` displays it.

### 4. What is Razor?

Razor is a markup syntax used to write C# code inside HTML in `.cshtml` files. It is used to generate dynamic web pages on the server.

### 5. What is Dependency Injection?

Dependency Injection (DI) is a design pattern where a class receives the objects it needs instead of creating them itself.

For example, a controller receives `IAppointmentService` through its constructor, and the service is registered in `Program.cs`. This makes the application easier to test and maintain.

### 6. What is Middleware?

Middleware is a component in the request pipeline that handles requests and responses. Examples are HTTPS redirection, static files, authentication, and authorization.

### 7. What is the purpose of Program.cs?

`Program.cs` is the entry point of the application. It configures services such as EF Core, Identity, and email, and configures the middleware pipeline and routing.

### 8. What is the difference between Authentication and Authorization?

- Authentication verifies who the user is (login).
- Authorization decides what the user is allowed to do (for example, only the mentor can confirm an appointment).

### 9. What is an interface?

An interface defines a contract that a class must implement. We use interfaces such as `IEmailService` so that the implementation can be replaced or mocked in tests.

### 10. What is an enum?

An enum is a set of named constants. We use an enum for appointment status: Pending, Confirmed, Rejected, Cancelled, and Completed.

### 11. What is the purpose of async and await?

`async` and `await` allow the application to perform long-running operations, such as database calls and sending emails, without blocking the thread. This helps the web server handle more requests.

### 12. What is the difference between IEnumerable and List?

- `IEnumerable<T>` represents a sequence that can be read (often lazily).
- `List<T>` is a concrete in-memory collection that supports adding, removing, and indexing.

## B. Entity Framework Core Questions

### 1. What is Entity Framework Core?

EF Core is an object-relational mapper (ORM). It allows us to work with the database using C# classes and LINQ instead of writing all SQL manually.

### 2. What is DbContext?

`DbContext` represents a session with the database. It is used to query data, track changes, and save changes.

### 3. What is DbSet?

`DbSet<T>` represents a table. For example, `DbSet<Appointment>` represents the Appointments table.

### 4. What is a Migration?

A migration is a generated file that describes changes to the database schema. It is used to create and update the database from the C# model.

### 5. What is the difference between Code First and Database First?

- Code First: We write the C# classes first and generate the database from them.
- Database First: We create the database first and generate the C# classes from it.

### 6. What is LINQ?

LINQ (Language Integrated Query) is used to query collections and databases using C# syntax.

```csharp
var mentors = await _context.MentorProfiles
    .Where(m => m.IsAvailable &&
                m.MentorSkills.Any(ms => ms.Skill.Name == "C#"))
    .ToListAsync();
```

### 7. What is the purpose of SaveChangesAsync?

`SaveChangesAsync` writes all tracked changes to the database in a single transaction. If it fails, the changes are not saved.

### 8. What is the difference between eager loading and lazy loading?

- Eager loading loads related data in the same query using `Include`.
- Lazy loading loads related data only when it is accessed.

We use `Include` to load a mentor's skills and availability together.

## C. ASP.NET Core Identity and Security Questions

### 1. What is ASP.NET Core Identity?

It is a built-in membership system that provides registration, login, password hashing, roles, and external login support.

### 2. How are passwords stored?

Passwords are never stored as plain text. Identity stores a salted hash of the password, so the original password cannot be read from the database.

### 3. What is OAuth?

OAuth is an authorization standard that allows an application to let users sign in using another provider, such as Google or Facebook, without sharing their password with our application.

### 4. How does Google login work?

1. The user clicks Login with Google.
2. The application redirects the user to Google.
3. The user signs in and grants permission.
4. Google redirects back with the user's information.
5. The application creates or links the local account and signs the user in.

### 5. What is an anti-forgery token?

It is a token added to forms to protect against Cross-Site Request Forgery (CSRF) attacks. The server checks that the form was submitted from our own site.

### 6. What is the Authorize attribute?

`[Authorize]` restricts a controller or action to authenticated users. It can also require a role or policy.

### 7. How do you protect secrets such as client IDs and SMTP passwords?

We never hardcode them in source code or upload them to GitHub. In development we use User Secrets, and in production we use environment variables or a secure service such as Azure Key Vault.

### 8. How do you prevent a user from viewing another user's appointment?

We check ownership in the controller or service. The query filters appointments by the logged-in user's ID, and mentor actions verify that the appointment belongs to the logged-in mentor. Otherwise, access is denied.

### 9. Why is HTTPS required?

HTTPS encrypts the data between the browser and the server, which protects passwords, login cookies, and personal information.

## D. SQL Server Questions

### 1. What is a Primary Key?

A Primary Key uniquely identifies each row in a table. For example, `Appointments.Id`.

### 2. What is a Foreign Key?

A Foreign Key links one table to another and enforces referential integrity. For example, `Appointments.MentorId` references `MentorProfiles.Id`.

### 3. What is a Unique Constraint?

A Unique Constraint ensures that no two rows have the same value in a column. For example, user email must be unique.

### 4. What is an Index?

An index improves the speed of searching and filtering data. For example, an index on the skill name can speed up mentor searches.

### 5. What is a filtered unique index?

It is a unique index that applies only to the rows matching a condition.

In our project, it makes mentor, date, and start time unique only for Pending and Confirmed appointments. Cancelled and Rejected appointments do not block the slot.

```sql
CREATE UNIQUE INDEX UX_Appointments_NoDoubleBooking
ON Appointments (MentorId, AppointmentDate, StartTime)
WHERE Status IN ('Pending', 'Confirmed');
```

### 6. What is a Join?

A Join combines rows from two or more tables based on a related column. For example, joining Appointments and Users to display the student name.

### 7. What is a many-to-many relationship?

It is a relationship where many rows in one table relate to many rows in another. Mentors and skills are many-to-many, so we use the `MentorSkills` link table.

### 8. What is a Transaction?

A transaction is a group of operations that either all succeed or all fail. `SaveChangesAsync` uses a transaction for all changes it saves.

### 9. What are ACID properties?

- Atomicity: All operations succeed or none.
- Consistency: Data remains valid after the transaction.
- Isolation: Concurrent transactions do not interfere with each other.
- Durability: Committed data is permanently saved.

### 10. How do you prevent double booking?

We check the slot before saving, and we also use the filtered unique index. If two users book at the same time, only one insert succeeds.

## E. Real-Time Scenario Questions

### 1. What happens if two users book the same slot at the same time?

Both users may see the slot as available, but only the first insert succeeds. The second insert violates the unique index, the application catches the error, shows "Slot already booked", and reloads the available slots.

### 2. What happens if the email service is unavailable when an appointment is booked?

The appointment is still saved because booking is the main operation. The email failure is logged in the `EmailLogs` table with the Failed status, and the email can be retried later.

### 3. How do you prevent a mentor from booking themselves?

Before saving, we compare the logged-in user's ID with the `UserId` of the selected mentor profile. If they are the same, the booking is blocked.

### 4. How do you show only the available time slots?

1. Get the mentor's active availability for the selected day of the week.
2. Create the time slots inside those windows.
3. Remove the slots that already have a Pending or Confirmed appointment on that date.
4. Show the remaining slots.

### 5. What happens to the slot when an appointment is cancelled?

The status changes to Cancelled. Because the unique index applies only to Pending and Confirmed appointments, the slot becomes available again.

### 6. How do you search mentors by skill?

We join MentorProfiles, MentorSkills, and Skills, and filter by skill name, mentor name, or professional title using a LINQ query.

### 7. How do you handle a user who registers with email and later logs in with Google?

Both logins use the same email. We link the Google login to the existing account using the `ExternalLogins` data, so the user does not get a duplicate account.

### 8. How would you implement reviews and ratings?

We would add a `Reviews` table with AppointmentId, MentorId, UserId, Rating, Comment, and CreatedDate. A user could review a mentor only after the appointment status becomes Completed, and the mentor's average rating would be displayed on the mentor profile.

---

# 4. Quick Revision Points

- The project is a Mentor Booking web application using ASP.NET Core, EF Core, Identity, and SQL Server.
- There are two roles: User and Mentor. A mentor is a user with `IsMentor = true`.
- The application uses a layered architecture: Presentation, Business, Data Access, and Database.
- Available slots are calculated from the mentor's availability minus existing Pending and Confirmed appointments.
- Double booking is prevented by a slot check and a filtered unique index.
- Appointment statuses are Pending, Confirmed, Rejected, Cancelled, and Completed.
- Passwords are hashed by Identity, and secrets are kept in User Secrets or Key Vault.
- Emails are sent for new, confirmed, rejected, and cancelled appointments, and failures are logged.

---

**Project:** Mentor Application  
**Technologies:** C# | ASP.NET Core | Entity Framework Core | SQL Server  
**Architecture:** Layered Architecture  
**Project Type:** Web Application