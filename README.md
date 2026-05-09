# IELTS Prep Application

A comprehensive full-stack application for IELTS (International English Language Testing System) exam preparation. Features interactive modules for Reading, Listening, Writing, and Speaking practice with AI-powered feedback.

## 📋 Project Overview

This is a **MERN-based** application (MongoDB, Express, React, Node.js) with Google Generative AI integration for intelligent test evaluation and feedback.

**Key Features:**
- 4 interactive test modules (Reading, Listening, Writing, Speaking)
- Real-time feedback using Google Gemini AI
- Progress tracking and scoring
- Clean, responsive UI with Tailwind CSS
- RESTful API backend with MongoDB

---

## 📁 Project Structure

```
Minor-Project/
├── client/                          # React frontend (TypeScript + Vite)
│   ├── src/
│   │   ├── components/              # Reusable React components
│   │   │   ├── WelcomeScreen.tsx
│   │   │   ├── ReadingModule.tsx
│   │   │   ├── ListeningModule.tsx
│   │   │   ├── WritingModule.tsx
│   │   │   ├── SpeakingModule.tsx
│   │   │   ├── ResultsScreen.tsx
│   │   │   └── Timer.tsx
│   │   ├── App.tsx                  # Main app component with routing
│   │   ├── main.tsx                 # Entry point
│   │   ├── App.css                  # App-level styles
│   │   ├── index.css                # Global styles
│   │   └── assets/                  # Static assets
│   ├── public/                      # Public assets
│   ├── package.json                 # Client dependencies
│   ├── vite.config.ts               # Vite configuration
│   ├── tsconfig.json                # TypeScript configuration
│   └── index.html                   # HTML template
│
├── server/                          # Express backend (Node.js)
│   ├── index.js                     # Express server entry point
│   ├── seed.js                      # Database seeding script
│   ├── test-speaking-feedback.js    # Test script for speaking feedback
│   ├── package.json                 # Server dependencies
│   │
│   ├── models/                      # MongoDB Mongoose schemas
│   │   ├── ReadingTest.js           # Reading test model
│   │   ├── ListeningTest.js         # Listening test model
│   │   ├── WritingTest.js           # Writing task model
│   │   ├── SpeakingTest.js          # Speaking test model
│   │   └── WritingSubmission.js     # User writing submissions
│   │
│   ├── routes/                      # API endpoints
│   │   ├── reading.js               # GET /api/reading
│   │   ├── listening.js             # GET /api/listening
│   │   ├── writing.js               # GET/POST /api/writing
│   │   └── speaking.js              # GET/POST /api/speaking
│   │
│   ├── services/                    # Business logic & utilities
│   │   └── feedbackService.js       # Gemini AI feedback generation
│   │
│   ├── scripts/                     # Utility scripts
│   │   └── check_gemini.js          # Verify Gemini API connection
│   │
│   └── public/                      # Static files (if needed)
│
├── .gitignore                       # Git ignore rules
├── BACKEND_SETUP.md                 # Backend setup instructions
└── README.md                         # This file
```

---

## 🎯 Component Documentation

### **Frontend Components** (`client/src/components/`)

#### **1. WelcomeScreen.tsx**
- **Purpose:** Landing page/home screen
- **Functionality:** Displays app introduction and navigation to test modules
- **Key Features:**
  - Navigation buttons to different test sections
  - Brief description of IELTS exam

#### **2. ReadingModule.tsx**
- **Purpose:** Interactive reading comprehension test
- **Functionality:**
  - Displays reading passages
  - Shows multiple-choice and short-answer questions
  - Tracks user responses
  - Validates answers against correct answers
- **Flow:**
  1. Fetch reading passages from `/api/reading`
  2. Display questions for each passage
  3. Record user answers
  4. Calculate score and show results

