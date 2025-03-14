
# Cortex - Intelligent Workspace Platform

[Cortex](https://www.cortexx.info/)

## Overview

Cortex is an intelligent workspace platform that integrates document editing, data analysis, and AI-powered insights into a unified interface. The platform enables users to work with various file types (Excel, Markdown, Python, Chat) while providing real-time AI assistance and insights.

### Key Features

-   Multi-file type workspace (Excel, Markdown, Python, Chat)
-   Real-time AI insights and suggestions
-   Integrated file explorer and project management
-   Collaborative editing capabilities
-   Intelligent data analysis

## System Architecture

Cortex follows a modern client-server architecture with a React-based frontend and a RESTful API backend.

### High-Level Architecture
![HLA](/mermaid-diagram-2025-03-11-184958.png)

    

### Data Flow
![Data Flow](/mermaid-diagram-2025-03-11-185553.png)

### Component Structure

The frontend is organized into the following main components:

- **CortexInterface**: Main container component
- **IconSidebar**: Left navigation sidebar
- **FileExplorer**: File and project explorer
- **TabBar**: File tabs management
- **ContentContainer**: Content display area
- **SideChat**: AI assistant sidebar
- **ConsolePanel**: Console output display


## API Integrations

Cortex requires several API integrations to function properly. All APIs are RESTful and use JSON for data exchange.

### 1. Workspace API

**Purpose**: Manages workspace, projects, and files metadata

**Base URL**: `${API_BASE_URL}/workspace`

**Authentication**: Bearer token

**Endpoints**:

| Endpoint | Method | Description | Request Body | Response
|-----|-----|-----|-----|-----
| `/workspace` | GET | Get workspace data | - | Workspace object
| `/files/:id` | GET | Get file by ID | - | File object with content
| `/files/:id` | PUT | Update file content | `{ content: any }` | `{ success: boolean }`
| `/files/:id` | DELETE | Delete file | - | `{ success: boolean }`
| `/projects/:id/files` | POST | Create new file | File object | Created file object


**Example Response**:

```json
{
  "name": "CORTEX WORKSPACE",
  "projects": [
    {
      "id": "project-1",
      "name": "Project Alpha",
      "files": [],
      "isOpen": false
    },
    {
      "id": "project-2",
      "name": "Project Beta",
      "files": [
        {
          "id": "excel-1",
          "name": "DealAnalysis.xlsx",
          "type": "excel",
          "icon": "FileSpreadsheet",
          "iconColor": "text-yellow-500"
        }
      ],
      "isOpen": true
    }
  ]
}
```

### 2. AI Processing API

**Purpose**: Handles AI-powered processing of different content types

**Base URL**: `${API_BASE_URL}/ai`

**Authentication**: Bearer token

**Endpoints**:

| Endpoint | Method | Description | Request Body | Response
|-----|-----|-----|-----|-----
| `/ai/code` | POST | Process Python code | `{ code: string, action: string }` | `{ processedCode: string }`
| `/ai/markdown` | POST | Process Markdown | `{ markdown: string, action: string }` | `{ processedMarkdown: string }`
| `/ai/insights/:id` | GET | Get AI insights for file | - | Array of insight objects
| `/ai/analyze` | POST | Analyze data | `{ data: any }` | `{ analysis: string, insights: any[] }`


**Example Request**:

```json
{
  "code": "def calculate_metrics(df):\n    # Calculate metrics\n    return df",
  "action": "optimize"
}
```

**Example Response**:

```json
{
  "processedCode": "def calculate_metrics(df):\n    # Vectorized operations for better performance\n    return df"
}
```

### 3. Console API

**Purpose**: Manages console logs and code execution

**Base URL**: `${API_BASE_URL}/console`

**Authentication**: Bearer token

**Endpoints**:

| Endpoint | Method | Description | Request Body | Response
|-----|-----|-----|-----|-----
| `/console/logs` | GET | Get console logs | - | Array of log objects
| `/console/execute` | POST | Execute code | `{ code: string }` | `{ success: boolean, output: string }`


**Example Response**:

```json
[
  {
    "id": "msg-1",
    "type": "info",
    "message": "Processing code with AI (optimize)...",
    "timestamp": "2023-05-15T14:30:45.123Z"
  },
  {
    "id": "msg-2",
    "type": "output",
    "message": "Code execution complete",
    "timestamp": "2023-05-15T14:30:47.456Z"
  }
]
```

### 4. Authentication API (To Be Implemented)

**Purpose**: Handles user authentication and authorization

**Base URL**: `${API_BASE_URL}/auth`

**Endpoints**:

| Endpoint | Method | Description | Request Body | Response
|-----|-----|-----|-----|-----
| `/auth/login` | POST | User login | `{ email: string, password: string }` | `{ token: string, user: User }`
| `/auth/logout` | POST | User logout | - | `{ success: boolean }`
| `/auth/refresh` | POST | Refresh token | `{ refreshToken: string }` | `{ token: string }`
| `/auth/user` | GET | Get current user | - | User object


## Technologies

### Frontend

- **Next.js**: React framework for server-side rendering and routing
- **TypeScript**: Type-safe JavaScript
- **Tailwind CSS**: Utility-first CSS framework
- **shadcn/ui**: Component library based on Radix UI
- **Lucide React**: Icon library
- **Context API**: State management


### Backend (To Be Implemented)

- **Node.js**: JavaScript runtime
- **Express**: Web framework
- **PostgreSQL**: Relational database
- **Prisma**: ORM for database access
- **JWT**: Authentication mechanism


### AI Services (To Be Implemented)

- **OpenAI API**: For code and text processing
- **Hugging Face**: For specialized AI models
- **TensorFlow.js**: For client-side AI processing


## Setup and Configuration

### Prerequisites

- Node.js 18.x or higher
- npm 9.x or higher
- Git


### Environment Variables

Create a `.env.local` file in the root directory with the following variables:

```plaintext
NEXT_PUBLIC_API_BASE_URL=http://localhost:3001/api
NEXT_PUBLIC_APP_ENV=development
```

For production, set these variables in your deployment environment:

```plaintext
NEXT_PUBLIC_API_BASE_URL=https://api.cortex-app.com/api
NEXT_PUBLIC_APP_ENV=production
```

### Installation

1. Clone the repository:

```shellscript
git clone https://github.com/your-org/cortex.git
cd cortex
```


2. Install dependencies:

```shellscript
npm install
```


3. Start the development server:

```shellscript
npm run dev
```


4. Open [http://localhost:3000](http://localhost:3000) in your browser.


## Development Workflow

### Code Structure

```plaintext
cortex/
├── app/                  # Next.js app directory
│   ├── api/              # API route handlers
│   ├── layout.tsx        # Root layout
│   └── page.tsx          # Home page
├── components/           # React components
│   ├── content/          # Content display components
│   ├── sidebar/          # Sidebar components
│   ├── tabs/             # Tab components
│   ├── console/          # Console components
│   ├── dashboard/        # Dashboard components
│   ├── ui/               # UI components
│   └── CortexInterface.tsx # Main interface component
├── context/              # React context providers
│   └── CortexContext.tsx # Main context provider
├── data/                 # Mock data
│   └── initialData.ts    # Initial data for development
├── hooks/                # Custom React hooks
│   ├── useFileContent.ts # File content management hook
│   └── useAIProcessing.ts # AI processing hook
├── lib/                  # Utility libraries
│   └── utils.ts          # Utility functions
├── public/               # Static assets
├── services/             # API services
│   ├── api.ts            # API client
│   └── mock-api.ts       # Mock API for development
├── types/                # TypeScript type definitions
│   └── index.ts          # Type definitions
└── utils/                # Utility functions
    ├── error-handling.ts # Error handling utilities
    └── data-transformations.ts # Data transformation utilities
```

### Branch Strategy

- `main`: Production-ready code
- `develop`: Integration branch for features
- `feature/*`: Feature branches
- `bugfix/*`: Bug fix branches
- `release/*`: Release preparation branches


### Pull Request Process

1. Create a feature branch from `develop`
2. Implement your changes
3. Write tests for your changes
4. Submit a pull request to `develop`
5. Get code review and approval
6. Merge to `develop`


## Deployment

### Staging Environment

- URL: [https://staging.cortex-app.com](https://staging.cortex-app.com)
- Automatically deployed from the `develop` branch
- Environment variables are set in Vercel project settings


### Production Environment

- URL: [https://cortex-app.com](https://cortex-app.com)
- Manually deployed from the `main` branch after QA
- Environment variables are set in Vercel project settings


### Deployment Process

1. Merge `develop` into `release/vX.Y.Z`
2. Test the release branch
3. Create a pull request to `main`
4. After approval, merge to `main`
5. Tag the release in Git
6. Deploy to production


## Contributing

### Code Standards

- Follow the TypeScript style guide
- Use functional components with hooks
- Write unit tests for all new features
- Document all public functions and components
- Use conventional commits for commit messages


### Documentation

- Update README.md for major changes
- Document all API endpoints
- Add JSDoc comments to functions and components
- Update diagrams when architecture changes


### Testing

- Write unit tests with Jest and React Testing Library
- Run tests before submitting pull requests:

```shellscript
npm test
```


- Ensure all tests pass before merging


---

## Contact

For questions or support, contact the team at:

- Email: [team@cortex-app.com](mailto:team@cortex-app.com)
- Slack: #cortex-dev
