# TicketPro - Vue 3 Ticket Management System

A modern, responsive ticket management application built with Vue 3, TypeScript, and Tailwind CSS. This application allows users to create, manage, and resolve support tickets efficiently.

## Features

- **Landing Page**: Beautiful landing page with decorative elements and call-to-action buttons
- **User Authentication**: Login and signup forms with validation
- **Dashboard**: Overview of ticket statistics with visual cards
- **Ticket Management**: Comprehensive ticket table with status indicators
- **Responsive Design**: Mobile-friendly interface that works on all devices
- **Modern UI**: Clean, professional design with smooth transitions

## Technology Stack

- **Vue 3**: Progressive JavaScript framework with Composition API
- **TypeScript**: Type-safe JavaScript for better development experience
- **Tailwind CSS**: Utility-first CSS framework for rapid styling
- **Vite**: Fast build tool and development server
- **Vue Router**: Client-side routing (configured but not actively used in current implementation)

## Project Structure

```
src/
├── App.vue              # Main application component
├── main.ts              # Application entry point
├── style.css            # Global styles with Tailwind directives
├── components/          # Reusable Vue components
│   └── HelloWorld.vue   # Default Vue component (can be removed)
├── views/              # Page components
│   └── Dashboard.vue   # Dashboard view (not actively used)
├── router/             # Vue Router configuration
│   └── index.ts        # Router setup
└── assets/             # Static assets
    └── vue.svg         # Vue logo
```

## Getting Started

### Prerequisites

- Node.js (version 16 or higher)
- npm or yarn package manager

### Installation

1. Clone the repository:
```bash
git clone <repository-url>
cd ticket_management_vue
```

2. Install dependencies:
```bash
npm install
```

3. Start the development server:
```bash
npm run dev
```

4. Open your browser and navigate to `http://localhost:5173`

### Building for Production

```bash
npm run build
```

The built files will be in the `dist` directory.

## Usage

### Navigation

The application uses a simple state-based navigation system:

- **Landing Page**: Initial page with login/signup options
- **Login**: User authentication form
- **Signup**: New user registration form
- **Dashboard**: Ticket statistics overview
- **Tickets**: Detailed ticket management interface

### Features

#### Landing Page
- Eye-catching gradient background with wave effect
- Decorative circular elements
- Clear call-to-action buttons for login and signup

#### Authentication
- Form validation for email and password fields
- Seamless navigation between login and signup
- Error handling for incomplete form submissions

#### Dashboard
- Four key metric cards showing:
  - Total Tickets
  - Open Tickets
  - In Progress Tickets
  - Closed Tickets
- Responsive grid layout that adapts to screen size

#### Ticket Management
- Comprehensive ticket table with:
  - Ticket ID
  - Subject description
  - Priority level (High, Medium, Low)
  - Assigned user
  - Current status
  - Last updated timestamp
- Color-coded status badges for quick visual identification
- Hover effects for better user interaction

## Styling

The application uses Tailwind CSS for styling with the following design principles:

- **Consistent Color Scheme**: Blue primary color with gray accents
- **Responsive Design**: Mobile-first approach with breakpoints
- **Accessibility**: Proper contrast ratios and semantic HTML
- **Modern UI Elements**: Shadows, rounded corners, and smooth transitions

## State Management

The application uses Vue 3's Composition API for state management:

- `ref()` for primitive values (current page)
- `reactive()` for complex objects (form data)
- Computed properties for derived state
- Event handlers for user interactions

## Development

### Adding New Pages

1. Add the new page to the code in `App.vue`
2. Update the `page` ref to include the new page option
3. Add navigation logic in the appropriate components
4. Style the new page using Tailwind CSS classes

### Customizing Styles

1. Modify `tailwind.config.js` for theme customization
2. Add custom utility classes in `src/style.css`
3. Use component-scoped styles for specific components

### Form Validation

The application includes basic form validation. To enhance:

1. Add more sophisticated validation rules
2. Implement real-time validation feedback
3. Add server-side validation integration

