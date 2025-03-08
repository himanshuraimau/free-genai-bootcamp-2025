# Backend Server Technical Specs

## Business Goal

A language learning school wants to build a prototype of a learning portal which will act as three things:
- Inventory of possible vocabulary that can be learned.
- Act as a Learning Record Store (LRS), providing correct and wrong scores on practice vocabulary.
- A unified launchpad to launch different learning apps.

## Technical Specs
 - The backend will be in Python.
 - The database will be SQLite3.
 - The API will be built using FastAPI.
 - The API will always return JSON.
 - There will be no auth.
 - Everything will be treated like a single user.

# Database Schema

We have the following tables:
 - words - stored vocabulary words
   - id integer
   - japanese string
   - romaji string
   - english string
   - parts json
 - word_groups - join table for groups and words (many to many)
   - id integer
   - word_id integer
   - group_id integer
 - groups - thematic groups of words
   - id integer
   - name string
 - study_sessions - record of study sessions
   - id integer
   - group_id integer
   - created_at datetime
   - study_activity_id integer
 - study_activities - a specific study activity, linking a study session to a group
   - id integer
   - study_session_id integer
   - group_id integer
 - word_review_items - a record of word practice, determining if the word was correct or not
   - word_id integer
   - study_session_id integer
   - correct boolean
   - created_at datetime

## API ENDPOINTS

- GET /words - Get all words
- GET /words/:id - Get specific word
- POST /words - Create new word
- PUT /words/:id - Update word
- DELETE /words/:id - Delete word

- GET /groups - Get all groups
- GET /groups/:id - Get specific group
- POST /groups - Create new group
- PUT /groups/:id - Update group
- DELETE /groups/:id - Delete group
- GET /groups/:id/words - Get all words in a group

- POST /word-groups - Add word to group
- DELETE /word-groups/:word_id/:group_id - Remove word from group

- POST /study-sessions - Create new study session
- GET /study-sessions - Get all study sessions
- GET /study-sessions/:id - Get specific study session details
- GET /study-sessions/:id/results - Get results for a study session

- POST /study-activities - Create new study activity
- GET /study-activities/:session_id - Get activities for a session

- POST /word-reviews - Submit word review result
- GET /word-reviews/:session_id - Get all reviews for a session
- GET /word-reviews/stats - Get overall statistics of word reviews

Each endpoint returns JSON with the following structure:
```json
{
    "status": "success|error",
    "data": {}, // actual data or error message
    "message": "optional message"
}
```