#### **3. ListeningModule.tsx**
- **Purpose:** Listening comprehension test
- **Functionality:**
  - Presents listening sections with questions
  - Simulates audio-based questions (currently text-based)
  - Multiple sections with progressive difficulty
  - Timed responses
- **Flow:**
  1. Fetch listening test data from `/api/listening`
  2. Present questions section by section
  3. Record answers
  4. Evaluate and score

#### **4. WritingModule.tsx** (`WritingModule.tsx` + `WritingModule.css`)
- **Purpose:** Writing task evaluation with AI feedback
- **Functionality:**
  - Two writing tasks (Task 1 & Task 2)
  - Users submit written responses
  - **AI Integration:** Sends submission to Gemini API for feedback
  - Displays feedback on grammar, coherence, vocabulary, etc.
- **Flow:**
  1. Fetch writing prompts from `/api/writing`
  2. User writes response
  3. Submit to `/api/writing` endpoint
  4. Backend uses Gemini AI to evaluate
  5. Display detailed feedback and band score

#### **5. SpeakingModule.tsx**
- **Purpose:** Speaking test evaluation
- **Functionality:**
  - 3 parts of speaking test with different question types
  - Records speaking responses (transcribed or text-based)
  - Submits to backend for AI evaluation
  - Returns feedback on fluency, pronunciation, grammar
- **Flow:**
  1. Get speaking questions from `/api/speaking`
  2. User provides response (text or speech)
  3. POST to `/api/speaking` for AI evaluation
  4. Receive band score and detailed feedback

#### **6. ResultsScreen.tsx**
- **Purpose:** Display test results and scores
- **Functionality:**
  - Shows overall band score (0-9)
  - Component-wise breakdown (Reading, Listening, Writing, Speaking)
  - Detailed feedback and performance analysis
  - Options to retake tests or return home

#### **7. Timer.tsx**
- **Purpose:** Countdown timer for timed modules
- **Functionality:**
  - Displays remaining time
  - Warns when time is running out
  - Triggers submission when time expires
  - Used in Reading, Listening modules

---

## 🔧 Backend Structure

### **Models** (`server/models/`)

Each model defines the database schema for storing test data.

#### **ReadingTest.js**
```javascript
{
  _id: ObjectId,
  passage: String,          // Reading passage text
  questions: Array,         // Array of question objects
  correctAnswers: Array,    // Correct answers for validation
  difficulty: String        // easy | medium | hard
}
```

#### **ListeningTest.js**
```javascript
{
  _id: ObjectId,
  section: Number,          // 1, 2, 3, or 4
  questions: Array,         // Question objects
  correctAnswers: Array,    // Answers for grading
  audioDuration: Number     // Duration in seconds
}
```

#### **WritingTest.js**
```javascript
{
  _id: ObjectId,
  taskNumber: Number,       // 1 or 2
  prompt: String,           // Writing task instruction
  minimumWords: Number,     // Minimum word requirement
  timeLimit: Number         // Time in minutes
}
```

#### **SpeakingTest.js**
```javascript
{
  _id: ObjectId,
  part: Number,            // 1, 2, or 3
  questions: Array,        // Speaking questions
  duration: Number,        // Duration in seconds per part
  bandScaleGuide: Object   // Evaluation criteria
}
```

#### **WritingSubmission.js**
```javascript
{
  _id: ObjectId,
  userId: String,
  taskNumber: Number,
  submission: String,      // User's written response
  feedback: Object,        // AI feedback from Gemini
  {
    score: Number,        // Band score 0-9
    grammar: Object,      // Grammar feedback
    coherence: Object,    // Coherence & organization
    vocabulary: Object,   // Vocabulary range & accuracy
    taskCompletion: Object // Task fulfillment
  },
  timestamp: Date
}
```

---

### **Routes/API Endpoints** (`server/routes/`)

#### **reading.js** - `/api/reading`
```
GET /api/reading
└─ Returns: Array of reading test passages with questions and answers
   Response: { passages, questions, correctAnswers, difficulty }
```

