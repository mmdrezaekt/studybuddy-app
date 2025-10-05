# 📚 StudyBuddy - Smart Study Planning Platform

<div align="center">
  <img src="https://img.shields.io/badge/React-19.2.0-blue?style=for-the-badge&logo=react" />
  <img src="https://img.shields.io/badge/TypeScript-4.9.5-blue?style=for-the-badge&logo=typescript" />
  <img src="https://img.shields.io/badge/Firebase-12.3.0-orange?style=for-the-badge&logo=firebase" />
  <img src="https://img.shields.io/badge/Tailwind-3.4.18-cyan?style=for-the-badge&logo=tailwindcss" />
</div>

<div align="center">
  <h3>🎯 A comprehensive study planning platform with group collaboration features</h3>
  <p><strong>Live Demo:</strong> <a href="https://studybuddy-393f1.web.app">https://studybuddy-393f1.web.app</a></p>
</div>

---

## ✨ **Features**

### 🏠 **Dashboard**
- **Progress Overview** - Track your study progress with visual statistics
- **Study Plan Management** - Create, edit, and organize study plans
- **Responsive Design** - Optimized for mobile, tablet, and desktop

### 📋 **Study Planning**
- **Individual & Group Plans** - Create personal or collaborative study plans
- **Task Management** - Add, complete, and track study tasks
- **Note Taking** - Keep detailed notes for each study plan
- **Progress Tracking** - Visual progress bars and completion statistics

### 👥 **Collaboration**
- **Group Study Plans** - Invite friends to study together
- **Member Management** - Add/remove group members
- **Real-time Updates** - See changes instantly across all devices

### 🔔 **Notifications**
- **Smart Reminders** - Get notified about upcoming deadlines
- **Push Notifications** - Real-time updates on your phone
- **Customizable Settings** - Control what notifications you receive

### 📱 **Mobile-First Design**
- **Responsive Layout** - Perfect on all screen sizes
- **Touch Optimized** - Smooth mobile interactions
- **PWA Support** - Install as a mobile app

---

## 🚀 **Quick Start**

### **Prerequisites**
- Node.js 16+ 
- npm or yarn
- Firebase account

### **Installation**

```bash
# Clone the repository
git clone https://github.com/mmdrezaekt/studybuddy-app.git

# Navigate to project directory
cd studybuddy-app

# Install dependencies
npm install

# Start development server
npm start
```

### **Firebase Setup**

1. Create a Firebase project at [console.firebase.google.com](https://console.firebase.google.com)
2. Enable Authentication, Firestore, and Hosting
3. Copy your Firebase config to `src/firebase/config.ts`
4. Deploy Cloud Functions:
   ```bash
   cd functions
   npm install
   firebase deploy --only functions
   ```

---

## 🛠️ **Tech Stack**

### **Frontend**
- **React 19.2.0** - UI framework
- **TypeScript 4.9.5** - Type safety
- **Tailwind CSS 3.4.18** - Styling
- **React Router 7.9.3** - Navigation

### **Backend**
- **Firebase Authentication** - User management
- **Firestore** - NoSQL database
- **Cloud Functions** - Serverless backend
- **Firebase Hosting** - Web hosting

### **Testing**
- **Jest** - Unit testing
- **Cypress** - E2E testing
- **React Testing Library** - Component testing

---

## 📁 **Project Structure**

```
studybuddy/
├── src/
│   ├── components/          # React components
│   │   ├── Auth/            # Authentication
│   │   ├── Dashboard/       # Main dashboard
│   │   ├── StudyPlan/      # Study plan management
│   │   └── Notifications/  # Notification system
│   ├── firebase/           # Firebase configuration
│   ├── services/           # API services
│   └── types/              # TypeScript definitions
├── functions/              # Firebase Cloud Functions
├── cypress/               # E2E tests
└── public/                # Static assets
```

---

## 🧪 **Testing**

```bash
# Run unit tests
npm test

# Run E2E tests
npm run test:e2e

# Run all tests
npm run test:all
```

---

## 📱 **Screenshots**

<div align="center">
  <img src="public/logo192.png" alt="StudyBuddy Logo" width="100" />
</div>

*Mobile-responsive design with clean, modern interface*

---

## 🚀 **Deployment**

### **Firebase Hosting**
```bash
# Build for production
npm run build

# Deploy to Firebase
firebase deploy
```

### **Environment Variables**
Create a `.env` file with your Firebase configuration:
```env
REACT_APP_FIREBASE_API_KEY=your_api_key
REACT_APP_FIREBASE_AUTH_DOMAIN=your_domain
REACT_APP_FIREBASE_PROJECT_ID=your_project_id
```

---

## 🤝 **Contributing**

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

---

## 📄 **License**

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## 📞 **Support**

- 📧 Email: support@studybuddy.app
- 🐛 Issues: [GitHub Issues](https://github.com/mmdrezaekt/studybuddy-app/issues)
- 📖 Documentation: [Full Documentation](PROJECT_DOCUMENTATION.md)

---

<div align="center">
  <p>Made with ❤️ by <strong>Mohsen Ektefaei</strong></p>
  <p>⭐ Star this repo if you found it helpful!</p>
</div>