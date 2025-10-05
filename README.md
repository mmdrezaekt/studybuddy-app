# StudyBuddy - Collaborative Study Planner

StudyBuddy is a web-based platform designed to help students plan, organize, and collaborate on their academic goals. It allows users to create personalized study plans, share notes, and monitor progress, while also enabling small groups of students to work together on shared learning objectives.

## Features

- **User Authentication**: Sign up and log in using Google or email/password via Firebase Authentication
- **Study Plan Management**: Create study plans with tasks, deadlines, and progress tracking
- **Collaboration**: Invite others to collaborate on study plans
- **File Sharing**: Upload and share study materials (PDFs, images, documents)
- **Progress Tracking**: Visual progress bars and gamification elements
- **Real-time Updates**: Instant synchronization across all devices
- **Responsive Design**: Works on desktop and mobile devices

## Technology Stack

- **Frontend**: React 19 + TypeScript + TailwindCSS
- **Backend**: Firebase (Firestore, Authentication, Storage, Cloud Functions)
- **Routing**: React Router DOM
- **State Management**: React Hooks
- **Styling**: TailwindCSS

## Getting Started

### Prerequisites

- Node.js (v14 or higher)
- npm or yarn
- Firebase project

### Installation

1. Clone the repository:
```bash
git clone <repository-url>
cd studybuddy
```

2. Install dependencies:
```bash
npm install
```

3. Configure Firebase:
   - Create a Firebase project at [Firebase Console](https://console.firebase.google.com/)
   - Enable Authentication, Firestore, and Storage
   - Copy your Firebase configuration
   - Update `src/firebase/config.ts` with your Firebase config

4. Start the development server:
```bash
npm start
```

5. Open [http://localhost:3000](http://localhost:3000) to view the app

## Available Scripts

- `npm start` - Runs the app in development mode
- `npm run build` - Builds the app for production
- `npm test` - Launches the test runner
- `npm run eject` - Ejects from Create React App (one-way operation)

## Project Structure

```
src/
├── components/
│   ├── Auth/           # Authentication components
│   ├── Dashboard/      # Main dashboard components
│   └── StudyPlan/      # Study plan detail components
├── firebase/           # Firebase configuration
├── types/              # TypeScript type definitions
└── App.tsx             # Main app component
```

## Firebase Setup

1. Create a new Firebase project
2. Enable the following services:
   - Authentication (Email/Password and Google)
   - Firestore Database
   - Storage
   - Cloud Functions (optional)
3. Set up Firestore security rules
4. Configure Firebase Hosting (optional)

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Submit a pull request

## License

This project is licensed under the MIT License.
