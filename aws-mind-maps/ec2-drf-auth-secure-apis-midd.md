To secure a **PROD-level Django codebase** with over **100+ API endpoints** and **25+ Django apps** using **Azure Active Directory (Azure AD)** and **AWS Cognito** (with SSO and MFA), follow these detailed steps. This approach leverages both Azure AD for authentication and AWS Cognito for managing user pools, SSO, and enforcing MFA. 

---

## High-Level Strategy

1. **Azure Active Directory**: Use Azure AD for **authentication** (sign-in).
2. **AWS Cognito**: Integrate AWS Cognito for **SSO** and **MFA enforcement**.
3. **Nginx**: Secure API traffic and act as a reverse proxy.
4. **Django Middleware**: Enforce authentication for all API endpoints.
5. **Token Validation**: Validate JWT tokens issued by Azure AD and AWS Cognito.
6. **Deployment on EC2**: Ensure secure deployment configurations.

---

## Step-by-Step Implementation

### 1. **Azure Active Directory Setup**

1. **Register the Application** in Azure AD:
   - Go to **Azure Portal** > **Azure Active Directory** > **App Registrations** > **New Registration**.
   - Set the **Redirect URI**: `https://your-domain.com/auth/callback/`.
   - **Note** the **Application (Client) ID** and **Directory (Tenant) ID**.

2. **Create Client Secret**:
   - Go to **Certificates & Secrets** > **New Client Secret**.
   - **Save** the generated secret.

---

### 2. **AWS Cognito Setup**

1. **Create a User Pool**:
   - Go to **AWS Cognito** > **Manage User Pools** > **Create a User Pool**.
   - Enable **MFA** under **Policies**:
     - Select **SMS-based MFA** or **TOTP-based MFA**.

2. **Configure SSO**:
   - In **App Client Settings**, enable **SSO** with **Azure AD** as the identity provider.

3. **Add Azure AD as an Identity Provider**:
   - In **Federation** > **Identity Providers**, choose **SAML** or **OIDC** and configure Azure AD details.

4. **Create an App Client** and **Note** the following:
   - **Client ID**.
   - **Client Secret**.
   - **Cognito Domain**.

---

### 3. **Install Required Packages**

Install the following packages in your Django project:

```bash
pip install msal boto3 django-cors-headers djangorestframework-simplejwt
```

---

### 4. **Update Django Settings**

Add configurations for **Azure AD** and **AWS Cognito** in `settings.py`:

```python
# settings.py

import os

# Azure AD Config
AZURE_AD = {
    "CLIENT_ID": os.getenv("AZURE_AD_CLIENT_ID"),
    "CLIENT_SECRET": os.getenv("AZURE_AD_CLIENT_SECRET"),
    "TENANT_ID": os.getenv("AZURE_AD_TENANT_ID"),
    "AUTHORITY": f"https://login.microsoftonline.com/{os.getenv('AZURE_AD_TENANT_ID')}",
    "REDIRECT_URI": "https://your-domain.com/auth/callback/",
    "SCOPE": ["openid", "profile", "email"],
}

# AWS Cognito Config
AWS_COGNITO = {
    "USER_POOL_ID": os.getenv("AWS_COGNITO_USER_POOL_ID"),
    "CLIENT_ID": os.getenv("AWS_COGNITO_CLIENT_ID"),
    "CLIENT_SECRET": os.getenv("AWS_COGNITO_CLIENT_SECRET"),
    "REGION": "your-region",
    "DOMAIN": "your-cognito-domain",
}

# CORS Config
CORS_ALLOWED_ORIGINS = [
    "https://your-domain.com",
]

# Middleware
MIDDLEWARE += [
    "django_cors_headers.middleware.CorsMiddleware",
    "your_app.middleware.AuthenticationMiddleware",  # Custom middleware for authentication
]

# JWT Token Settings
SIMPLE_JWT = {
    "AUTH_HEADER_TYPES": ("Bearer",),
}
```

---

### 5. **Custom Middleware for Authentication**

Create a custom middleware to enforce authentication for all API endpoints:

