```markdown
# Detailed Implementation Plan for Holistic Questionnaire Application

This plan outlines necessary file changes, new file creations, UI/UX design, API endpoints, error handling, and integration steps for building an application that guides patients through a holistic diagnostic questionnaire and delivers personalized therapeutic recommendations to the therapist.

---

## 1. Setup Domain Types and Utility Logic

### File: src/types/diagnosis.ts
- **Purpose:** Define TypeScript interfaces/types for the questionnaire intake, scores, recommendation actions, and session data.
- **Changes:**
  - Create interfaces such as `Intake`, `Scores`, `Recommendation`, and `Session`.
  - Include field definitions for each of the emotional, physical, and spiritual response items.

### File: src/lib/therapeuticAlgorithm.ts
- **Purpose:** Encapsulate business logic (score calculation, recommendation mapping, and rules application).
- **Changes:**
  - Implement `calcScores(intake: Intake): Scores` using the provided pseudocode.
  - Implement `generateRecommendations(intake: Intake, scores: Scores): Recommendation` that returns recommendation packages based on defined rules.
  - (Optional) Include a helper function `getAIRecommendations` for future AI integration via external API calls.
- **Error Handling:** Use try/catch blocks inside these functions and validate inputs before processing.

---

## 2. Frontend — Questionnaire and UI Components

### File: src/app/questionnaire/page.tsx
- **Purpose:** Create a multi-step patient form with a calming, modern UI inspired by Calm.com.
- **Changes:**
  - Layout a full-screen form with soft gradient backgrounds (defined in globals.css) and ample white space.
  - Use existing UI components (e.g., Button, Input, Card) from `src/components/ui/` for consistency.
  - Implement a progress indicator component (e.g., "Pregunta 2 de 8") using plain typography and CSS.
  - Build state management (using React useState/useReducer) to track responses, current question, and form validity.
  - On final submission, send a POST request to `/api/submit` and display error messages if validation fails.

### File: src/components/ProgressIndicator.tsx (New)
- **Purpose:** Display the current progress of the questionnaire.
- **Changes:**
  - Create a simple functional component that receives current and total steps as props and renders a progress bar using CSS (no external images/icons).

### File: src/app/thankyou/page.tsx
- **Purpose:** Show a pleasant “Thank You” / processing screen after submission.
- **Changes:**
  - Display a calm, textual animation or message (“¡Gracias por tus respuestas! Estamos procesando tu información...”) with clear call-to-action.
  - Use a modern layout with centered typography and soft colors.

---

## 3. API Endpoints

### File: src/app/api/submit/route.ts
- **Purpose:** Receive questionnaire submissions, compute scores, generate recommendations, and store session data.
- **Changes:**
  - Create a POST route that parses JSON input.
  - Validate the input payload (use libraries like zod or custom validation).
  - Call functions from `therapeuticAlgorithm.ts` and return JSON with computed scores and recommendations.
  - Implement error handling: return 400 for invalid data and 500 for unexpected errors.
  - (For future expansion) Simulate storage in memory or file-based storage.

### File: src/app/api/sessions/route.ts
- **Purpose:** Provide a GET endpoint for retrieving stored session data for the therapist dashboard.
- **Changes:**
  - Return a JSON list of diagnostic sessions with intake summaries, scores, and recommendations.
  - Ensure proper error handling and add header info for caching if needed.

---

## 4. Admin Dashboard for Therapist

### File: src/app/dashboard/page.tsx
- **Purpose:** Create a secure, simple dashboard that displays session summaries and detailed recommendation panels.
- **Changes:**
  - Fetch session data from the `/api/sessions` endpoint and render a list or grid of session cards.
  - Display intake details, computed scores, and structured recommendations (using cards or tables).
  - Implement filtering or sorting if needed.
  - Use plain typography, spacing, and layout to mimic a calm, uncluttered design.
  - (Optional) Include checkboxes for marking sessions reviewed or adding therapist notes.

---

## 5. Global Styles and Additional UI Considerations

### File: src/app/globals.css
- **Purpose:** Define the calm-themed global CSS styles.
- **Changes:**
  - Add custom classes for soft gradients, modern typography, ample white space, and responsive design.
  - Include fallback styles for form elements and error states.
- **UI/UX Considerations:**
  - Maintain consistency in button styles, card shadows, and spacing.
  - Avoid external icons or image libraries; rely on textual indicators and CSS elements only.

---

## 6. Testing and Documentation

- **API Testing:**  
  - Write sample curl commands for testing `/api/submit` and `/api/sessions`.  
  - Validate JSON responses, HTTP status codes, and response times.  
  - Use error logging and appropriate HTTP error codes.
  
- **Documentation:**  
  - Update the README.md with a description of the new endpoints, data flow, and the overall diagnostic process.
  - Include instructions on how to start the server, run tests, and view the dashboard.

---

## 7. AI Integration (Optional Future Enhancement)

- **AI Features to be Implemented:**  
  - (Optional) Leverage an LLM-based approach via OpenRouter API to generate additional textual recommendations.
  - **Chosen AI Provider:** OpenRouter with a default model of `anthropic/claude-sonnet-4`.
  - **API Endpoint:** `https://openrouter.ai/api/v1/chat/completions`.
  - **Integration:** Create a helper function in `therapeuticAlgorithm.ts` to call the external API and merge additional insights with the deterministic recommendation logic.
- **API Keys:** Request the user to provide the necessary API key in environment variables.

---

# Summary

• Defined new types in `src/types/diagnosis.ts` and business logic in `src/lib/therapeuticAlgorithm.ts`.  
• Created a modern multi-step questionnaire form in `src/app/questionnaire/page.tsx` with a progress indicator and error handling.  
• Built API routes for form submission (`/api/submit`) and session retrieval (`/api/sessions`) with robust validation.  
• Developed a clean, responsive admin dashboard in `src/app/dashboard/page.tsx` for therapists.  
• Updated global styles in `src/app/globals.css` following a calm, minimalist design.  
• Provided optional LLM integration using OpenRouter for enhanced recommendations.  
• Included detailed error handling, security checks, and testing protocols to ensure production readiness.