#### **listening.js** - `/api/listening`
```
GET /api/listening
└─ Returns: Array of listening test sections with questions
   Response: { sections, questions, correctAnswers }
```

#### **writing.js** - `/api/writing`
```
GET /api/writing
└─ Returns: Array of writing task prompts
   Response: { tasks: [{ taskNumber, prompt, minimumWords, timeLimit }] }

POST /api/writing
└─ Request: { taskNumber, submission, userId }
└─ Returns: Gemini AI feedback
   Response: { score, feedback, grammar, coherence, vocabulary, taskCompletion }
```

#### **speaking.js** - `/api/speaking`
```
GET /api/speaking
└─ Returns: Speaking test questions for all 3 parts
   Response: { parts: [{ part, questions, duration }] }

POST /api/speaking
└─ Request: { part, response, userId }
└─ Returns: AI evaluation with band score
   Response: { score, feedback, fluency, pronunciation, grammar }
```

---

### **Services** (`server/services/`)

#### **feedbackService.js**
- **Purpose:** Centralized AI feedback generation using Google Gemini API
- **Key Methods:**
  - `generateWritingFeedback(submission, taskNumber)` - Evaluates writing
  - `generateSpeakingFeedback(response, part)` - Evaluates speaking
  - **AI Criteria:**
    - **Writing:** Grammar, vocabulary, coherence, task fulfillment, band score
    - **Speaking:** Fluency, pronunciation, grammar, vocabulary, band score

---

### **Scripts** (`server/scripts/`)

#### **check_gemini.js**
- Tests connectivity to Google Generative AI API
- Verifies API key configuration
- Useful for debugging API issues

---

## 🚀 Tech Stack

### **Frontend**
| Technology | Purpose |
|-----------|---------|
| **React 18** | UI framework |
| **TypeScript** | Type-safe JavaScript |
| **Vite** | Fast build tool & dev server |
| **Tailwind CSS** | Utility-first CSS styling |
| **Lucide React** | Icon library |
| **Axios** | HTTP client for API calls |
| **React Router** | Client-side routing |
| **React Toastify** | Toast notifications |

### **Backend**
| Technology | Purpose |
|-----------|---------|
| **Express.js** | Web framework |
| **MongoDB** | NoSQL database |
| **Mongoose** | MongoDB ODM |
| **Google Generative AI** | AI-powered feedback (Gemini) |
| **CORS** | Cross-origin resource sharing |
| **Nodemon** | Auto-reload during development |

---

## 🔧 Setup Instructions

### **Prerequisites**
- Node.js (v14+)
- MongoDB (local or Atlas cloud)
- Google Gemini API key

### **1. Backend Setup**

```bash
# Navigate to server directory
cd server

# Install dependencies
npm install

# Create .env file
cat > .env << EOF
MONGODB_URI=mongodb://127.0.0.1:27017/ielts_prep
PORT=5000
GOOGLE_API_KEY=your_gemini_api_key_here
EOF

# Seed the database with initial test data
npm run seed

# Start the server
npm run dev
# Server runs on http://localhost:5000
```

### **2. Frontend Setup**

```bash
# Navigate to client directory
cd client

# Install dependencies
npm install

# Create environment file (optional)
cat > .env.local << EOF
VITE_API_URL=http://localhost:5000
EOF

# Start development server
npm run dev
# App runs on http://localhost:5173
```

### **3. Verify Setup**
```bash
# Test Gemini API connection
cd server
node scripts/check_gemini.js
```

---

## 🔄 Data Flow

### **Reading Test Flow**
```
User selects Reading → WelcomeScreen routes to ReadingModule
  ↓
ReadingModule fetches passages from GET /api/reading
  ↓
Display passage and questions to user
  ↓
User submits answers → Compare with correctAnswers
  ↓
Calculate score → Show ResultsScreen
```

