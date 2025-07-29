# Comprehensive Project Prompt: Task Manager with Role-Based Access Control

## Project Overview
Create a complete serverless task management system with comprehensive role-based access control using AWS services and React. The system MUST support three distinct user roles with different permission levels and SHALL provide secure, scalable task management capabilities.

**RFC 2119 Compliance**: The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in RFC 2119.

## Architecture Requirements

### Frontend Stack
- **Framework**: MUST use React SPA with TypeScript
- **Authentication**: MUST implement AWS Amplify with Cognito integration
- **UI Components**: SHOULD use modern, responsive design
- **State Management**: MAY use Context API or Redux for user state
- **Routing**: MUST implement React Router with protected routes based on user roles

### Backend Stack
- **API**: MUST use AWS API Gateway with Lambda integration
- **Functions**: MUST implement AWS Lambda (Node.js/TypeScript)
- **Authorization**: MUST implement custom Lambda authorizer for JWT validation
- **Database**: MUST use DynamoDB with optimized table design
- **Storage**: MUST use S3 for file attachments with presigned URLs
- **CORS**: MUST be properly configured for secure cross-origin requests

### Infrastructure
- **IaC**: MUST use AWS CDK (TypeScript) for complete infrastructure deployment
- **Security**: MUST implement IAM roles with least privilege principle
- **Monitoring**: MUST include CloudWatch logs and metrics
- **Environment**: SHOULD support dev/staging/prod environments

## User Role System

### Admin Role
- **Permissions**: MUST have full CRUD operations on all tasks
- **User Management**: MUST be able to create, modify, delete user accounts
- **System Configuration**: MUST be able to manage system settings and permissions
- **Reporting**: MUST have access to all system analytics and reports
- **Task Assignment**: MUST be able to assign tasks to any user

### Contributor Role
- **Task Management**: MUST be able to create and edit own tasks
- **Task Viewing**: MUST be able to view tasks assigned to them
- **File Operations**: MUST be able to upload and manage attachments
- **Collaboration**: MAY comment on assigned tasks
- **Status Updates**: SHOULD be able to update status of own and assigned tasks

### Viewer Role
- **Read-Only Access**: MUST have view access to assigned tasks only
- **No Modifications**: MUST NOT be able to create, edit, or delete tasks
- **Limited Viewing**: MUST only see tasks specifically assigned to them
- **Download Only**: MAY download attachments but MUST NOT upload

## Database Schema Design

### Tasks Table (DynamoDB)
```
Primary Key: taskId (String)
Attributes:
- title (String, required)
- description (String)
- status (String: "pending" | "in-progress" | "completed")
- assignedTo (String: Cognito User ID)
- createdBy (String: Cognito User ID)
- deadline (String: ISO date)
- priority (String: "low" | "medium" | "high")
- attachments (List: S3 object keys)
- createdAt (String: ISO timestamp)
- updatedAt (String: ISO timestamp)
- tags (List: String array)
- comments (List: Comment objects)

GSI: assignedTo-createdAt-index
GSI: createdBy-createdAt-index
GSI: status-deadline-index
```

### UserProfiles Table (DynamoDB)
```
Primary Key: userId (String: Cognito User ID)
Attributes:
- email (String, required)
- role (String: "Admin" | "Contributor" | "Viewer")
- permissions (List: Specific permission strings)
- createdAt (String: ISO timestamp)
- lastLogin (String: ISO timestamp)
- isActive (Boolean)
- preferences (Map: User preferences object)
```

## API Endpoints Specification

### Task Management Endpoints
```
GET /tasks
- Query Parameters: status, assignedTo, priority, limit, nextToken
- Authorization: All roles (filtered by permissions)
- Response: Paginated task list

POST /tasks
- Authorization: Admin, Contributor
- Body: Task creation payload
- Response: Created task object

GET /tasks/{taskId}
- Authorization: All roles (filtered by access)
- Response: Single task object

PUT /tasks/{taskId}
- Authorization: Admin, Contributor (own tasks only)
- Body: Task update payload
- Response: Updated task object

DELETE /tasks/{taskId}
- Authorization: Admin only
- Response: Deletion confirmation

POST /tasks/{taskId}/attachments
- Authorization: Admin, Contributor
- Body: Multipart file upload
- Response: S3 presigned URL and attachment metadata
```

