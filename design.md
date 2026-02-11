**Project Name:** Samjhao
**Team Name:** Desi Logic

# Design Document: Samjhao

## Overview

Samjhao is a web-based AI companion that demystifies complex academic and government documents for Indian students and parents. The system leverages Amazon Bedrock for intelligent document analysis, providing simplified explanations, eligibility determination, and actionable next steps—all in the user's preferred language (English, Hindi, or Tamil).

The architecture follows a client-server model with a stateless backend that orchestrates AI operations. No authentication or persistent storage is required for the MVP, keeping the system simple and accessible.

## System Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                     Frontend (Web Browser)                   │
│  - Document Input Interface                                  │
│  - Language/Mode Selection                                   │
│  - Question Flow UI                                          │
│  - Results Display                                           │
└────────────────────┬────────────────────────────────────────┘
                     │ HTTP/REST
                     ▼
┌─────────────────────────────────────────────────────────────┐
│                    Backend (Node.js/Python)                  │
│  - API Endpoints                                             │
│  - Request Validation                                        │
│  - Session State Management (in-memory)                      │
│  - AI Orchestration Logic                                    │
└────────────────────┬────────────────────────────────────────┘
                     │ AWS SDK
                     ▼
┌─────────────────────────────────────────────────────────────┐
│              Amazon Bedrock (AI Engine)                      │
│  - Document Analysis                                         │
│  - Explanation Generation                                    │
│  - Criteria Extraction                                       │
│  - Question Generation                                       │
│  - Eligibility Evaluation                                    │
│  - Action Plan Creation                                      │
└─────────────────────────────────────────────────────────────┘
```

## Technology Stack

| Layer | Technology | Purpose |
|-------|-----------|---------|
| Frontend | React/Vue.js | Responsive web UI |
| Backend | Node.js/Python | API server, AI orchestration |
| AI Engine | Amazon Bedrock | LLM-powered reasoning |
| Styling | Tailwind CSS | Responsive design |
| HTTP | REST API | Client-server communication |
| Deployment | AWS (Lambda/EC2) | Scalable hosting |

## Components and Interfaces

### Frontend Components

1. **DocumentInput Component**
   - Text area for document pasting
   - Character count display
   - Submit button with validation feedback
   - Error message display

2. **LanguageModeSelector Component**
   - Language radio buttons (English, Hindi, Tamil)
   - Mode radio buttons (Student, Parent)
   - Persistent selection during session

3. **ExplanationDisplay Component**
   - Formatted document explanation
   - Section headers and paragraphs
   - Clear typography for readability

4. **CriteriaList Component**
   - Numbered list of extracted eligibility criteria
   - Visual hierarchy for clarity

5. **QuestionFlow Component**
   - Question display with context
   - Input fields (text, radio, checkbox based on question type)
   - Progress indicator
   - Next/Previous navigation

6. **EligibilityResult Component**
   - Status badge (Eligible/Not Eligible/Partially Eligible)
   - Decision explanation with reasoning
   - Visual emphasis on key points

7. **ActionPlanDisplay Component**
   - Numbered steps with descriptions
   - Deadline information
   - Required documents list
   - Common mistakes warnings

### Backend API Endpoints

#### 1. POST /api/analyze-document
**Request:**
```json
{
  "document": "string (document text)",
  "language": "string (en|hi|ta)",
  "mode": "string (student|parent)"
}
```

**Response:**
```json
{
  "success": boolean,
  "explanation": "string (simplified explanation)",
  "criteria": [
    {
      "id": "string",
      "criterion": "string",
      "description": "string"
    }
  ],
  "questions": [
    {
      "id": "string",
      "question": "string",
      "type": "string (text|radio|checkbox)",
      "options": ["string"] (if applicable)
    }
  ],
  "error": "string (if applicable)"
}
```

#### 2. POST /api/evaluate-eligibility
**Request:**
```json
{
  "criteria": [
    {
      "id": "string",
      "criterion": "string"
    }
  ],
  "responses": [
    {
      "questionId": "string",
      "answer": "string|string[]"
    }
  ],
  "language": "string (en|hi|ta)",
  "mode": "string (student|parent)"
}
```

**Response:**
```json
{
  "success": boolean,
  "status": "string (eligible|not_eligible|partially_eligible)",
  "explanation": "string (detailed reasoning)",
  "actionPlan": [
    {
      "step": "number",
      "action": "string",
      "deadline": "string",
      "documents": ["string"],
      "mistakes": ["string"]
    }
  ],
  "error": "string (if applicable)"
}
```

#### 3. POST /api/generate-action-plan
**Request:**
```json
{
  "document": "string",
  "eligibilityStatus": "string (eligible|partially_eligible)",
  "language": "string (en|hi|ta)",
  "mode": "string (student|parent)"
}
```

**Response:**
```json
{
  "success": boolean,
  "actionPlan": [
    {
      "step": "number",
      "action": "string",
      "deadline": "string",
      "documents": ["string"],
      "mistakes": ["string"]
    }
  ],
  "error": "string (if applicable)"
}
```

## Data Flow

### Happy Path: Document Analysis to Action Plan

```
1. User Input
   ├─ Paste document text
   ├─ Select language (en|hi|ta)
   └─ Select mode (student|parent)

2. Frontend Validation
   ├─ Check document not empty
   └─ Send to /api/analyze-document