```python
# middleware.py

import jwt
import requests
from django.http import JsonResponse
from django.conf import settings

class AuthenticationMiddleware:
    """
    Middleware to enforce authentication using Azure AD and AWS Cognito.
    """

    def __init__(self, get_response):
        self.get_response = get_response

    def __call__(self, request):
        auth_header = request.headers.get("Authorization")

        if not auth_header:
            return JsonResponse({"error": "Unauthorized"}, status=401)

        try:
            token = auth_header.split(" ")[1]
            self.validate_token(token)
        except Exception as e:
            return JsonResponse({"error": str(e)}, status=401)

        return self.get_response(request)

    def validate_token(self, token):
        try:
            # Decode token without verification to get the issuer
            unverified_header = jwt.get_unverified_header(token)
            unverified_claims = jwt.decode(token, options={"verify_signature": False})

            issuer = unverified_claims.get("iss")

            if "microsoftonline.com" in issuer:
                jwks_url = f"{issuer}/discovery/v2.0/keys"
            elif "cognito" in issuer:
                jwks_url = f"https://cognito-idp.{settings.AWS_COGNITO['REGION']}.amazonaws.com/{settings.AWS_COGNITO['USER_POOL_ID']}/.well-known/jwks.json"
            else:
                raise Exception("Invalid token issuer")

            # Fetch public keys and decode the token
            jwks = requests.get(jwks_url).json()
            jwt.decode(token, jwks, algorithms=["RS256"], audience=settings.AZURE_AD["CLIENT_ID"])
        except Exception as e:
            raise Exception(f"Token validation failed: {str(e)}")
```

---

### 6. **Secure Your Views**

All API views will now be protected by the middleware. For additional control, you can use Django decorators:

```python
# views.py

from rest_framework.decorators import api_view, permission_classes
from rest_framework.permissions import IsAuthenticated
from rest_framework.response import Response

@api_view(["GET"])
@permission_classes([IsAuthenticated])
def secure_api(request):
    return Response({"message": "This is a secure API"})
```

---

### 7. **Update Nginx Configuration**

Ensure **Nginx** forwards requests to Django securely:

```nginx
# nginx.conf

server {
    listen 80;
    server_name your-domain.com;

    location / {
        proxy_pass http://127.0.0.1:8000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

---

### 8. **Deploy and Test**

1. **Set Environment Variables** on your EC2 instance:

   ```bash
   export AZURE_AD_CLIENT_ID="your-azure-client-id"
   export AZURE_AD_CLIENT_SECRET="your-azure-client-secret"
   export AZURE_AD_TENANT_ID="your-tenant-id"
   export AWS_COGNITO_USER_POOL_ID="your-user-pool-id"
   export AWS_COGNITO_CLIENT_ID="your-cognito-client-id"
   export AWS_COGNITO_CLIENT_SECRET="your-cognito-client-secret"
   ```

2. **Restart Services**:

   ```bash
   sudo systemctl restart nginx
   sudo systemctl restart gunicorn
   ```

3. **Test Secure API Endpoints**:
   - Visit a secure endpoint with a valid Azure AD or Cognito token in the `Authorization` header.
   - If unauthorized, you should receive a **401 Unauthorized** error.

---

## Summary of Security Enhancements

1. **Azure AD** for user authentication.
2. **AWS Cognito** for managing users, enforcing MFA, and enabling SSO.
3. **Middleware** to enforce authentication across all API endpoints.
4. **Nginx** acts as a reverse proxy to handle traffic securely.
5. **JWT Token Validation** to ensure only authenticated users can access the APIs.

This approach ensures that your Django application is secure and robust for a production environment.



To **secure 100+ Class-Based Views (CBVs)** with authentication in a Django project while minimizing code changes, using **middleware** is the most efficient approach. This allows us to enforce authentication globally without modifying each individual view.

Here's a detailed explanation and steps to achieve this.

---

## 1. **Middleware for Authentication**

Middleware runs on each request and can validate authentication tokens globally before the request reaches any view. Here's the middleware code that handles authentication using **Azure Active Directory** and **AWS Cognito** tokens.

### **`middleware.py`**

```python
# middleware.py

import jwt
import requests
from django.http import JsonResponse
from django.conf import settings

