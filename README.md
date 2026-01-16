# Spark - Smart Classroom Management System

A comprehensive, AI-powered classroom management system with face recognition authentication, automated attendance tracking, intelligent class scheduling, and Gemini AI integration.

## 📋 Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Technology Stack](#technology-stack)
- [Architecture](#architecture)
- [Code Structure](#code-structure)
- [Installation & Setup](#installation--setup)
- [Usage Guide](#usage-guide)
- [API & Functions Reference](#api--functions-reference)
- [Data Storage](#data-storage)
- [Security & Privacy](#security--privacy)
- [Configuration](#configuration)
- [Troubleshooting](#troubleshooting)
- [Future Enhancements](#future-enhancements)

## 🌟 Overview

Spark is a single-page application (SPA) built entirely in vanilla JavaScript, HTML, and CSS. It provides a complete classroom management solution with three distinct user roles: **Student**, **Teacher**, and **Admin**. The system uses face recognition for secure authentication, GPS-based location verification for attendance, and includes an intelligent class rescheduling system.

### Key Highlights

- **100% Client-Side**: No backend server required - all data stored in browser LocalStorage
- **Face Recognition**: Biometric authentication using TensorFlow.js models
- **Location Verification**: GPS-based attendance verification
- **AI Integration**: Gemini AI chat assistant for user support
- **Responsive Design**: Works on desktop, tablet, and mobile devices
- **Dark Theme UI**: Modern dark mode interface with neon accents

## 🎯 Features

### 🔐 Authentication & Security

#### Face Recognition System
- **Registration**: Capture and store facial descriptors for new users
- **Login**: Real-time face matching with confidence threshold (0.6)
- **Liveness Detection**: Advanced anti-spoofing system that detects:
  - Face movement and position changes
  - Eye blinking (minimum 2 blinks required)
  - Face size variation (zoom in/out detection)
  - Static image/video detection
  - Eye Aspect Ratio (EAR) variance analysis

#### Location Verification
- GPS-based attendance verification
- Configurable classroom location and radius (default: 49 meters)
- Prevents remote attendance marking
- Real-time distance calculation using Haversine formula

### 👨‍🎓 Student Features

1. **Dashboard**
   - Welcome screen with personalized greeting
   - Real-time date and time display
   - Attendance statistics (today, week, total)
   - Upcoming classes counter
   - Quick action buttons
   - Today's schedule preview
   - Recent activity feed

2. **Attendance Management**
   - Face recognition-based attendance marking
   - Liveness detection to prevent spoofing
   - Location verification
   - Today's attendance history table
   - Automatic status updates

3. **Timetable**
   - Weekly timetable view (Monday-Friday)
   - Day-based navigation
   - Real-time status indicators:
     - **Upcoming** (Blue) - Future classes
     - **Ongoing** (Yellow) - Current classes
     - **Completed** (Green) - Past classes
   - Batch-specific timetable loading
   - PDF export functionality
   - Auto-refresh every minute

4. **Rescheduler Tool**
   - View class rescheduling interface
   - Preview regenerated schedules

5. **AI-Powered Suggestions**
   - Personalized study recommendations
   - Based on student interests and skills
   - Activity cards with time estimates
   - Quick start buttons

6. **Profile Management**
   - Profile photo upload and display
   - Personal information editing:
     - Full name
     - College ID
     - Email
     - Course
   - Skills and interests management
   - Career goals text area
   - Auto-save functionality

7. **Daily Routine**
   - Daily schedule view
   - Activity status tracking
   - Export routine functionality

### 👨‍🏫 Teacher Features

1. **Dashboard**
   - Quick access to attendance and timetable
   - Rescheduler tool access

2. **Attendance Management**
   - Start attendance sessions
   - View student attendance records

3. **Class Rescheduler**
   - **Step 1: Setup**
     - Add multiple classes
     - Define subjects per class
     - Assign teachers to subjects
     - Set class average for each subject
     - Configure number of periods per day
     - Create timetable templates
   - **Step 2: Mark Absent Teachers**
     - Select absent teachers
     - Support for multiple absent teachers
   - **Step 3: Regenerate Schedule**
     - Automatic teacher reassignment
     - Priority-based assignment (most needy classes first)
     - Conflict detection and resolution
     - One teacher per period constraint
     - Preview regenerated timetable
     - CSV export functionality

### 👨‍💼 Admin Features

1. **User Management**
   - Add students and teachers by email
   - View all registered users
   - Remove users
   - User type filtering

2. **Batch Management**
   - Create new batches
   - Assign students to batches
   - View batch members
   - Remove students from batches
   - Delete batches

3. **Timetable Management**
   - Create weekly timetables for batches
   - Grid-based timetable editor
   - Time slot configuration
   - Subject and room assignment per day
   - Save and load batch timetables

4. **System Overview**
   - Monitor all users
   - View batch statistics
   - System health indicators

## 🛠️ Technology Stack

### Core Technologies
- **HTML5**: Structure and semantic markup
- **CSS3**: Styling with custom properties and animations
- **JavaScript (ES6+)**: Vanilla JavaScript, no frameworks

### External Libraries (CDN)
- **Tailwind CSS v3**: Utility-first CSS framework
- **Face-API.js v0.22.2**: Face detection and recognition
- **jsPDF v2.5.1**: PDF generation for timetable export
- **Font Awesome 6.4.0**: Icon library
- **Google Generative AI**: Gemini 2.5 Flash model for AI chat

### Deployment
- **Vercel**: Hosting platform with SPA routing configuration

### Storage
- **LocalStorage**: Browser-based data persistence
  - User profiles
  - Face descriptors
  - Attendance records
  - Timetables
  - Batch data
  - User lists

## 🏗️ Architecture

### Application Structure

```
index.html (5326 lines)
├── Head Section
│   ├── Meta tags & title
│   ├── External CDN links (Tailwind, Face-API, jsPDF, Font Awesome)
│   ├── Custom CSS styles
│   └── Gemini AI integration script
├── Body Section
│   ├── Role Selection Screen
│   ├── Student Dashboard
│   │   ├── Navigation Bar
│   │   ├── Sidebar (Desktop)
│   │   ├── Mobile Tabs
│   │   └── Main Content Area
│   │       ├── Authentication Screens
│   │       └── Dashboard Content (Tabs)
│   ├── Teacher Dashboard
│   ├── Admin Dashboard
│   └── Gemini Chat Widget
└── JavaScript Section
    ├── Gemini AI Initialization
    ├── Student Dashboard Functions
    ├── Teacher Dashboard Functions
    ├── Admin Dashboard Functions
    └── Global Utility Functions
```

### Data Flow

1. **User Registration**
   - Face capture → Face descriptor extraction → LocalStorage
   - Username input → Profile creation → Batch selection

2. **User Login**
   - Face detection → Descriptor matching → Location verification → Dashboard access

3. **Attendance Marking**
   - Liveness check → Face matching → Location check → Attendance record

4. **Timetable Loading**
   - Batch lookup → Timetable data retrieval → Rendering with status calculation

## 📁 Code Structure

### Main JavaScript Functions

#### Authentication & Face Recognition
- `loadModels()`: Loads Face-API.js models (TinyFaceDetector, FaceLandmark68Net, FaceRecognitionNet)
- `startVideo(video, canvas, mode)`: Initializes camera stream
- `detectFace(video, canvas, mode)`: Real-time face detection and drawing
- `performLivenessCheck(video, duration)`: Advanced liveness detection
- `calculateEAR(eyePoints)`: Eye Aspect Ratio calculation for blink detection
- `calculateVariance(values)`: Statistical variance calculation
- `getDistance(lat1, lon1, lat2, lon2)`: Haversine distance formula

#### Student Dashboard
- `initializeStudentDashboard()`: Main initialization function
- `initializeDashboard()`: Dashboard stats and schedule updates
- `updateDateTime()`: Real-time clock updates
- `updateDashboardStats()`: Attendance statistics calculation
- `updateDashboardSchedule()`: Today's schedule rendering
- `loadTimetableData()`: Load batch-specific timetable
- `saveTimetableData()`: Persist timetable changes
- `renderTimetable(day)`: Render timetable for specific day
- `getLectureStatusAndColor(entryTime, day)`: Calculate class status
- `exportTimetableToPDF()`: Generate PDF export
- `initSkills()`: Initialize skills management
- `addSkill()`: Add new skill to profile
- `removeSkill(skill)`: Remove skill from profile

#### Rescheduler System
- `addClass(initial)`: Add new class to rescheduler
- `removeClass(id)`: Remove class from rescheduler
- `renderClasses()`: Render all classes UI
- `renderTimetableInputs()`: Generate timetable input fields
- `refreshAbsentSelect()`: Update absent teacher dropdown
- `renderAbsentList()`: Display absent teachers
- `renderPreview()`: Preview current schedule
- `regenerateScheduleModel()`: Core rescheduling algorithm
- `showResult(model)`: Display regenerated schedule
- `bestTeacherForClass(classObj)`: Find optimal teacher assignment

#### Admin Dashboard
- `initializeAdminDashboard()`: Admin panel initialization
- `loadUsers()`: Load user data from LocalStorage
- `saveUsers(users)`: Persist user data
- `loadBatches()`: Load batch data
- `saveBatches(batches)`: Persist batch data
- `renderUsers()`: Display user list
- `createBatchFromInput()`: Create new batch
- `renderBatchSelect()`: Update batch dropdown
- `renderStudentsForBatch()`: Display students for batch assignment
- `renderSelectedStudents()`: Show selected students
- `loadBatchTimetables()`: Load batch timetable data
- `saveBatchTimetables(timetables)`: Persist batch timetables
- `addTimetableRow(time)`: Add row to timetable grid
- `saveTimetable()`: Save timetable to LocalStorage

#### Gemini AI Integration
- `sendMessageToGemini(userMessage)`: Send chat message
- `generateContent(prompt)`: Generate content from prompt
- `sendMessage()`: Chat widget message handler

#### Utility Functions
- `selectRole(role)`: Switch between user roles
- `logout()`: Clear session and return to role selection
- `switchTab(tabId)`: Programmatic tab switching
- `escapeHtml(s)`: XSS prevention
- `uid(prefix)`: Generate unique IDs
- `clamp(v, min, max)`: Number clamping
- `showMessage(msg)`: Display notification
- `hideMessage()`: Hide notification

### CSS Custom Properties

```css
:root {
    --bg: #0f1020;
    --card: #0b1220;
    --accent1: #6EE7B7;
    --accent2: #7C4DFF;
    --neon: #00FFE1;
    --glass: rgba(255, 255, 255, 0.04);
    --muted: #bfc7d6;
    --radius: 14px;
}
```

## 🚀 Installation & Setup

### Prerequisites
- Modern web browser (Chrome, Firefox, Safari, Edge)
- Webcam for face recognition
- GPS/Location services enabled
- Internet connection (for CDN resources)

### Local Development

1. **Clone the repository**
   ```bash
   git clone https://github.com/hemannt003/sparkuuu.git
   cd sparkuuu
   ```

2. **Serve locally**
   
   Using Python:
   ```bash
   python -m http.server 8000
   ```
   
   Using Node.js:
   ```bash
   npx serve
   ```
   
   Using PHP:
   ```bash
   php -S localhost:8000
   ```

3. **Open in browser**
   ```
   http://localhost:8000
   ```

### Vercel Deployment

1. **Connect repository to Vercel**
   - Import GitHub repository
   - Vercel auto-detects `vercel.json` configuration

2. **Deploy**
   - Automatic deployment on push
   - Custom domain configuration available

### First Time Setup

1. **Grant Permissions**
   - Camera access (required for face recognition)
   - Location access (required for attendance verification)

2. **Register as Student**
   - Select "Student" role
   - Click "Register"
   - Enter username
   - Position face in camera
   - Click "Register Face"
   - Select batch from dropdown
   - Confirm batch selection

3. **Register as Teacher/Admin**
   - Similar process without batch selection

## 📖 Usage Guide

### For Students

#### Marking Attendance
1. Navigate to **Attendance** tab
2. Wait for camera to initialize
3. Click **"Mark Attendance with Face Recognition"**
4. Follow liveness check instructions:
   - Move head naturally
   - Blink 2-3 times
   - Wait 6 seconds
5. System verifies:
   - Liveness (anti-spoofing)
   - Face match
   - Location (within 49m radius)
6. Attendance marked successfully ✅

#### Viewing Timetable
1. Go to **Timetable** tab
2. Select day (Mon-Fri)
3. View classes with real-time status
4. Export to PDF if needed

#### Managing Profile
1. Navigate to **Profile** tab
2. Upload profile photo
3. Edit personal information
4. Add/remove skills
5. Update career goals
6. Click **"Save Changes"**

### For Teachers

#### Rescheduling Classes
1. Navigate to **Rescheduler** tab
2. **Step 1**: Add classes and subjects
   - Click **"+ Add Class"**
   - Enter class name
   - Add subjects with teachers and averages
   - Configure periods per day
   - Create timetable template
3. **Step 2**: Mark absent teachers
   - Select teacher from dropdown
   - Click **"+ Add Absent"**
4. **Step 3**: Regenerate schedule
   - Click **"Regenerate Schedule"**
   - Preview results
   - Download CSV if needed

### For Admins

#### Creating Batches
1. Go to **Admin Dashboard**
2. Navigate to **Batch Management**
3. Enter batch name
4. Select students from list
5. Click **"Add to Batch"**

#### Managing Timetables
1. Select batch from dropdown
2. Use grid editor to add/remove rows
3. Enter time, subject, and room for each day
4. Click **"Save Timetable"**

#### User Management
1. Select user type (Student/Teacher)
2. Enter email address
3. Click **"Add User"**
4. View/remove users from list

## 🔌 API & Functions Reference

### Face Recognition API

#### `loadModels()`
Loads Face-API.js models asynchronously.
```javascript
await loadModels();
```

#### `startVideo(video, canvas, mode)`
Starts camera stream and initializes face detection.
- `video`: HTMLVideoElement
- `canvas`: HTMLCanvasElement
- `mode`: String ('register', 'login', 'attendance')

#### `performLivenessCheck(video, duration)`
Performs liveness detection to prevent spoofing.
- Returns: `Promise<{isLive: boolean, reason?: string}>`
- Duration: Default 6000ms (6 seconds)

### Location API

#### `getDistance(lat1, lon1, lat2, lon2)`
Calculates distance between two GPS coordinates using Haversine formula.
- Returns: Distance in meters

### LocalStorage API

#### User Data
- `userProfile`: Student profile information
- `adminUsers`: Admin-managed user list
- `adminBatches`: Batch assignments
- `batchTimetables`: Batch-specific timetables
- `timetableData`: Student timetable cache
- `attendanceData`: Attendance records

### Gemini AI API

#### `window.geminiAI.sendMessage(message)`
Sends message to Gemini AI chat.
```javascript
const response = await window.geminiAI.sendMessage("Hello");
```

#### `window.geminiAI.generate(prompt)`
Generates content from prompt (no chat history).
```javascript
const content = await window.geminiAI.generate("Explain face recognition");
```

## 💾 Data Storage

### LocalStorage Keys

| Key | Description | Structure |
|-----|-------------|-----------|
| `userProfile` | Current user profile | `{username, email, batch, photo, skills, goals}` |
| `adminUsers` | All registered users | `{students: [], teachers: []}` |
| `adminBatches` | Batch assignments | `{batchName: [email1, email2, ...]}` |
| `batchTimetables` | Batch timetables | `{batchName_day: [{time, subject, room}]}` |
| `timetableData` | Student timetable cache | `{Mon: [], Tue: [], ...}` |
| `attendanceData` | Attendance records | `[{date, class, time, method, status}]` |
| `faceDescriptors` | Face recognition data | `{username: descriptor}` |

### Data Format Examples

#### User Profile
```json
{
  "username": "john_doe",
  "email": "john@example.com",
  "batch": "CS2021",
  "photo": "data:image/jpeg;base64,...",
  "skills": ["JavaScript", "Python"],
  "goals": "Become a full-stack developer"
}
```

#### Batch Timetable
```json
{
  "CS2021_Mon": [
    {"time": "09:00", "subject": "Data Structures", "room": "302"},
    {"time": "10:00", "subject": "Algorithms", "room": "305"}
  ]
}
```

## 🔒 Security & Privacy

### Security Features

1. **Face Recognition**
   - Descriptors stored locally (not transmitted)
   - Confidence threshold: 0.6
   - Liveness detection prevents photo/video spoofing

2. **Location Verification**
   - GPS coordinates not stored
   - Only distance calculation performed
   - Configurable radius enforcement

3. **XSS Prevention**
   - All user input escaped with `escapeHtml()`
   - No `innerHTML` with user data
   - Content Security Policy ready

4. **Data Privacy**
   - All data stored locally in browser
   - No server-side data transmission
   - User controls all personal information

### Privacy Considerations

- **Face Data**: Stored in browser LocalStorage, never transmitted
- **Location Data**: Used only for verification, not stored
- **User Data**: All data remains on user's device
- **No Tracking**: No analytics or tracking scripts

## ⚙️ Configuration

### Classroom Location
Edit in `initializeStudentDashboard()`:
```javascript
const CLASSROOM_LOCATION = { lat: 22.681432, lon: 75.879018 };
const RADIUS_METERS = 49;
```

### Face Recognition Threshold
Adjust in login/attendance functions:
```javascript
const distance = faceapi.euclideanDistance(descriptor1, descriptor2);
if (distance >= 0.6) { // Adjust threshold here
    // Face doesn't match
}
```

### Gemini AI API Key
Located in head section (line 198):
```javascript
const API_KEY = "YOUR_API_KEY_HERE";
```

### Liveness Detection Parameters
Adjust in `performLivenessCheck()`:
```javascript
const minFrames = 20;           // Minimum frames required
const minMovement = 25;         // Minimum pixel movement
const minBlinks = 2;            // Required blinks
const blinkThreshold = 0.25;    // EAR threshold for blink
```

## 🐛 Troubleshooting

### Common Issues

#### Camera Not Working
- **Check permissions**: Browser settings → Site permissions → Camera
- **HTTPS required**: Some browsers require HTTPS for camera access
- **Try different browser**: Chrome/Firefox recommended

#### Face Recognition Fails
- **Lighting**: Ensure good lighting conditions
- **Face position**: Center face in camera view
- **Distance**: Stay 1-2 feet from camera
- **Glasses**: Remove if causing issues

#### Location Not Detected
- **Enable location**: Browser settings → Site permissions → Location
- **GPS enabled**: Check device GPS settings
- **Try different browser**: Some browsers have better GPS support

#### LocalStorage Full
- **Clear data**: Browser settings → Clear browsing data
- **Reduce data**: Remove old attendance records
- **Check quota**: LocalStorage limit is ~5-10MB

#### Models Not Loading
- **Internet connection**: Face-API.js models load from CDN
- **Check console**: Look for network errors
- **Refresh page**: Models load on page initialization

### Debug Mode

Enable console logging:
```javascript
// Already enabled in code
console.log('Face detected:', detections);
console.log('Distance:', distance);
console.log('Liveness:', livenessResult);
```

## 🎯 Future Enhancements

### Planned Features
- [ ] Backend integration for persistent storage
- [ ] Multi-device synchronization
- [ ] Advanced analytics and reporting
- [ ] Mobile app development (React Native)
- [ ] Email notifications
- [ ] Calendar integration (Google Calendar, Outlook)
- [ ] Multi-language support
- [ ] Offline mode with service workers
- [ ] Real-time collaboration features
- [ ] Advanced attendance analytics
- [ ] Export attendance reports (PDF/Excel)
- [ ] Integration with learning management systems

### Technical Improvements
- [ ] Code splitting and modularization
- [ ] TypeScript migration
- [ ] Unit and integration tests
- [ ] Performance optimization
- [ ] Progressive Web App (PWA) features
- [ ] Database integration (Firebase/Supabase)
- [ ] API backend development
- [ ] Authentication system enhancement

## 📝 Code Statistics

- **Total Lines**: 5,326
- **JavaScript Functions**: ~95+
- **CSS Lines**: ~500
- **HTML Structure**: ~2,000 lines
- **External Dependencies**: 5 (CDN)
- **LocalStorage Keys**: 7
- **User Roles**: 3 (Student, Teacher, Admin)
- **Main Tabs**: 7 (Student dashboard)

## 🤝 Contributing

Contributions are welcome! Please follow these steps:

1. Fork the repository
2. Create feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit changes (`git commit -m 'Add AmazingFeature'`)
4. Push to branch (`git push origin feature/AmazingFeature`)
5. Open Pull Request

### Code Style Guidelines
- Use meaningful variable names
- Add comments for complex logic
- Follow existing code structure
- Test on multiple browsers
- Ensure responsive design

## 📄 License

This project is open source and available under the [MIT License](LICENSE).

## 👥 Authors

- **Hemanth** - [@hemannt003](https://github.com/hemannt003)

## 🙏 Acknowledgments

- **Face-API.js**: Face detection and recognition capabilities
- **Tailwind CSS**: Beautiful utility-first CSS framework
- **Font Awesome**: Comprehensive icon library
- **jsPDF**: PDF generation functionality
- **Google Gemini AI**: AI chat assistant integration
- **Vercel**: Hosting and deployment platform

## 📞 Support

For support, email [your-email] or open an issue in the repository.

---

**Note**: This application requires camera and location permissions to function properly. Make sure to grant these permissions when prompted by your browser.

**Version**: 1.0.0  
**Last Updated**: 2024
>>>>>>> 74ecb2d (docs: Add comprehensive README.md with full code documentation)
