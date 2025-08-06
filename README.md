# Fowbit - Personal Planner & Tracker

A comprehensive personal productivity application built with Vue.js and Node.js that combines task management, expense tracking, and mood monitoring in a beautiful, intuitive interface.

## 🌟 Features

### 📅 Multi-View Planning

- **Day View**: Detailed daily overview with tasks, expenses, and mood tracking
- **Week View**: Seven-day perspective for weekly planning
- **Month View**: Monthly calendar with data visualization
- **Timeline View**: Chronological view of all activities

### ✅ Task Management

- Create, edit, and delete tasks
- Drag-and-drop task organization with vuedraggable
- Task status tracking and completion
- Date-based task organization

### 💰 Expense Tracking

- Record daily expenses with categories
- Set and monitor daily/monthly budgets
- Visual expense summaries and totals
- Category-based expense organization

### 😊 Mood Tracking

- Daily mood logging with emoji indicators
- 5-point mood scale (Very Bad to Excellent)
- Historical mood data visualization
- Mood trends and insights

### 🔐 User Authentication

- Secure user registration and login
- JWT-based authentication
- Protected routes and data
- User session management

### 🎨 Modern UI/UX

- Responsive design with Tailwind CSS
- Beautiful icons with Heroicons
- Toast notifications for user feedback
- Modal dialogs for better user experience

## 🛠️ Tech Stack

### Frontend

- **Vue.js 3** - Progressive JavaScript framework
- **Vite** - Fast build tool and development server
- **Tailwind CSS 4** - Utility-first CSS framework
- **Heroicons** - Beautiful hand-crafted SVG icons
- **Axios** - HTTP client for API requests
- **Vuedraggable** - Vue drag-and-drop component

### Backend

- **Node.js** - JavaScript runtime
- **Express.js** - Web application framework
- **MongoDB** - NoSQL database
- **Mongoose** - MongoDB object modeling
- **JWT** - JSON Web Tokens for authentication
- **bcryptjs** - Password hashing
- **CORS** - Cross-origin resource sharing
- **Rate Limiting** - API protection

### DevOps

- **Docker** - Containerization
- **Docker Compose** - Multi-container orchestration
- **Nginx** - Web server for frontend
- **MongoDB** - Database container

## 📁 Project Structure

```
fowbit-vue-app/
├── frontend/                 # Vue.js frontend application
│   ├── src/
│   │   ├── components/      # Reusable Vue components
│   │   ├── composables/     # Vue composition functions
│   │   ├── utils/           # Utility functions
│   │   ├── views/           # Page components
│   │   ├── App.vue          # Root component
│   │   └── main.js          # Application entry point
│   ├── public/              # Static assets
│   ├── Dockerfile           # Frontend Docker configuration
│   └── package.json         # Frontend dependencies
├── backend/                 # Node.js backend API
│   ├── middleware/          # Express middleware
│   ├── models/              # MongoDB data models
│   ├── routes/              # API route handlers
│   ├── server.js            # Server entry point
│   ├── Dockerfile           # Backend Docker configuration
│   └── package.json         # Backend dependencies
└── docker-compose.yml       # Multi-container setup
```

## 🚀 Getting Started

### Prerequisites

- Node.js (v18 or higher)
- Docker and Docker Compose
- MongoDB (if running locally)

### Quick Start with Docker

1. **Clone the repository**

   ```bash
   git clone <repository-url>
   cd fowbit-vue-app
   ```

2. **Set up environment variables**
   Create a `.env` file in the root directory:

   ```env
   MONGODB_URI=mongodb://mongodb:27017/fowbit
   JWT_SECRET=your-super-secret-jwt-key
   ```

3. **Start the application**

   ```bash
   docker-compose up -d
   ```

4. **Access the application**
   - Frontend: http://localhost:3000
   - Backend API: http://localhost:5000
   - MongoDB: localhost:27017

### Local Development Setup

#### Backend Setup

```bash
cd backend
npm install
npm run dev
```

#### Frontend Setup

```bash
cd frontend
npm install
npm run dev
```

## 📚 API Documentation

### Authentication Endpoints

- `POST /api/auth/register` - User registration
- `POST /api/auth/login` - User login

### Task Endpoints

- `GET /api/tasks` - Get user tasks
- `POST /api/tasks` - Create new task
- `PUT /api/tasks/:id` - Update task
- `DELETE /api/tasks/:id` - Delete task

### Expense Endpoints

- `GET /api/expenses` - Get user expenses
- `POST /api/expenses` - Create new expense
- `PUT /api/expenses/:id` - Update expense
- `DELETE /api/expenses/:id` - Delete expense

### Mood Endpoints

- `GET /api/moods` - Get user moods
- `POST /api/moods` - Record mood
- `PUT /api/moods/:id` - Update mood

## 🔧 Configuration

### Environment Variables

#### Backend

- `PORT` - Server port (default: 5000)
- `MONGODB_URI` - MongoDB connection string
- `JWT_SECRET` - Secret key for JWT tokens
- `NODE_ENV` - Environment (development/production)

#### Frontend

- Built-time configuration in `vite.config.js`
- API base URL configured in axios setup

## 🐳 Docker Configuration

The application uses multi-stage Docker builds for optimized production images:

- **Frontend**: Nginx-based serving of built Vue.js application
- **Backend**: Node.js application with Express server
- **Database**: MongoDB container with persistent volume

## 📱 Features in Detail

### Task Management

- Intuitive task creation with date assignment
- Drag-and-drop task reordering
- Task completion tracking
- Task filtering and search

### Expense Tracking

- Categorized expense logging
- Budget setting and monitoring
- Daily and monthly expense summaries
- Visual expense charts and insights

### Mood Tracking

- Simple 5-point mood scale
- Daily mood logging
- Historical mood data
- Mood pattern analysis

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📄 License

This project is licensed under the MIT License - see the LICENSE file for details.

## 🙏 Acknowledgments

- Vue.js team for the amazing framework
- Tailwind CSS for the utility-first CSS approach
- Heroicons for beautiful iconography
- MongoDB team for the flexible database solution

## 📞 Support

For support, email support@fowbit.com or open an issue in the repository.

---

**Built with ❤️ using Vue.js and Node.js**
