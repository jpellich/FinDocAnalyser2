# Medical Analysis Assistant - Design Guidelines

## Design Approach
**System:** Material Design 3 (Material You)  
**Rationale:** Healthcare applications require maximum clarity, accessibility, and trust. Material Design provides proven patterns for data-dense interfaces with strong visual hierarchy and excellent form/table components essential for medical data presentation.

## Core Design Principles
1. **Clarity Over Aesthetics** - Medical information must be immediately comprehensible
2. **Trust Through Structure** - Consistent, predictable layouts build user confidence
3. **Accessibility First** - High contrast ratios, clear typography, sufficient touch targets
4. **Information Hierarchy** - Critical medical disclaimers and warnings must be visually prominent

## Typography System

**Font Family:** Roboto (via Google Fonts CDN)
- Primary: Roboto (400, 500, 700)
- Monospace: Roboto Mono (for lab values/numbers)

**Hierarchy:**
- H1: text-4xl font-bold (Main headings, Analysis titles)
- H2: text-2xl font-semibold (Section headers)
- H3: text-xl font-medium (Subsections, Test names)
- Body: text-base (General content, explanations)
- Small: text-sm (Metadata, timestamps, helper text)
- Caption: text-xs (Disclaimers - must be legible despite size)
- Numbers/Values: text-lg font-mono font-semibold (Lab result values)

**Special Typography:**
- Medical Disclaimers: text-sm font-medium with border/background treatment for visibility
- Warning Text: text-base font-semibold
- Reference Ranges: text-sm font-mono

## Layout System

**Spacing Primitives:** Tailwind units of 2, 4, 6, and 8
- Micro spacing (between related items): p-2, gap-2
- Standard spacing (form fields, cards): p-4, gap-4, m-4
- Section spacing: p-6, py-8
- Large spacing (major sections): p-8, gap-8

**Container Strategy:**
- Main content: max-w-6xl mx-auto
- Chat interface: max-w-4xl mx-auto
- Forms: max-w-2xl mx-auto
- Sidebar: w-64 (profile, history navigation)
- Data tables: w-full with responsive scroll

**Grid Layouts:**
- Profile cards: grid-cols-1 md:grid-cols-2 gap-4
- Test results comparison: grid-cols-1 lg:grid-cols-2 gap-6
- Dashboard metrics: grid-cols-2 md:grid-cols-4 gap-4

## Component Library

### Navigation
- **App Bar:** Fixed top navigation with app title, profile icon, settings
- **Sidebar (Desktop):** Persistent left sidebar with: Profile Summary, Upload New Analysis, Analysis History, Medication Reminders, Symptom Checker
- **Bottom Navigation (Mobile):** Fixed bottom nav with 4-5 primary actions

### File Upload
- **Drag-and-Drop Zone:** Large, prominent area with dashed border, centered icon and text
- States: Default, Hover (elevated), Active (dragging), Success, Error
- File preview after upload with filename, size, remove option

### Chat Interface
- **Message Bubbles:** 
  - User messages: Aligned right, distinct background
  - Assistant messages: Aligned left, structured with sections
  - Max width: max-w-3xl for readability
  - Padding: p-4, rounded-2xl
  
- **Structured Medical Response:**
  - Section headers with icons (summary, explanation, analysis, recommendations)
  - Collapsible sections for lengthy content
  - Highlighted warnings/disclaimers with border-l-4 accent

### Lab Results Display
- **Results Table:**
  - Columns: Test Name | Value | Units | Reference Range | Status
  - Row highlighting for out-of-range values
  - Sticky header on scroll
  - Sortable columns
  
- **Individual Result Card:**
  - Test name (text-xl font-semibold)
  - Large value display (text-3xl font-mono)
  - Reference range in smaller text below
  - Visual indicator (icon/badge) for normal/high/low status
  - Date/time stamp (text-xs)

### Forms
- **Profile Form:** Structured sections with labels above inputs
- **Input Fields:** Standard Material Design outlined text fields with floating labels
- **Medication Reminder Form:**
  - Medication name (text input)
  - Dosage (number input with unit selector)
  - Time pickers (multiple times support)
  - Frequency selector (radio buttons: daily/weekly/custom)
  - Date range pickers
  - Notes textarea
  
### Cards
- **Analysis History Card:** 
  - Date uploaded (text-base font-semibold)
  - Number of tests
  - Quick stats (tests out of range)
  - Action buttons (View, Compare, Delete)
  - Padding: p-4, rounded-lg, shadow-sm

- **Medication Reminder Card:**
  - Medication name (text-lg font-semibold)
  - Dosage and time
  - Next dose countdown
  - Action buttons (Take Now, Skip, Edit)

### Disclaimers & Warnings
- **Standard Disclaimer:** 
  - Border treatment (border-l-4)
  - Background differentiation
  - Icon (info/warning)
  - Padding: p-4
  - Margin: mt-6 (at end of responses)

- **Emergency Warning:**
  - Prominent placement (top of response if triggered)
  - Strong visual treatment (border, background, icon)
  - Larger text (text-base font-semibold)
  - Call-to-action button (Call Emergency Services)

### Buttons
- **Primary:** Elevated, medium size, rounded-lg, px-6 py-3
- **Secondary:** Outlined variant, same sizing
- **Icon Buttons:** Circular, p-2, for actions like delete/edit
- **Text Buttons:** For tertiary actions

### Data Visualization
- **Trend Charts:** Line charts for tracking test results over time
- **Status Indicators:** Badge components with icons for normal/high/low
- **Progress Indicators:** For multi-step processes (PDF processing, analysis)

## Icons
**Library:** Material Symbols (via Google Fonts Icon CDN)
- Upload: upload_file
- Analysis: lab_profile, science
- Chat: chat_bubble
- Profile: account_circle
- Medication: medication, pill
- Warning: warning, error
- Info: info
- Calendar: calendar_today
- History: history

## Accessibility Requirements
- All form inputs must have associated labels
- Color must not be the only means of conveying status (use icons + text)
- Minimum touch target: 44x44px (p-3 for icon buttons)
- Focus indicators on all interactive elements
- ARIA labels for icon-only buttons
- Screen reader announcements for warnings/disclaimers
- Keyboard navigation support throughout

## Special Considerations

**Russian Language:**
- Ensure Roboto supports Cyrillic characters fully
- Account for longer text strings in layouts (Russian typically 15-20% longer than English)
- Right-aligned labels may need adjustment for RTL considerations

**Medical Data Sensitivity:**
- No session persistence of medical data without explicit consent
- Visual indicators when data is being processed
- Clear data deletion options

**PDF Processing States:**
- Loading indicator during upload/processing
- Success state with extracted data preview
- Error state with retry option
- Progress indicator for multi-page documents

## Layout Examples

**Dashboard View:**
- Hero area: Upload PDF (prominent drag-drop zone)
- Below: Recent Analyses (3 cards in grid)
- Sidebar: Profile summary, Quick stats

**Analysis Detail View:**
- Top: Document metadata (date, source)
- Main area: Results table with sorting/filtering
- Right panel: AI interpretation chat
- Bottom: Disclaimer

**Chat Interface:**
- Fixed header with context (current analysis/symptom)
- Scrollable message area (main content)
- Fixed bottom input area with attach/send

This design system prioritizes medical data clarity, user trust, and efficient information processing while maintaining professional healthcare application standards.