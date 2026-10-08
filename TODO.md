# CampusFlow outcome tracker

## 1. Brand, responsive shell, and navigation

- [x] Create a modern, responsive student web application called **CampusFlow** with the tagline **“Learn. Connect. Trade. Grow.”** CampusFlow is an all-in-one digital ecosystem for university and college students combining an AI-powered study assistant, document management and learning tools, student communication, and a student marketplace.
- [x] Make the design feel modern, youthful, trustworthy, fun, and professional, like a polished startup product. Use blue as the main brand colour: deep/modern blue primary, light blue secondary, white or very light blue-gray background, dark navy/charcoal text, and small amounts of green, orange, or yellow for statuses and important actions. Use rounded cards, soft shadows, clean spacing, modern typography, subtle animations, smooth hover effects, clean icons, and responsive desktop/tablet/mobile layouts. Avoid excessive gradients, excessive glassmorphism, neon colours, generic AI-looking interfaces, and huge unnecessary animations; make it feel like a real student product.
- [x] Create desktop sidebar navigation and responsive bottom/mobile navigation for Dashboard, AI Tutor, My Documents, Study Tools, Marketplace, Messages, and Profile; include notifications and a user profile/avatar area at the top.
- [x] Implement the approved real-student MVP using the initialized managed server/database and Manus OAuth. Use the existing `webdev_app_session` session convention, validate authenticated ownership in protected operations, and never show a simulated logged-in Preview user. Persist each student's own profile and product data.

## 2. Personalized student dashboard

- [x] Show the greeting “Good morning, [Student Name] 👋” and a short motivational/study message.
- [x] Create dashboard cards for AI Tutor, Recent Documents, Study Progress, Marketplace, and Upcoming Study Tasks.
- [x] Include quick-action buttons “Ask AI Tutor”, “Upload Document”, “Create Summary”, “Generate Quiz”, “Create Flashcards”, and “Browse Marketplace”.
- [x] Include a “Continue Studying” section showing recently opened documents or quizzes, with recent quizzes available to reopen.
- [x] Include a small study-progress visualization for documents processed, quizzes completed, flashcards reviewed, and study streak. Derive progress from saved student activity.

## 3. Education-focused AI Tutor

- [x] Create a modern messaging-style AI chat where students can send questions, receive explanations, ask follow-up questions, and view formatted text, code blocks when needed, and mathematical/scientific formatting as appropriate. Design it specifically for education rather than as a generic chatbot.
- [x] Include persistent conversation history and controls to Start New Chat, rename conversations, and delete conversations.
- [x] Include suggested prompts “Explain this topic simply”, “Give me an example”, “Quiz me on this topic”, “Explain this like I'm a beginner”, and “Create revision notes”.
- [x] Use the built-in AI service for real educational responses, retain owned conversation history, and present clear loading, error, and retry states.

## 4. My Documents / document hub

- [x] Allow a student to upload PDF, DOC/DOCX, PPT/PPTX, TXT, and images where possible; provide a drag-and-drop upload area.
- [x] Store document bytes in platform storage with randomized keys and the document index/metadata in the database. Validate Base64 integrity, file content signatures/Office container markers, and size on the server; require an authenticated owner, and issue download access only after ownership is checked.
- [x] Display document name, file type, upload date, course/module, file size, and processing status on each document card.
- [x] Provide document actions “Open”, “Summarize”, “Generate Quiz”, “Generate Flashcards”, and “Ask AI”.
- [x] Organize documents by Course, Semester, Recent, and Favorites.

## 5. AI Study Tools: summaries, quizzes, and flashcards

- [x] Let students use an uploaded document or manually entered content to generate study material.
- [x] Summaries: offer Quick Summary, Detailed Summary, and Exam Revision Notes; display the result in a clean document-style interface with Copy, Download, Save, and Ask AI about this summary actions. Summaries are automatically saved and shown with a “Saved” state.
- [x] Quizzes: generate from uploaded documents; offer 5, 10, and 20 questions; support Multiple choice, True/False, and Short answer. After a completed quiz, show score, percentage, correct answers, incorrect answers, explanations, and areas needing improvement. Accept semantically equivalent short answers and show grading feedback; save quiz history.
- [x] Flashcards: generate from study material, with Question/Concept on the front and Answer/Explanation on the back; allow flipping and marking Easy, Medium, or Difficult; let students study difficult cards again. Call them **Flashcards**, not “punch cards.”
- [x] Use the built-in AI service for content generation and persist user-owned results/review progress.

## 6. Student marketplace

- [x] Create a student-to-student marketplace that allows students to sell products or offer services to other students, with functional listing creation and browsing and ownership-aware listing management.
- [x] Products include Phones, Laptops, Textbooks, Clothes, Shoes, Electronics, Furniture, Accessories, and Other student-related items.
- [x] Services include Graphic design, Photography, Tutoring, Web development, and Music (the attached brief ends mid-list after “Music”).
- [x] Let students contact listing owners through the student messaging area. Do not add payment or checkout flows: none was specified in the approved scope.

## 7. Messages, profile, and persistence

- [x] Include the Messages and Profile areas in the main navigation, with a useful student profile backed by the signed-in account and student-to-student conversations linked to marketplace contacts.
- [x] Keep tutor conversations, documents, study artifacts, quiz attempts, flashcard review state, study tasks, listings, and messages persistent and isolated to their authenticated owner/participants.

## 8. App completion and route metadata

- [x] Keep the static `/manus-routes.json` page-route manifest synchronized with implemented routes and serve it as JSON rather than SPA fallback HTML.
- [x] Set the project logo metadata to an HTTPS logo asset uploaded via the supported `manus-upload-file` public-CDN method; verify the asset URL responds successfully.
- [x] Run available project TypeScript, test, and build checks; correct actionable issues and commit the deliverable on the canonical project `main` branch as a recorded checkpoint. Do not publish absent an explicit request or already-authorized auto-publish setting.