class AuthenticationMiddleware:
    """
    Middleware to enforce authentication using Azure AD and AWS Cognito.
    """

    def __init__(self, get_response):
        self.get_response = get_response

    def __call__(self, request):
        auth_header = request.headers.get("Authorization")

        # Check if Authorization header exists
        if not auth_header:
            return JsonResponse({"error": "Unauthorized: No Authorization header provided"}, status=401)

        try:
            token = auth_header.split(" ")[1]
            self.validate_token(token)
        except Exception as e:
            return JsonResponse({"error": f"Unauthorized: {str(e)}"}, status=401)

        # Proceed to the view if authentication is successful
        return self.get_response(request)

    def validate_token(self, token):
        try:
            # Decode token without verification to get the issuer
            unverified_header = jwt.get_unverified_header(token)
            unverified_claims = jwt.decode(token, options={"verify_signature": False})

            issuer = unverified_claims.get("iss")

            # Determine the appropriate JWKS URL based on the issuer
            if "microsoftonline.com" in issuer:
                jwks_url = f"{issuer}/discovery/v2.0/keys"
                audience = settings.AZURE_AD["CLIENT_ID"]
            elif "cognito" in issuer:
                jwks_url = f"https://cognito-idp.{settings.AWS_COGNITO['REGION']}.amazonaws.com/{settings.AWS_COGNITO['USER_POOL_ID']}/.well-known/jwks.json"
                audience = settings.AWS_COGNITO["CLIENT_ID"]
            else:
                raise Exception("Invalid token issuer")

            # Fetch public keys and decode the token
            jwks = requests.get(jwks_url).json()
            jwt.decode(token, jwks, algorithms=["RS256"], audience=audience)
        except Exception as e:
            raise Exception(f"Token validation failed: {str(e)}")
```

### **Explanation of `AuthenticationMiddleware`**

1. **Initialization**:
   - The middleware is initialized with `get_response`, which is the callable that handles the request.

2. **`__call__` Method**:
   - This method is called for each incoming request.
   - It checks for the `Authorization` header.
   - If the header is missing, it returns a **401 Unauthorized** response.
   - If present, it extracts the JWT token and calls `validate_token`.

3. **`validate_token` Method**:
   - This method validates the JWT token using public keys from Azure AD or AWS Cognito.
   - Determines the appropriate **JWKS URL** based on the token's issuer.
   - Fetches public keys and decodes the token.
   - If validation fails, it raises an exception, returning a **401 Unauthorized** response.

---

## 2. **Add Middleware to Django Settings**

Add the `AuthenticationMiddleware` to the `MIDDLEWARE` list in `settings.py`:

```python
# settings.py

MIDDLEWARE = [
    ...
    'your_app.middleware.AuthenticationMiddleware',  # Add this line
]
```

---

## 3. **Why Middleware Is Efficient**

1. **Global Enforcement**:
   - Authentication is enforced for every request automatically.
   - No need to modify individual views.

2. **DRY Principle**:
   - Centralizes authentication logic in one place, reducing code duplication.

3. **Flexibility**:
   - Easily extendable to add more authentication providers or custom logic.

---

## 4. **Handling Public Endpoints**

If you have endpoints that should be publicly accessible (without authentication), you can exclude them in the middleware:

### **Modify Middleware to Exclude Public Endpoints**

```python
class AuthenticationMiddleware:
    PUBLIC_PATHS = ["/public/", "/health/", "/docs/"]

    def __call__(self, request):
        if any(request.path.startswith(path) for path in self.PUBLIC_PATHS):
            return self.get_response(request)

        auth_header = request.headers.get("Authorization")
        if not auth_header:
            return JsonResponse({"error": "Unauthorized: No Authorization header provided"}, status=401)

        try:
            token = auth_header.split(" ")[1]
            self.validate_token(token)
        except Exception as e:
            return JsonResponse({"error": f"Unauthorized: {str(e)}"}, status=401)

        return self.get_response(request)
```

---

## 5. **Secure APIs with Minimal Code Changes**

Since the middleware handles authentication, **no changes are needed in your 100+ Class-Based Views**. Your views can remain as they are.

### Example View (No Modification Needed)

```python
# views.py

from rest_framework.views import APIView
from rest_framework.response import Response

class SecureDataView(APIView):
    def get(self, request):
        data = {"message": "This is a secure endpoint"}
        return Response(data)
```

---

## 6. **Testing the Setup**

### **1. Obtain a JWT Token**

- Authenticate via **Azure AD** or **AWS Cognito** to get a valid JWT token.

### **2. Call the API with the Token**

```bash
curl -X GET https://your-domain.com/api/secure-data/ \
     -H "Authorization: Bearer <YOUR_JWT_TOKEN>"
```

### **3. Expected Responses**

- **200 OK**: If the token is valid.
- **401 Unauthorized**: If the token is missing or invalid.

---

## Summary

1. **Middleware** enforces authentication globally for all requests.
2. Minimal code changes: no need to modify existing views.
3. Exclude public endpoints by customizing the middleware.
4. Easily integrate Azure AD and AWS Cognito with token validation.

This approach ensures your **100+ API endpoints** are secured efficiently and consistently.