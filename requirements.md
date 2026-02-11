**Project Name:** KARL Simplifies  
**Team Name:** Desi Logic

# Requirements Document: KARL Simplifies

## Introduction

KARL Simplifies is an AI-powered document companion designed to help students and parents in India understand complex academic and government documents. The system simplifies explanations in local languages (Hindi/Tamil), extracts eligibility criteria, determines user eligibility, and generates actionable plans with deadlines and required documents.

## Glossary

- **Document**: Academic or government text (scholarships, college notices, schemes) provided by the user
- **User**: Student or parent accessing the system
- **Mode**: User role context (Student or Parent) that influences explanation style
- **Language**: Output language preference (English, Hindi, or Tamil)
- **Eligibility Criteria**: Specific conditions extracted from a document that determine if a user qualifies
- **Eligibility Question**: A clarifying question asked to the user to determine if they meet extracted criteria
- **Eligibility Decision**: The system's determination of whether the user meets all criteria (Eligible, Not Eligible, or Partially Eligible)
- **Action Plan**: Step-by-step instructions with deadlines and required documents for the user to proceed
- **Simple Explanation**: Document content explained in plain language appropriate for the target user's education level
- **AI_Engine**: Amazon Bedrock service providing reasoning and language processing capabilities
- **Frontend**: Web application interface for user interaction
- **Backend**: Server handling API requests and orchestrating AI operations

## Requirements

### Requirement 1: Document Input and Processing

**User Story:** As a student or parent, I want to paste document text into the system, so that I can get help understanding it without uploading files.

#### Acceptance Criteria

1. WHEN a user accesses the application, THE Frontend SHALL display a text input area with placeholder text indicating "Paste your document here"
2. WHEN a user pastes text into the input area, THE Frontend SHALL accept and store the text without file upload
3. WHEN a user submits a document with empty or whitespace-only content, THE System SHALL reject the submission and display an error message
4. WHEN a user submits a valid document, THE Backend SHALL send the document to the AI_Engine for processing
5. WHEN the AI_Engine receives a document, THE AI_Engine SHALL validate that the content is recognizable as an academic or government document

### Requirement 2: Language and Mode Selection

**User Story:** As a student or parent, I want to select my preferred language and user mode, so that I receive explanations tailored to my needs.

#### Acceptance Criteria

1. WHEN the user accesses the application, THE Frontend SHALL display language selection options (English, Hindi, Tamil)
2. WHEN the user accesses the application, THE Frontend SHALL display mode selection options (Student, Parent)
3. WHEN a user selects a language and mode, THE Frontend SHALL store these preferences for the current session
4. WHEN a user submits a document, THE Backend SHALL use the selected language and mode to configure AI_Engine requests
5. WHEN the AI_Engine processes a document, THE AI_Engine SHALL generate all outputs in the selected language

### Requirement 3: Simple Document Explanation

**User Story:** As a student or parent, I want the system to explain complex documents in simple language, so that I can understand what the document is about.

#### Acceptance Criteria

1. WHEN the AI_Engine receives a document with selected language and mode, THE AI_Engine SHALL generate a simplified explanation
2. WHEN generating an explanation for Student mode, THE AI_Engine SHALL use vocabulary and examples appropriate for high school level understanding
3. WHEN generating an explanation for Parent mode, THE AI_Engine SHALL use vocabulary and examples appropriate for adult understanding
4. WHEN the AI_Engine generates an explanation, THE Backend SHALL return the explanation to the Frontend
5. WHEN the Frontend receives an explanation, THE Frontend SHALL display it in a readable format with clear sections

### Requirement 4: Eligibility Criteria Extraction

**User Story:** As a student or parent, I want the system to identify eligibility requirements from a document, so that I know what conditions I need to meet.

#### Acceptance Criteria

1. WHEN the AI_Engine processes a document, THE AI_Engine SHALL extract all eligibility criteria mentioned in the document
2. WHEN extracting criteria, THE AI_Engine SHALL identify specific conditions (age, income, academic performance, location, etc.)
3. WHEN the AI_Engine extracts criteria, THE Backend SHALL structure the criteria as a list of conditions
4. WHEN criteria are extracted, THE Backend SHALL return the structured criteria to the Frontend
5. WHEN the Frontend receives criteria, THE Frontend SHALL display them in a clear, numbered list format

