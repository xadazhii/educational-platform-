# EduFlex: Interactive Learning Platform

**EduFlex** is a modern, full-stack web application designed to create a seamless and secure educational environment for students and instructors. It provides a comprehensive suite of tools for course management, interactive testing, progress tracking, and secure material sharing.

The application is built on a robust client-server architecture, featuring a stateless **Spring Boot** backend with **JWT-based security** and a dynamic **React.js** frontend. This project showcases the integration of modern backend security practices with a responsive user interface to deliver a powerful and user-friendly learning tool.

---

### ⛓️ Table of Contents

1.  [Visual Overview](#visual-overview)
2.  [Features](#features)
3.  [How It Works (Technical Deep-Dive)](#how-it-works-technical-deep-dive)
    * [Architecture Overview](#architecture-overview)
    * [JWT Authentication Flow](#jwt-authentication-flow)
    * [Role-Based Access Control (RBAC)](#role-based-access-control-rbac)
    * [Secure File Handling](#secure-file-handling)
4.  [Technologies & Stack](#technologies--stack)
5.  [Building and Running](#building-and-running)
6.  [License](#license)

---

### Visual Overview

<img width="1670" height="952" alt="Screenshot 2025-09-10 at 22 54 34" src="https://github.com/user-attachments/assets/7ee010e4-fb5a-4ca4-8a01-eb1a041e5917" />
<img width="1674" height="950" alt="Screenshot 2025-09-10 at 23 03 33" src="https://github.com/user-attachments/assets/5e8989fa-a139-43d0-9cd4-935f2a3860d9" />
<img width="1676" height="955" alt="Screenshot 2025-09-10 at 23 03 51" src="https://github.com/user-attachments/assets/b1036319-56b8-4ac8-9735-562daa1e7c2a" />
<img width="1677" height="953" alt="Screenshot 2025-09-10 at 23 03 59" src="https://github.com/user-attachments/assets/a5a99bf8-9e89-40c6-af6a-bd2092d8b463" />
<img width="1680" height="962" alt="Screenshot 2025-09-10 at 23 04 16" src="https://github.com/user-attachments/assets/ca581c77-0e70-47a7-8896-a7611aee9671" />
<img width="1678" height="951" alt="Screenshot 2025-09-10 at 23 04 25" src="https://github.com/user-attachments/assets/95775547-42dd-4bbe-8511-f97d7bfb876d" />

---

### Features

#### 👥 For Students (USER Role)

* **Personal Dashboard**: View upcoming events, tests, and personal statistics.
* **Interactive Tests**: Take tests with various question types and receive instant results.
* **Progress Tracking**: Monitor scores and performance across all completed tests.
* **Secure Material Access**: Download course materials via secure, temporary links.
* **Event Calendar**: Keep track of important deadlines and events shared by administrators.

#### 🔑 For Administrators (ADMIN Role)

* **Full User Management**: Create, delete, and manage user accounts, including role assignments.
* **Test Management (CRUD)**: Create, update, and delete tests with questions and answers. A key feature prevents editing a test once students have submitted results, ensuring data integrity.
* **File Management**: Securely upload and manage course materials.
* **Calendar Management**: Add or remove events in the shared academic calendar.
* **Student Whitelisting**: A specific business logic feature ensures that only students from a pre-approved list can register.

---

### How It Works (Technical Deep-Dive)

#### Architecture Overview

EduFlex uses a decoupled architecture where the **React Single-Page Application (SPA)** acts as the client, communicating with the **Spring Boot RESTful API** backend. All communication is stateless, relying on JSON Web Tokens (JWT) for authentication.

#### JWT Authentication Flow

The security is built around stateless JWT authentication, which is ideal for modern web applications.
_\_

1.  **Login**: The user submits their username and password to the `/api/auth/signin` endpoint.
2.  **Authentication**: `Spring Security`'s `AuthenticationManager` validates the credentials.
3.  **Token Generation**: If successful, `JwtUtils` generates a signed JWT containing the user's identity and roles.
4.  **Token Response**: The server sends the JWT back to the React client.
5.  **Token Storage**: The client stores the JWT (e.g., in `localStorage`).
6.  **Authenticated Requests**: For every subsequent request to a protected endpoint, the client sends the JWT in the `Authorization: Bearer <token>` header.
7.  **Token Validation**: The `AuthTokenFilter` on the server intercepts each request, validates the JWT, and sets the user's `Authentication` in the `SecurityContextHolder`. This makes the user's identity available throughout the request lifecycle.

#### Role-Based Access Control (RBAC)

Method-level security is enforced using the `@PreAuthorize` annotation. This allows for granular control over which roles can access specific API endpoints. For example, creating a test is restricted to administrators:

```java
@PostMapping
@PreAuthorize("hasRole('ADMIN')")
public ResponseEntity<?> createTest(@RequestBody TestRequest testRequest) {
    // ... logic to create a test
}
```

#### Secure File Handling

To manage educational materials securely, the application integrates with **Cloudinary**.

* **Private Storage**: Files are uploaded with the `type: "private"` flag, ensuring they are not publicly accessible via a direct URL.
* **Signed URLs**: When a student needs to download a file, the backend generates a **temporary, signed URL** via the `getSignedUrl` method. This URL provides short-lived, secure access to the private file, preventing unauthorized sharing.

---

### Technologies & Stack

* **Backend**: **Java 17**, **Spring Boot**, **Spring Security**, **Spring Data JPA**, **JWT**
* **Frontend**: **React.js**, **Axios**
* **Database**: **PostgreSQL** (or any other JPA-compatible SQL database)
* **DevOps & Cloud**: **Docker**, **Render** (for backend hosting), **Netlify** (for frontend hosting), **Cloudinary** (for file storage)
* **Build Tools**: **Maven** (for backend), **NPM** (for frontend)

---

### Building and Running

#### Prerequisites

* Java 17+
* Maven 3.8+
* Node.js 16+ & NPM
* Docker (optional, for containerized deployment)
* A running PostgreSQL database instance

#### Backend (Server)

1.  **Navigate to the `server` directory.**
2.  **Configure `application.properties`**: Set your database connection details, JWT secret, and Cloudinary API credentials.
3.  **Run the application**:
    ```bash
    mvn spring-boot:run
    ```
    The server will start on port `8080`.

#### Frontend (Client)

1.  **Navigate to the `client` directory.**
2.  **Install dependencies**:
    ```bash
    npm install
    ```
3.  **Configure Environment**: Create a `.env` file in the `client` directory and set the backend API URL:
    ```
    REACT_APP_API_URL=http://localhost:8080
    ```
4.  **Run the application**:
    ```bash
    npm start
    ```
    The React development server will open the application in your browser.

---

### License

This project is owned by **Adazhii Kristina**. 
