# Employee Management Backend Server

A Node.js backend server template for employee management with three-tier role-based authorization system.

## Features

- **Three-tier Authorization System**
  - **Admin**: Full access (Create, Read, Update, Delete)
  - **Editor**: Limited access (Create, Read)
  - **User**: Read-only access
- **JSON File-based Data Storage**
- **Employee Data Management** (ID, First Name, Last Name)
- **Bearer Token Authentication**
- **RESTful API Design**

## Authorization Tiers

### 🔴 Admin
- View all employee data
- Add new employees
- Edit existing employee records
- Delete employee records

### 🟡 Editor
- View all employee data
- Add new employees
- Edit existing employee records
- **Cannot** delete existing records

### 🟢 User
- View employee data only
- **Cannot** perform any modifications

## API Endpoints

### Base URL
- **Local Development**: `http://localhost:3000`
- **Production**: `https://backend-server-vqch.onrender.com/`

### Endpoints

| Method | Endpoint | Description | Required Role |
|--------|----------|-------------|---------------|
| `POST` | `/auth` | Get authentication token | None |
| `GET` | `/api/employee` | Retrieve all employees | User, Editor, Admin |
| `POST` | `/api/employee` | Add new employee | Editor, Admin |
| `PUT` | `/api/employee/:id` | Update employee by ID | Editor, Admin |
| `DELETE` | `/api/employee/:id` | Delete employee by ID | Admin only |

## Data Storage

Employee data is stored in a JSON file with the following structure:

```json
[
  {
    "id": 1,
    "firstname": "John",
    "lastname": "Doe"
  },
  {
    "id": 2,
    "firstname": "Jane",
    "lastname": "Smith"
  }
]
```

## Authentication

### Getting Bearer Token
First, you need to authenticate by making a POST request to `/auth` to obtain a Bearer token:

```bash
curl -X POST \
  -H "Content-Type: application/json" \
  -d '{
    "username": "your-username",
    "password": "your-password"
  }' \
  http://localhost:3000/auth
```

### Using Bearer Token
All API requests require a valid Bearer token in the Authorization header:

```bash
Authorization: Bearer <your-token-here>
```

### Example Request
```bash
curl -H "Authorization: Bearer your-jwt-token" \
     -X GET \
     http://localhost:3000/api/employee
```

## Getting Started

### Prerequisites
- Node.js (v14 or higher)
- npm

### Installation

1. Clone the repository:
```bash
git clone https://github.com/lifelesscycle/Backend_server.git
cd Backend_Server
```

2. Install dependencies:
```bash
npm install
```

3. Start the development server:
```bash
npm run server
```

The server will start on `http://localhost:3000`

## Environment Variables

Create a `.env` file in the root directory with the following variables:

```env
REFRESH_ACCESS_TOKEN=your-refresh-token-secret
SECRET_ACCESS_TOKEN=your-secret-access-token
```

## Usage Examples

### Getting Authentication Token
```bash
curl -X POST \
  -H "Content-Type: application/json" \
  -d '{
    "username": "admin",
    "password": "password123"
  }' \
  http://localhost:3000/auth
```

### Adding an Employee (Editor/Admin)
```bash
curl -X POST \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer <editor-or-admin-token>" \
  -d '{
    "firstname": "John",
    "lastname": "Doe"
  }' \
  http://localhost:3000/api/employee
```

### Updating an Employee (Editor/Admin)
```bash
curl -X PUT \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer <editor-or-admin-token>" \
  -d '{
    "firstname": "John",
    "lastname": "Smith"
  }' \
  http://localhost:3000/api/employee/1
```

### Viewing Employees (All Roles)
```bash
curl -H "Authorization: Bearer <any-valid-token>" \
     http://localhost:3000/api/employee
```

### Deleting an Employee (Admin Only)
```bash
curl -X DELETE \
  -H "Authorization: Bearer <admin-token>" \
  http://localhost:3000/api/employee/1
```

## Deployment

The application is currently deployed on Render.com. Any changes pushed to the main branch will automatically trigger a new deployment.

**Live URL**: https://backend-server-vqch.onrender.com/

## Error Responses

The API returns appropriate HTTP status codes:

- `200` - Success
- `201` - Created
- `401` - Unauthorized (Invalid/Missing token)
- `403` - Forbidden (Insufficient permissions)
- `404` - Not Found
- `500` - Internal Server Error

## Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/new-feature`)
3. Commit your changes (`git commit -am 'Add new feature'`)
4. Push to the branch (`git push origin feature/new-feature`)
5. Create a Pull Request

## License

This project is licensed under the MIT License - see the LICENSE file for details.

## Support

For support or questions, please open an issue in the GitHub repository.
