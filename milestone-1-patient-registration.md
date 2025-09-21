# Milestone 1: Patient Registration & AI Bot Assessment

## Overview
This milestone covers the initial patient onboarding process where patients register on the platform, describe their problems to an AI bot, and get their issues summarized and sent to a therapist for initial consultation.

## High-Level Flow

```mermaid
graph TD
    START[Patient Registration] --> REG[Register on Platform]
    REG --> AIBOT[AI Bot Problem Description]
    AIBOT --> SUMMARY[AI Bot Summarizes Problem]
    SUMMARY --> CONFIRM[Patient Confirms Problem]
    CONFIRM --> SEND[Send to Therapist]
    SEND --> MILESTONE1[Milestone 1 Complete]

    %% Styling
    classDef milestone fill:#ffecb3,stroke:#ff8f00,stroke-width:3px
    classDef patient fill:#f3e5f5,stroke:#4a148c,stroke-width:2px
    classDef ai fill:#e8f5e8,stroke:#1b5e20,stroke-width:2px
    classDef system fill:#fce4ec,stroke:#880e4f,stroke-width:2px

    class START,REG,CONFIRM patient
    class AIBOT,SUMMARY ai
    class SEND,MILESTONE1 system
```

## Detailed User Interactions

```mermaid
graph LR
    subgraph "Patient Actions"
        PAT[Patient] --> |Register| PLATFORM[Platform Registration]
        PAT --> |Describe Symptoms| AIBOT[AI Bot Chat]
        PAT --> |Review & Confirm| SUMMARY[Problem Summary]
    end

    subgraph "AI Bot Processing"
        AIBOT --> |Natural Language Processing| NLP[Text Analysis]
        NLP --> |Extract Key Information| EXTRACT[Problem Extraction]
        EXTRACT --> |Generate Summary| SUMMARY
    end

    subgraph "System Actions"
        SUMMARY --> |Send Data| THERAPIST[Therapist Queue]
        THERAPIST --> |Notification| NOTIFY[Therapist Alert]
    end

    classDef user fill:#bbdefb,stroke:#1976d2,stroke-width:2px
    classDef ai fill:#c8e6c9,stroke:#388e3c,stroke-width:2px
    classDef system fill:#f8bbd9,stroke:#c2185b,stroke-width:2px

    class PAT,PLATFORM user
    class AIBOT,NLP,EXTRACT,SUMMARY ai
    class THERAPIST,NOTIFY system
```

## Detailed Sequence Diagram

```mermaid
sequenceDiagram
    participant P as Patient
    participant AI as AI Bot
    participant S as System
    participant T as Therapist
    participant DB as Database

    P->>S: Register on Platform
    S->>DB: Create Patient Profile
    DB->>S: Profile Created
    S->>P: Registration Confirmation

    P->>AI: Start Problem Description
    AI->>P: "Please describe your symptoms and concerns"
    P->>AI: Describe Problem/Symptoms
    AI->>AI: Process & Analyze Description
    AI->>AI: Extract Key Medical Information
    AI->>P: "I understand you're experiencing [summary]. Is this correct?"
    P->>AI: Confirm/Edit Summary
    AI->>S: Send Confirmed Problem Summary
    S->>DB: Store Problem Summary
    S->>T: Notify New Patient Assessment
    T->>S: Review Problem Summary
    T->>P: Schedule Initial Consultation
    S->>P: Send Consultation Details
```

## AI Bot Conversation Flow

```mermaid
graph TD
    START[Patient Starts Chat] --> GREET[AI Bot Greeting]
    GREET --> QUESTION[Ask About Symptoms]
    QUESTION --> COLLECT[Collect Detailed Information]
    COLLECT --> CLARIFY[Ask Clarifying Questions]
    CLARIFY --> SUMMARIZE[Generate Summary]
    SUMMARIZE --> CONFIRM[Present Summary to Patient]
    CONFIRM --> EDIT{Patient Edits?}
    EDIT -->|Yes| COLLECT
    EDIT -->|No| FINAL[Final Confirmation]
    FINAL --> SEND[Send to Therapist]

    classDef start fill:#e3f2fd,stroke:#1976d2,stroke-width:2px
    classDef process fill:#e8f5e8,stroke:#388e3c,stroke-width:2px
    classDef decision fill:#fff3e0,stroke:#f57c00,stroke-width:2px
    classDef final fill:#fce4ec,stroke:#c2185b,stroke-width:2px


    class START,GREET start
    class QUESTION,COLLECT,CLARIFY,SUMMARIZE,CONFIRM process
    class EDIT decision
    class FINAL,SEND final
```

## System Architecture for Milestone 1

