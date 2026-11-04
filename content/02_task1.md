# Task 1

## MAIN OBJECTS, CLASSES, ATTRIBUTES AND METHODS

The main objects of the Secure Feedback Platform are grouped into classes according to their common properties and behaviours. Attributes represent the state of an object, while methods represent the operations it can perform.

### User Class

**Example Objects:** Student, Administrator

**Attributes (State):**

- `userId`
- `name`
- `email`
- `role`
- `isAuthenticated`

**Methods (Behaviour):**

- `login()`
- `logout()`
- `authenticate()`
- `checkPermission()`

### FeedbackInstance Class

**Example Objects:** Semester Feedback, Course Feedback

**Attributes (State):**

- `instanceId`
- `title`
- `description`
- `status`
- `createdAt`

**Methods (Behaviour):**

- `createFeedbackSession()`
- `updateFeedbackSession()`
- `activateSession()`
- `closeSession()`

### CourseOffering Class

**Example Objects:** Mathematics Course, Computer Networks Course

**Attributes (State):**

- `courseId`
- `courseName`
- `courseCode`
- `facultyId`

**Methods (Behaviour):**

- `addCourse()`
- `updateCourse()`
- `assignFaculty()`
- `getCourseDetails()`

### Faculty Class

**Example Objects:** Faculty Member 1, Faculty Member 2

**Attributes (State):**

- `facultyId`
- `facultyName`
- `department`
- `email`

**Methods (Behaviour):**

- `addFaculty()`
- `updateFacultyDetails()`
- `getFacultyDetails()`

### StudentAccessCode Class

**Example Objects:** Access Code 101, Access Code 102

**Attributes (State):**

- `codeId`
- `codeValue`
- `isUsed`
- `createdAt`
- `usedAt`

**Methods (Behaviour):**

- `generateCode()`
- `validateCode()`
- `claimCode()`
- `consumeCode()`

### FeedbackSubmission Class

**Example Objects:** Submission 1, Submission 2

**Attributes (State):**

- `submissionId`
- `instanceId`
- `submittedAt`
- `submissionStatus`

**Methods (Behaviour):**

- `submitFeedback()`
- `validateSubmission()`
- `saveSubmission()`
- `getSubmissionStatus()`

### FeedbackResponse Class

**Example Objects:** Rating Response, Comment Response

**Attributes (State):**

- `responseId`
- `questionId`
- `rating`
- `comments`

**Methods (Behaviour):**

- `recordResponse()`
- `validateResponse()`
- `updateResponse()`

### OfflineQueueItem Class

**Example Objects:** Pending Submission, Synchronized Submission

**Attributes (State):**

- `queueItemId`
- `submissionData`
- `status`
- `retryCount`
- `lastAttemptAt`

**Methods (Behaviour):**

- `saveLocally()`
- `markPending()`
- `markSynced()`
- `markInvalid()`
- `retrySync()`

### SyncService Class

**Example Objects:** Pending Queue Processor, Synchronization Process

**Attributes (State):**

- `isOnline`
- `pendingCount`
- `lastSyncTime`

**Methods (Behaviour):**

- `checkConnectivity()`
- `processQueue()`
- `synchronizeSubmission()`
- `handleSyncFailure()`

### RealtimeService Class

**Example Objects:** Feedback Update Service, Dashboard Notification Service

**Attributes (State):**

- `connectionStatus`
- `channelName`

**Methods (Behaviour):**

- `connect()`
- `publishFeedbackUpdate()`
- `notifyDashboard()`
- `disconnect()`

### Report Class

**Example Objects:** Feedback Summary, PDF Report

**Attributes (State):**

- `reportId`
- `feedbackInstanceId`
- `totalResponses`
- `averageRating`
- `generatedAt`

**Methods (Behaviour):**

- `generateReport()`
- `calculateSummary()`
- `exportPDF()`

## CRC CARDS

### CRC Card: User

**Responsibilities:**

- Authenticate users.
- Manage login sessions and user roles.

**Collaborators:**

- FeedbackInstance
- StudentAccessCode

### CRC Card: FeedbackInstance

**Responsibilities:**

- Create and manage feedback sessions.
- Activate and close feedback sessions.

**Collaborators:**

- User
- StudentAccessCode
- FeedbackSubmission

### CRC Card: StudentAccessCode

**Responsibilities:**

- Generate and validate access codes.
- Prevent reuse of access codes.

**Collaborators:**

- FeedbackInstance
- FeedbackSubmission

### CRC Card: FeedbackSubmission

**Responsibilities:**

- Validate and save feedback submissions.
- Maintain submission status.

**Collaborators:**

- StudentAccessCode
- FeedbackInstance
- FeedbackResponse
- SyncService

### CRC Card: FeedbackResponse

**Responsibilities:**

- Store individual feedback answers.
- Validate ratings and comments.

**Collaborators:**

- FeedbackSubmission
- Report

### Class Collaboration for One Use Case: Submit Anonymous Feedback

![Collaboration Diagram](assets/images/collaboration.jpg){width=6.5in height=4.5in}

1. The student enters an access code and completes the feedback form.
2. `FeedbackInstance` verifies that the feedback session is active.
3. `StudentAccessCode` validates that the code is valid and unused.
4. `FeedbackSubmission` processes and saves the submission.
5. `FeedbackResponse` records the individual answers.
6. The system saves the submission and consumes the code in a database transaction.
7. `RealtimeService` notifies the administrator dashboard after successful submission.

If the student is offline, the submission is stored locally and synchronized when connectivity returns.

## OBJECT-ORIENTED DESIGN PRINCIPLES

### Encapsulation

**Example:** `FeedbackSubmission`

The class groups submission data and related operations, such as `submitFeedback()` and `validateSubmission()`, into one unit.

### Information Hiding

**Example:** `StudentAccessCode`

The class hides the internal logic of code validation and consumption. Other classes interact through methods such as `validateCode()` and `consumeCode()`.

### Class Hierarchy

**Example:** `User`, `Student`, and `Administrator`

`Student` and `Administrator` can inherit common attributes and methods from `User`, while providing role-specific functionality.

### Polymorphism

**Example:** `displayDashboard()`

`Student` and `Administrator` can implement the same `displayDashboard()` method differently to display their respective dashboards.

### Aggregation or Containment

**Example:** `FeedbackSubmission` and `FeedbackResponse`

A `FeedbackSubmission` contains multiple `FeedbackResponse` objects representing the individual answers submitted in a feedback form.

\newpage
