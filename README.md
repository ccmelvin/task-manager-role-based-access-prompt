# Task Manager with Role-Based Access Control

A comprehensive serverless task management system built with AWS services and React, featuring role-based access control for Admin, Contributor, and Viewer users.

## 📋 Project Documents

- **[DETAILED_PROJECT_PROMPT.md](./DETAILED_PROJECT_PROMPT.md)** - Complete project specifications with RFC 2119 compliant requirements
- **[RFC2119_REQUIREMENTS.md](./RFC2119_REQUIREMENTS.md)** - Formal requirements document using RFC 2119 terminology

## 🏗️ Architecture

- **Frontend**: React SPA with TypeScript, AWS Amplify
- **Backend**: AWS Lambda, API Gateway, DynamoDB
- **Infrastructure**: AWS CDK (TypeScript)
- **Authentication**: AWS Cognito with JWT tokens
- **Storage**: Amazon S3 for file attachments

## 👥 User Roles

| Role | Permissions |
|------|-------------|
| **Admin** | Full CRUD on all tasks, user management, system configuration |
| **Contributor** | Create/edit own tasks, view assigned tasks, file uploads |
| **Viewer** | Read-only access to assigned tasks, download attachments |

## 🚀 Getting Started

### Prerequisites
- AWS CLI configured
- Node.js 18+
- AWS CDK CLI
- TypeScript

### Installation
```bash
# Clone the repository
git clone <repository-url>
cd task-manager-roles-prompt

# Install dependencies
npm install

# Deploy infrastructure
cdk deploy

# Start development server
npm run dev
```

## 📚 Key Features

- ✅ Role-based access control
- ✅ Secure file upload/download
- ✅ Real-time task updates
- ✅ Mobile-responsive design
- ✅ Comprehensive audit logging
- ✅ Multi-environment support

## 🔒 Security

- JWT token authentication
- Custom Lambda authorizer
- Encryption at rest and in transit
- IAM least privilege principle
- Input validation and sanitization

## 📖 Documentation

Refer to the project documents for detailed specifications:
- Technical requirements and architecture details
- API endpoint specifications
- Database schema design
- Security implementation guidelines
- Testing and deployment procedures

## 🤝 Contributing

1. Review the RFC 2119 requirements document
2. Follow the coding standards outlined in the project prompt
3. Ensure all MUST requirements are implemented
4. Test thoroughly before submitting changes

## 📄 License

This project is licensed under the MIT License.