3. Backend Processing
   ├─ Validate inputs
   ├─ Call AI_Engine for analysis
   └─ Return explanation, criteria, questions

4. Question Flow
   ├─ Display questions to user
   ├─ Collect responses
   └─ Send to /api/evaluate-eligibility

5. Eligibility Determination
   ├─ AI_Engine evaluates responses against criteria
   ├─ Determine status (eligible/not_eligible/partially_eligible)
   └─ Generate explanation

6. Action Plan (if eligible/partially eligible)
   ├─ Call /api/generate-action-plan
   ├─ AI_Engine creates step-by-step plan
   └─ Display to user

7. Session End
   └─ Clear all session data
```

## Design Principles

### 1. Simplicity
- Minimal UI with clear navigation
- One task per screen when possible
- Plain language explanations
- No jargon or technical terms

### 2. Accessibility
- High contrast text and backgrounds
- Large, readable fonts
- Mobile-first responsive design
- Touch-friendly interactive elements

### 3. Low-Bandwidth Optimization
- Minimal CSS and JavaScript
- Compress API responses
- Lazy load content where possible
- Optimize image sizes

### 4. Responsible AI
- Clear indication that AI is being used
- Transparency about limitations
- No guarantees of eligibility (advisory only)
- Encourage verification with official sources

### 5. Explainability
- Show reasoning for eligibility decisions
- Display extracted criteria clearly
- Explain why questions are being asked
- Provide context for action plan steps

## Error Handling

### Document Validation Errors
- Empty document: "Please paste a document to analyze"
- Non-document content: "This doesn't appear to be an academic or government document. Please try another document."
- Unsupported language: "The document language is not supported. Please provide documents in English, Hindi, or Tamil."

### AI Processing Errors
- Extraction failure: "We couldn't extract clear eligibility criteria from this document. Please try a different document."
- Analysis timeout: "The analysis is taking longer than expected. Please try again."
- Service unavailable: "The AI service is temporarily unavailable. Please try again later."

### User Input Errors
- Incomplete responses: "Please answer all questions before proceeding."
- Invalid input format: "Please provide a valid response to this question."

## Testing Strategy

### Unit Testing
- Document validation logic
- Input sanitization
- Response formatting
- Error message generation
- Language selection persistence

### Property-Based Testing
- For each correctness property defined below, implement a property-based test
- Minimum 100 iterations per property test
- Use fast-check (JavaScript) or Hypothesis (Python)
- Tag each test with: **Feature: karl-simplifies, Property {N}: {property_text}**

### Integration Testing
- End-to-end document analysis flow
- Question flow with various response types
- Eligibility determination accuracy
- Action plan generation completeness

### Manual Testing
- UI responsiveness on various devices
- Language output quality (spot-check translations)
- Error message clarity
- Accessibility with screen readers

**Note:** Property-based testing and correctness properties are aspirational design goals and may be partially implemented in the hackathon prototype.

## Correctness Properties

A property is a characteristic or behavior that should hold true across all valid executions of a system—essentially, a formal statement about what the system should do. Properties serve as the bridge between human-readable specifications and machine-verifiable correctness guarantees.

### Property 1: Document Explanation Completeness
*For any* valid academic or government document and selected language/mode, the generated explanation SHALL contain a summary of the document's main purpose and key sections.
**Validates: Requirements 3.1, 3.2, 3.3, 3.4, 3.5**

### Property 2: Criteria Extraction Consistency
*For any* document with extractable eligibility criteria, the extracted criteria list SHALL be non-empty and each criterion SHALL be a specific, measurable condition.
**Validates: Requirements 4.1, 4.2, 4.3, 4.4, 4.5**

### Property 3: Question Generation Completeness
*For any* set of extracted eligibility criteria, the system SHALL generate at least one clarifying question for each criterion.
**Validates: Requirements 5.1, 5.2, 5.3, 5.4, 5.5**

### Property 4: Eligibility Decision Determinism
*For any* set of user responses to eligibility questions, the eligibility determination SHALL be consistent and deterministic (same responses always produce the same decision).
**Validates: Requirements 6.1, 6.2, 6.3, 6.4, 6.5**

### Property 5: Action Plan Completeness
*For any* eligible or partially eligible user, the generated action plan SHALL contain at least one step with action description, deadline, required documents, and common mistakes.
**Validates: Requirements 7.1, 7.2, 7.3, 7.4, 7.5, 7.6, 7.7**

### Property 6: Language Consistency
*For any* selected language (English, Hindi, Tamil), all system outputs (explanation, questions, decisions, action plan) SHALL be in the selected language.
**Validates: Requirements 2.1, 2.2, 2.3, 2.4, 2.5**

### Property 7: Error Message Clarity
*For any* error condition (invalid document, extraction failure, incomplete responses), the system SHALL return a user-friendly error message in the selected language that explains the issue.
**Validates: Requirements 8.1, 8.2, 8.3, 8.4, 8.5**

### Property 8: Session Data Preservation
*For any* user session, all entered data (document, language, mode, responses) SHALL be preserved throughout the session until the session ends.
**Validates: Requirements 10.1, 10.2, 10.3, 10.4, 10.5**

### Property 9: Responsive Layout Adaptation
*For any* screen size (mobile, tablet, desktop), the Frontend SHALL adapt layout and text size appropriately without horizontal scrolling.
**Validates: Requirements 9.1, 9.2, 9.3, 9.4**

