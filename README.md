# Fastapi-RBAC
Role-Based Access Control (RBAC) is a common authorization mechanism used to control access to resources in web applications, including FastAPI applications. FastAPI itself doesn't provide a built-in RBAC system, but you can implement RBAC in your FastAPI application using Python libraries and best practices.

Here's a guide on how to handle RBAC in a FastAPI application:

** Define User Roles and Permissions:
  Identify the different roles and permissions your application needs. Common roles might include "admin," "user," and "guest." Permissions could be things like "read," "write," "delete," etc.

** Database Schema:
  Create a database schema that includes tables for users, roles, and permissions.
  Create tables that associate users with roles and roles with permissions. For example, a many-to-many relationship table can map users to roles.

** User Authentication:
  Implement user authentication using tools like OAuth, JWT (JSON Web Tokens), or session-based authentication.

** Authorization Middleware:
  Create a middleware function or decorator to perform RBAC checks for each route. This middleware should check the user's role and permissions against the required ones for the route being accessed.


** Route Authorization:
  Use the middleware decorator created in step 4 to protect specific routes:
  @app.get("/admin", response_model=SomeModel)
async def admin_route(user: User = Depends(check_permissions(["admin"]))):
    # This route is only accessible to users with the "admin" role
    return {"message": "Admin route"}
```
from fastapi import Depends, HTTPException, status
from .models import User, Role, Permission

def check_permissions(required_permissions):
    def middleware(user: User = Depends(get_current_user)):
        # Check user's permissions against required_permissions
        if not user_has_permissions(user, required_permissions):
            raise HTTPException(
                status_code=status.HTTP_403_FORBIDDEN,
                detail="Insufficient permissions",
            )
        return user
    return middleware
´´´
@app.get("/user", response_model=SomeModel)
``` async def user_route(user: User = Depends(check_permissions(["user"]))):
    # This route is only accessible to users with the "user" role
    return {"message": "User route"}
    ```
