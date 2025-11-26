# Medical Analysis Assistant (МедАнализ-Ассистент)

## Overview

This is a Russian-language medical analysis assistant application built with React, Express, and PostgreSQL. The application helps users interpret medical laboratory test results, track symptoms, and manage medication reminders through an AI-powered chat interface. It uses OpenAI's GPT-5 model to provide empathetic, professional medical guidance while maintaining clear disclaimers that it does not replace professional medical advice.

The system is designed with healthcare-specific considerations: Material Design 3 for clarity and trust, structured medical data handling, and prominent safety warnings for emergency symptoms.

## User Preferences

Preferred communication style: Simple, everyday language.

## System Architecture

### Frontend Architecture

**Framework**: React 18 with TypeScript using Vite as the build tool

**UI Component System**: 
- Shadcn/ui components built on Radix UI primitives
- Material Design 3 (Material You) design system
- Tailwind CSS for styling with custom healthcare-focused color tokens
- Roboto font family (including Roboto Mono for numeric values)

**Design Principles**:
- Clarity over aesthetics - medical information must be immediately comprehensible
- High contrast ratios and accessibility-first approach
- Structured information hierarchy with prominent disclaimers
- Consistent spacing system (2, 4, 6, 8 units)

**State Management**:
- TanStack Query (React Query v5) for server state and caching
- React Hook Form with Zod validation for form handling
- Wouter for client-side routing (lightweight alternative to React Router)

**Key Pages**:
- Dashboard: PDF upload, AI chat interface for analysis
- Profile: User medical profile (age, gender, chronic diseases, allergies, medications)
- Reminders: Medication reminder management
- History: Lab analysis history with detailed result views

### Backend Architecture

**Framework**: Express.js with TypeScript

**Server Configuration**:
- Development server uses Vite middleware for HMR
- Production server serves static built files
- Session-based architecture (cookies for state management)

**API Design Pattern**: RESTful endpoints under `/api/*`

**Core Routes**:
- `POST /api/upload-pdf` - PDF upload and text extraction
- `POST /api/chat` - AI conversation endpoint
- `GET/POST /api/profile` - User profile management
- `GET/POST /api/reminders` - Medication reminder CRUD
- `GET /api/analysis-history` - Lab analysis history
- `GET /api/analysis/:id/results` - Detailed test results

**File Upload Handling**:
- Multer middleware for multipart/form-data
- PDF-only validation (10MB limit)
- Temporary storage in `/tmp/uploads/`

### Data Storage

**ORM**: Drizzle ORM with PostgreSQL dialect

**Database Provider**: Neon serverless PostgreSQL

**Schema Design**:

1. **user_profiles** - Patient demographic and medical history
   - age, gender, chronic diseases (array), allergies (array), current medications (array)

2. **lab_analyses** - Uploaded PDF analysis metadata
   - fileName, uploadDate, pdfText (extracted), parsedData (JSONB)

3. **test_results** - Individual lab test results
   - testName, value, unit, referenceMin/Max, status (high/low/normal), testDate
   - Linked to lab_analyses via analysisId

4. **medication_reminders** - Medication schedule tracking
   - medication, dose, times (array), frequency, startDate, endDate, active flag

5. **chat_messages** - AI conversation history
   - role (user/assistant), content, hasEmergencyWarning, hasDisclaimer flags

**Data Validation**: Zod schemas for type-safe inserts and updates

**Migration Strategy**: Drizzle Kit for schema migrations (`npm run db:push`)

### AI Integration

**Provider**: OpenAI API (GPT-5 model)

**System Prompt Design**:
- Russian-language responses by default
- Structured medical analysis format (краткая сводка, разбор результатов, рекомендации)
- Mandatory disclaimers on every health-related response
- Emergency symptom detection with urgent care recommendations
- Triage algorithm for symptom analysis
- Profile-based personalization (age, gender, conditions)

**PDF Processing**:
- Base64 encoding of PDF files
- GPT-5 vision/document understanding for data extraction
- Structured JSON parsing of lab results (test name, value, units, reference ranges)

**Safety Features**:
- Two-tier disclaimer system (standard vs. emergency)
- Explicit "no diagnosis/no prescription" boundaries
- Emergency symptom detection (chest pain, difficulty breathing, etc.) triggers urgent care warnings

### External Dependencies

**Core Infrastructure**:
- **Neon Database**: Serverless PostgreSQL hosting
- **OpenAI API**: GPT-5 model for medical analysis and chat

**Frontend Libraries**:
- **Radix UI**: Headless component primitives (20+ components)
- **TanStack Query**: Server state management
- **Wouter**: Lightweight routing
- **React Hook Form + Zod**: Form handling and validation
- **date-fns**: Date formatting

**Backend Libraries**:
- **Multer**: File upload handling
- **Drizzle ORM**: Type-safe database queries
- **connect-pg-simple**: PostgreSQL session storage

**Development Tools**:
- **Vite**: Build tool and dev server
- **Replit plugins**: Runtime error modal, cartographer, dev banner
- **TypeScript**: Type safety across full stack

**Design System**:
- **Tailwind CSS**: Utility-first styling
- **Google Fonts**: Roboto and Roboto Mono
- **Material Symbols**: Icon font

**Notable Architectural Decisions**:
1. Monorepo structure with shared types between client/server
2. Session-based auth ready (connect-pg-simple configured) but not yet implemented
3. Russian-first UX with design optimized for medical data density
4. Separation of PDF text extraction from AI analysis for flexibility
5. JSONB storage for flexible parsed data structure while maintaining relational test results