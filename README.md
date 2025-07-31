# Task Manager RBAC - Prompt Engineering Repository

This repository contains comprehensive prompt engineering documentation for building a serverless task management system with role-based access control using Amazon Q CLI and AWS services.

> 🚀 **Live Implementation**: See the actual working application at [task-manager-role-based-access](https://github.com/ccmelvin/task-manager-role-based-access)

## 📋 What's Inside

This repository demonstrates how to use structured prompts and formal specifications to generate enterprise-grade applications with AI assistance.

### 📄 Documentation Files

- **[DETAILED_PROJECT_PROMPT.md](./DETAILED_PROJECT_PROMPT.md)** - Complete project specification with RFC 2119 compliant requirements for AI input
- **[RFC2119_REQUIREMENTS.md](./RFC2119_REQUIREMENTS.md)** - Formal requirements document using RFC 2119 terminology (MUST, SHOULD, MAY)

## 🎯 Prompt Engineering Approach

This project demonstrates how to:

### 1. **Structured Requirements**
- Use RFC 2119 terminology for precise AI instructions
- Define clear role-based permissions (Admin, Contributor, Viewer)
- Specify exact technical requirements for each component

### 2. **Architecture-First Prompting**
- Break complex systems into manageable components
- Define AWS service requirements upfront
- Ensure security and scalability from the start

### 3. **Role-Based System Design**
- **Admin**: Full CRUD operations, user management, system configuration
- **Contributor**: Own task management, assigned task access, file operations
- **Viewer**: Read-only access to assigned tasks only

## 🏗️ Generated System Architecture

The prompts in this repository generate:

- **Frontend**: React SPA with TypeScript, AWS Amplify
- **Backend**: AWS Lambda, API Gateway, DynamoDB
- **Infrastructure**: AWS CDK (TypeScript)
- **Authentication**: AWS Cognito with JWT tokens
- **Storage**: Amazon S3 for file attachments
- **Security**: Custom Lambda authorizers, encryption, IAM policies

## 🔧 How to Use These Prompts

1. **Start with the detailed prompt** - Use `DETAILED_PROJECT_PROMPT.md` as input to Amazon Q CLI
2. **Reference RFC 2119 requirements** - Use formal specifications for precise AI guidance
3. **Generate incrementally** - Build system components step by step
4. **Validate outputs** - Ensure generated code meets all MUST requirements

### Example Usage with Amazon Q CLI

```bash
# Use the detailed prompt as input
q generate --input DETAILED_PROJECT_PROMPT.md --type infrastructure

# Generate specific components
q generate --input "Create Lambda authorizer based on User Role System section"

# Validate against requirements
q validate --requirements RFC2119_REQUIREMENTS.md
```

## 📊 Prompt Engineering Results

Using these structured prompts with Amazon Q CLI generates:

- ✅ **Complete AWS CDK Infrastructure** (200+ lines)
- ✅ **Production-ready Lambda Functions** with proper error handling
- ✅ **Type-safe React Components** with role-based rendering
- ✅ **Comprehensive Security Implementation** following AWS best practices
- ✅ **Formal Documentation** and API specifications

## 🎓 Key Learnings

### Effective Prompt Patterns
1. **Constraint-Based Prompting** - Define what roles MUST NOT do
2. **Security-First Approach** - Lead with security requirements
3. **Formal Specifications** - Use RFC 2119 for precision
4. **Incremental Building** - Layer complex systems through connected prompts

### Best Practices
- Start with comprehensive requirements documents
- Use formal specification languages (RFC 2119, OpenAPI)
- Define role boundaries explicitly
- Validate AI outputs against requirements

## 🔗 Related Resources

- **[Working Implementation](https://github.com/ccmelvin/task-manager-role-based-access)** - See the generated code in action
- **[AWS CDK Documentation](https://docs.aws.amazon.com/cdk/)** - Infrastructure as Code reference
- **[RFC 2119](https://tools.ietf.org/html/rfc2119)** - Requirement levels specification
- **[Amazon Q CLI Documentation](https://docs.aws.amazon.com/amazonq/)** - AI-assisted development guide

## 🤝 Contributing

Contributions to improve the prompt engineering approach are welcome:

1. Review the existing prompt structures
2. Test prompts with Amazon Q CLI
3. Suggest improvements to requirement specifications
4. Share results and learnings

## 📄 License

This project is licensed under the MIT License.

---

**Note**: This repository focuses on prompt engineering and AI-assisted development. For the actual implementation and deployable code, visit the [main project repository](https://github.com/ccmelvin/task-manager-role-based-access).