### User Management Endpoints (Admin Only)
```
GET /users
- Authorization: Admin only
- Response: User list with roles

POST /users
- Authorization: Admin only
- Body: User creation payload
- Response: Created user object

PUT /users/{userId}/role
- Authorization: Admin only
- Body: Role assignment payload
- Response: Updated user object
```

## Security Implementation

### Authentication Flow
1. **User Registration**: MUST use Cognito user pool with email verification
2. **Login Process**: MAY use Cognito hosted UI or custom login form
3. **Token Management**: MUST implement JWT tokens with automatic refresh
4. **Session Handling**: MUST implement secure token storage and validation

### Authorization Strategy
1. **Lambda Authorizer**: MUST implement custom authorizer to validate JWT tokens
2. **Role Verification**: MUST check user's Cognito group membership
3. **Resource Access**: MUST validate user permissions for specific resources
4. **API Gateway**: MUST integrate with custom authorizer for all endpoints

### Data Security
1. **Encryption**: MUST implement DynamoDB encryption at rest
2. **S3 Security**: MUST implement bucket policies with restricted access
3. **HTTPS Only**: MUST use HTTPS for all API communications over TLS
4. **Input Validation**: MUST implement comprehensive input sanitization

## Frontend Implementation Requirements

### Component Structure
```
src/
├── components/
│   ├── auth/
│   │   ├── LoginForm.tsx
│   │   ├── ProtectedRoute.tsx
│   │   └── RoleGuard.tsx
│   ├── tasks/
│   │   ├── TaskList.tsx
│   │   ├── TaskCard.tsx
│   │   ├── TaskForm.tsx
│   │   └── TaskDetails.tsx
│   ├── users/
│   │   ├── UserManagement.tsx
│   │   └── UserProfile.tsx
│   └── common/
│       ├── Header.tsx
│       ├── Sidebar.tsx
│       └── LoadingSpinner.tsx
├── contexts/
│   ├── AuthContext.tsx
│   └── TaskContext.tsx
├── hooks/
│   ├── useAuth.ts
│   ├── useTasks.ts
│   └── useRole.ts
├── services/
│   ├── api.ts
│   ├── auth.ts
│   └── storage.ts
└── types/
    ├── Task.ts
    ├── User.ts
    └── Auth.ts
```

### Key Features to Implement
1. **Dashboard**: MUST implement role-specific dashboard with relevant metrics
2. **Task Management**: MUST implement CRUD operations with role-based restrictions
3. **File Upload**: SHOULD implement drag-and-drop file upload with progress indicators
4. **Real-time Updates**: MAY implement WebSocket or polling for live task updates
5. **Search & Filter**: SHOULD implement advanced search with multiple filter options
6. **Responsive Design**: MUST implement mobile-first responsive layout
7. **Error Handling**: MUST implement comprehensive error boundaries and user feedback

## Backend Implementation Requirements

### Lambda Functions Structure
```
backend/
├── src/
│   ├── handlers/
│   │   ├── tasks/
│   │   │   ├── createTask.ts
│   │   │   ├── getTasks.ts
│   │   │   ├── updateTask.ts
│   │   │   └── deleteTask.ts
│   │   ├── users/
│   │   │   ├── getUsers.ts
│   │   │   └── updateUserRole.ts
│   │   └── auth/
│   │       └── authorizer.ts
│   ├── services/
│   │   ├── dynamodb.ts
│   │   ├── s3.ts
│   │   └── cognito.ts
│   ├── utils/
│   │   ├── response.ts
│   │   ├── validation.ts
│   │   └── permissions.ts
│   └── types/
│       ├── api.ts
│       └── database.ts
```

