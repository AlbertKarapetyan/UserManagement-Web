# User Management WEB

A Vue.js application for managing users, featuring modern web development technologies.

## Project Setup

### Installation
1. Clone the repository:
```bash
git clone https://github.com/your-username/User-management-WEB.git
```

2. Navigate to the project directory
```bash
cd User-management-WEB
```

3. Install dependencies:
```bash
npm install
```

## Development Server
Start the development server:
```bash
npm run dev
```

## By default, the application runs at http://localhost:5173.

Configuration
The config.js file in the root directory contains the API base URL:

```javascript
const config = {
  apiBaseUrl: "http://localhost:5223",
};

export default config;
```
Update the apiBaseUrl as needed for your environment.

## Technologies Used
    1. Vue 3
    2. Vue Router
    3. Axios

## Features
    1. User management interface
    2. Router-enabled navigation
    3. Axios integration for API communication