### Requirement 5: Eligibility Question Flow

**User Story:** As a student or parent, I want to answer questions about my situation, so that the system can determine if I meet the eligibility criteria.

#### Acceptance Criteria

1. WHEN the Backend has extracted eligibility criteria, THE Backend SHALL generate clarifying questions for each criterion
2. WHEN generating questions, THE AI_Engine SHALL create questions in the selected language that are easy to understand
3. WHEN the Frontend displays questions, THE Frontend SHALL present them one at a time or in a grouped format
4. WHEN a user answers a question, THE Frontend SHALL capture and store the response
5. WHEN all questions are answered, THE Frontend SHALL send all responses to the Backend for eligibility determination

### Requirement 6: Eligibility Decision and Explanation

**User Story:** As a student or parent, I want to know if I'm eligible and why, so that I understand my status clearly.

#### Acceptance Criteria

1. WHEN the Backend receives all user responses, THE Backend SHALL send responses and criteria to the AI_Engine for evaluation
2. WHEN the AI_Engine evaluates responses, THE AI_Engine SHALL determine eligibility status (Eligible, Not Eligible, or Partially Eligible)
3. WHEN the AI_Engine determines eligibility, THE AI_Engine SHALL generate a clear explanation of the decision with specific reasons
4. WHEN the Backend receives the decision, THE Backend SHALL return the status and explanation to the Frontend
5. WHEN the Frontend displays the decision, THE Frontend SHALL highlight the status prominently and show the explanation clearly

### Requirement 7: Action Plan Generation

**User Story:** As a student or parent, I want a step-by-step action plan with deadlines and required documents, so that I know what to do next.

#### Acceptance Criteria

1. WHEN the user is determined to be Eligible or Partially Eligible, THE Backend SHALL request an action plan from the AI_Engine
2. WHEN generating an action plan, THE AI_Engine SHALL create numbered steps with specific actions
3. WHEN generating an action plan, THE AI_Engine SHALL include realistic deadlines for each step
4. WHEN generating an action plan, THE AI_Engine SHALL list required documents for each step
5. WHEN generating an action plan, THE AI_Engine SHALL include common mistakes to avoid for each step
6. WHEN the Backend receives the action plan, THE Backend SHALL return it to the Frontend
7. WHEN the Frontend displays the action plan, THE Frontend SHALL format it clearly with steps, deadlines, documents, and warnings

### Requirement 8: Error Handling for Unclear Documents

**User Story:** As a student or parent, I want clear error messages when the system cannot understand a document, so that I know what went wrong.

#### Acceptance Criteria

1. WHEN the AI_Engine receives a document that is not recognizable as an academic or government document, THE AI_Engine SHALL return an error status
2. WHEN the AI_Engine cannot extract meaningful eligibility criteria, THE AI_Engine SHALL return a specific error message
3. WHEN an error occurs, THE Backend SHALL return the error message to the Frontend
4. WHEN the Frontend receives an error, THE Frontend SHALL display a user-friendly message explaining the issue
5. WHEN an error occurs, THE Frontend SHALL provide guidance on what types of documents are supported

### Requirement 9: Responsive Web Interface

**User Story:** As a student or parent, I want to use the application on various devices, so that I can access it from my phone, tablet, or computer.

#### Acceptance Criteria

1. WHEN the Frontend is accessed on different screen sizes, THE Frontend SHALL adapt layout and text size appropriately
2. WHEN the Frontend is accessed on mobile devices, THE Frontend SHALL ensure all interactive elements are easily tappable
3. WHEN the Frontend is accessed on low-bandwidth connections, THE Frontend SHALL minimize data transfer and load times
4. WHEN the Frontend displays content, THE Frontend SHALL use clear typography and sufficient contrast for readability

### Requirement 10: Session Management

**User Story:** As a student or parent, I want my current work to be preserved during my session, so that I don't lose my progress.

#### Acceptance Criteria

1. WHEN a user submits a document, THE Frontend SHALL store the document text in the current session
2. WHEN a user selects language and mode, THE Frontend SHALL preserve these selections throughout the session
3. WHEN a user answers eligibility questions, THE Frontend SHALL store all responses in the current session
4. WHEN a user navigates between sections, THE Frontend SHALL maintain all previously entered data
5. WHEN a user closes the browser or session expires, THE Frontend SHALL clear all stored data (no persistence required)