```mermaid
graph TB
    subgraph "Frontend Layer"
        WEB[Web Application]
        MOBILE[Mobile App]
        CHAT[Chat Interface]
    end

    subgraph "AI/ML Services"
        NLP[Natural Language Processing]
        INTENT[Intent Recognition]
        ENTITY[Entity Extraction]
        SUMMARY[Text Summarization]
    end

    subgraph "Backend Services"
        AUTH[Authentication Service]
        USER[User Management]
        CHAT_API[Chat API]
        NOTIFICATION[Notification Service]
    end

    subgraph "Data Layer"
        USER_DB[(User Database)]
        CHAT_DB[(Chat History)]
        PROBLEM_DB[(Problem Summaries)]
    end

    subgraph "External Integrations"
        EMAIL[Email Service]
        SMS[SMS Service]
        CALENDAR[Calendar Integration]
    end

    WEB --> AUTH
    MOBILE --> AUTH
    CHAT --> CHAT_API

    CHAT_API --> NLP
    NLP --> INTENT
    NLP --> ENTITY
    NLP --> SUMMARY

    AUTH --> USER_DB
    CHAT_API --> CHAT_DB
    SUMMARY --> PROBLEM_DB

    NOTIFICATION --> EMAIL
    NOTIFICATION --> SMS
    NOTIFICATION --> CALENDAR

    classDef frontend fill:#e3f2fd,stroke:#1976d2,stroke-width:2px
    classDef ai fill:#e8f5e8,stroke:#388e3c,stroke-width:2px
    classDef backend fill:#fff3e0,stroke:#f57c00,stroke-width:2px
    classDef data fill:#fce4ec,stroke:#c2185b,stroke-width:2px
    classDef external fill:#f3e5f5,stroke:#7b1fa2,stroke-width:2px

    class WEB,MOBILE,CHAT frontend
    class NLP,INTENT,ENTITY,SUMMARY ai
    class AUTH,USER,CHAT_API,NOTIFICATION backend
    class USER_DB,CHAT_DB,PROBLEM_DB data
    class EMAIL,SMS,CALENDAR external
```

## Key Features & Requirements

### Patient Experience
- **Simple Registration**: Quick signup with basic information
- **Intuitive Chat Interface**: Natural conversation with AI bot
- **Real-time Feedback**: Immediate responses and clarifications
- **Summary Review**: Ability to edit and confirm problem summary
- **Clear Next Steps**: Transparent process for what happens next

### AI Bot Capabilities
- **Natural Language Understanding**: Process complex medical descriptions
- **Intent Recognition**: Identify the type of problem (pain, mobility, etc.)
- **Entity Extraction**: Extract key medical terms, body parts, symptoms
- **Contextual Clarification**: Ask relevant follow-up questions
- **Summary Generation**: Create clear, structured problem summaries
- **Confirmation Flow**: Ensure accuracy before sending to therapist

### System Requirements
- **Secure Data Handling**: HIPAA-compliant data storage and transmission
- **Real-time Processing**: Fast AI responses for smooth user experience
- **Scalable Architecture**: Handle multiple concurrent users
- **Integration Ready**: Connect with therapist scheduling and notification systems
- **Audit Trail**: Complete logging of all interactions

## Data Flow

```mermaid
graph LR
    subgraph "Data Collection"
        INPUT[Patient Input] --> PROCESS[AI Processing]
        PROCESS --> EXTRACT[Extract Information]
        EXTRACT --> STRUCTURE[Structure Data]
    end

    subgraph "Data Storage"
        STRUCTURE --> USER_PROFILE[User Profile]
        STRUCTURE --> CHAT_LOG[Chat Log]
        STRUCTURE --> PROBLEM_SUMMARY[Problem Summary]
    end

    subgraph "Data Transmission"
        PROBLEM_SUMMARY --> THERAPIST_QUEUE[Therapist Queue]
        THERAPIST_QUEUE --> NOTIFICATION[Therapist Notification]
    end

    classDef collection fill:#e8f5e8,stroke:#388e3c,stroke-width:2px
    classDef storage fill:#fff3e0,stroke:#f57c00,stroke-width:2px
    classDef transmission fill:#fce4ec,stroke:#c2185b,stroke-width:2px

    class INPUT,PROCESS,EXTRACT,STRUCTURE collection
    class USER_PROFILE,CHAT_LOG,PROBLEM_SUMMARY storage
    class THERAPIST_QUEUE,NOTIFICATION transmission
```

## Success Metrics

### Patient Engagement
- **Registration Completion Rate**: % of users who complete registration
- **Chat Completion Rate**: % of users who complete the full AI bot conversation
- **Summary Accuracy**: % of patients who confirm the AI-generated summary
- **Time to Complete**: Average time from registration to therapist notification

### AI Bot Performance
- **Response Time**: Average time for AI bot responses
- **Intent Recognition Accuracy**: % of correctly identified user intents
- **Entity Extraction Accuracy**: % of correctly extracted medical entities
- **Summary Quality**: Therapist satisfaction with AI-generated summaries

### System Performance
- **Uptime**: System availability percentage
- **Concurrent Users**: Maximum users supported simultaneously
- **Data Processing Time**: Time from input to summary generation
- **Integration Success**: % of successful therapist notifications

## Technology Stack

### Frontend
- **Web**: React/TypeScript with Material-UI
- **Mobile**: React Native or Flutter
- **Chat Interface**: Custom chat component with typing indicators

### Backend
- **API**: Node.js/Express or Python/FastAPI
- **Authentication**: JWT tokens with OAuth2
- **Real-time**: WebSocket connections for chat

### AI/ML
- **NLP**: OpenAI GPT-4 or Google BERT
- **Intent Recognition**: Rasa or Dialogflow
- **Entity Extraction**: spaCy or AWS Comprehend Medical
- **Text Summarization**: Custom models or API services

### Data & Infrastructure
- **Database**: PostgreSQL for structured data, Redis for caching
- **Cloud**: AWS/Azure with auto-scaling
- **Monitoring**: Application performance monitoring and logging
- **Security**: End-to-end encryption, HIPAA compliance

This milestone establishes the foundation for the entire physiotherapy workflow by ensuring smooth patient onboarding and effective problem assessment before therapist consultation.