### Key Backend Features
1. **Input Validation**: MUST use Joi or similar for request validation
2. **Error Handling**: MUST implement standardized error responses
3. **Logging**: MUST implement structured logging with correlation IDs
4. **Performance**: MUST implement optimized DynamoDB queries with proper indexing
5. **Rate Limiting**: SHOULD implement API Gateway throttling configuration
6. **Monitoring**: MUST implement CloudWatch metrics and alarms

## Infrastructure as Code (CDK)

### Stack Organization
```
infrastructure/
├── lib/
│   ├── stacks/
│   │   ├── auth-stack.ts          # Cognito resources
│   │   ├── database-stack.ts      # DynamoDB tables
│   │   ├── api-stack.ts          # API Gateway + Lambda
│   │   ├── storage-stack.ts      # S3 buckets
│   │   └── monitoring-stack.ts   # CloudWatch resources
│   ├── constructs/
│   │   ├── lambda-function.ts
│   │   └── dynamodb-table.ts
│   └── task-manager-app.ts
└── bin/
    └── task-manager.ts
```

### CDK Implementation Requirements
1. **Environment Configuration**: MUST support multiple environments
2. **Resource Tagging**: MUST implement consistent tagging strategy
3. **IAM Policies**: MUST follow least privilege access patterns
4. **Outputs**: SHOULD export important resource ARNs and URLs
5. **Dependencies**: MUST implement proper stack dependencies and cross-references

## Development Workflow

### Setup Instructions
1. **Prerequisites**: MUST have AWS CLI, Node.js 18+, CDK CLI
2. **Installation**: MUST support multi-package dependency installation
3. **Configuration**: MUST provide environment-specific configuration files
4. **Deployment**: SHOULD implement automated deployment pipeline
5. **Testing**: MUST include unit and integration test setup

### Quality Assurance
1. **Code Standards**: MUST implement ESLint, Prettier configuration
2. **Type Safety**: MUST use strict TypeScript configuration
3. **Testing**: MUST use Jest for unit tests, MAY use Cypress for E2E
4. **Security**: SHOULD implement regular dependency vulnerability scans
5. **Performance**: SHOULD implement bundle size optimization and monitoring

## Success Criteria

### Functional Requirements (MUST be implemented)
- [ ] Users MUST be able to register and login with email verification
- [ ] Role-based access control MUST be fully functional
- [ ] Admin users MUST have full system access
- [ ] Contributor users MUST be able to manage their own tasks
- [ ] Viewer users MUST have read-only access to assigned tasks
- [ ] File upload and download MUST work securely
- [ ] API endpoints MUST implement proper authentication and authorization

### Non-Functional Requirements
- [ ] System MUST respond within 2 seconds for API calls
- [ ] Application MUST be mobile responsive
- [ ] All data MUST be encrypted at rest and in transit
- [ ] System SHOULD support 1000+ concurrent users
- [ ] Code coverage MUST be at least 80%
- [ ] All AWS resources MUST follow least privilege principles control works correctly for all endpoints
- [ ] Tasks can be created, viewed, updated, and deleted based on permissions
- [ ] File attachments upload and download successfully
- [ ] Admin users can manage other users and their roles
- [ ] All data is properly validated and sanitized

### Non-Functional Requirements
- [ ] API response times under 500ms for 95th percentile
- [ ] Frontend loads within 3 seconds on 3G connection
- [ ] System handles 1000+ concurrent users
- [ ] 99.9% uptime availability
- [ ] All sensitive data encrypted in transit and at rest
- [ ] WCAG 2.1 AA accessibility compliance

### Security Requirements
- [ ] No unauthorized access to tasks or user data
- [ ] All API endpoints properly authenticated and authorized
- [ ] Input validation prevents injection attacks
- [ ] File uploads are scanned and restricted by type/size
- [ ] Audit logging for all administrative actions