### **Writing Test Flow**
```
User selects Writing → WritingModule fetches prompts
  ↓
User reads task prompt and writes response
  ↓
Submit response → POST to /api/writing
  ↓
feedbackService.generateWritingFeedback() called
  ↓
Gemini AI evaluates: grammar, coherence, vocabulary, task completion
  ↓
Feedback returned to client → Display on WritingModule
```

### **Speaking Test Flow**
```
User selects Speaking → SpeakingModule fetches questions
  ↓
User records/provides response for each part (1, 2, 3)
  ↓
Submit → POST to /api/speaking
  ↓
feedbackService.generateSpeakingFeedback() called
  ↓
Gemini AI evaluates: fluency, pronunciation, grammar
  ↓
Band score calculated → Display results
```

---

## 📊 Key Features

| Feature | Location | Implementation |
|---------|----------|-----------------|
| **4 Test Modules** | `client/src/components/` | Separate components for each skill |
| **AI Feedback** | `server/services/feedbackService.js` | Google Gemini API integration |
| **Progress Tracking** | `WritingSubmission` model | Stores submissions with feedback |
| **Responsive UI** | `client/src/` | Tailwind CSS responsive design |
| **Real-time Timer** | `Timer.tsx` | Countdown for timed sections |
| **Database Storage** | MongoDB models | Persistent data for tests & submissions |

---

## 🎓 IELTS Scoring (0-9 Band Scale)

The application evaluates submissions using the standard IELTS band scale:
- **9** - Expert User
- **8** - Very Good User
- **7** - Good User
- **6** - Competent User
- **5** - Modest User
- **4** - Limited User
- **3** - Extremely Limited User
- **2** - Intermittent User
- **1** - Non User

Each test module scores on this scale, with overall band averaging all four skills.

---

## 🐛 Troubleshooting

| Issue | Solution |
|-------|----------|
| **Cannot connect to MongoDB** | Ensure MongoDB is running locally or check MongoDB Atlas connection string in `.env` |
| **Gemini API errors** | Verify `GOOGLE_API_KEY` is set correctly in `server/.env` |
| **CORS errors** | Check CORS configuration in `server/index.js` |
| **Port already in use** | Change `PORT` in `.env` or kill process using the port |
| **Dependencies missing** | Run `npm install` in both `client/` and `server/` directories |

---

## 📝 Environment Variables

### **Server** (`server/.env`)
```
MONGODB_URI=mongodb://127.0.0.1:27017/ielts_prep
PORT=5000
GOOGLE_API_KEY=your_gemini_api_key_here
NODE_ENV=development
```

### **Client** (`client/.env.local`) [Optional]
```
VITE_API_URL=http://localhost:5000
```

---

## 📦 Dependencies Overview

### **Frontend**
- **React & React DOM**: UI rendering
- **Vite**: Fast build tool
- **TypeScript**: Type safety
- **Tailwind CSS**: Styling
- **Axios**: API communication
- **React Router**: Navigation
- **Lucide React**: Icons

### **Backend**
- **Express**: HTTP server
- **MongoDB & Mongoose**: Database
- **Google Generative AI**: AI feedback
- **CORS**: Cross-origin handling
- **Dotenv**: Environment variables

---

## 🚢 Deployment Considerations

- **Frontend:** Build with `npm run build` → Deploy to Vercel, Netlify
- **Backend:** Deploy to Heroku, Railway, or any Node.js hosting
- **Database:** Use MongoDB Atlas for cloud hosting
- **API Keys:** Use environment variables, never commit sensitive keys
- **CORS:** Configure allowed origins for production

---

## 📞 Support & Contribution

For issues or improvements, ensure:
1. Both `client/` and `server/` directories have dependencies installed
2. Environment variables are properly configured
3. MongoDB is accessible
4. Google Gemini API key is valid

---

## 📄 License

This project is created for educational purposes.

---

**Happy studying! 🎓**
