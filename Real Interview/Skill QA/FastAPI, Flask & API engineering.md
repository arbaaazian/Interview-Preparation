# FastAPI, REST APIs & Backend Interview Questions (47 Questions and Answers)

Questions are arranged from fundamentals to advanced/scenario-based topics. Importance labels are kept in the format provided.

## 1. FastAPI and Web Framework Fundamentals

### Q1. What is the difference between Flask and FastAPI? [VERY IMPORTANT]

**Answer:**

Flask and FastAPI are Python web frameworks, but they have different design goals.

| Feature | Flask | FastAPI |
|---|---|---|
| Main focus | Lightweight web framework | Modern API development |
| Performance | Generally lower for async-heavy workloads | High performance; built on Starlette and ASGI |
| Async support | Supports async views, with limitations from its WSGI model | Native ASGI-based async support |
| Validation | Usually added with libraries such as Marshmallow | Integrated with Pydantic |
| API documentation | Usually added separately | Automatic OpenAPI, Swagger UI and ReDoc |
| Type hints | Optional; not central to the framework | Used extensively for validation and documentation |
| Best suited for | Small apps, traditional web apps, flexible projects | REST APIs, typed services and async applications |

**Example:** In FastAPI, a Pydantic model can validate an incoming request body automatically. In Flask, you typically choose and configure a validation library yourself.

**Interview summary:** Flask is minimal and flexible; FastAPI provides built-in request validation, type-hint-based development, automatic API documentation and native ASGI support.

### Q2. What is the primary purpose of implementing a nonce in a RESTful API request?

**Answer:**

A nonce means “number used once.” It is a unique, unpredictable value included in a request to help prevent replay attacks.

A server can:
1. Receive the nonce with the request.
2. Verify that it is valid and has not been used before.
3. Store or otherwise track it for the relevant validity period.
4. Reject a repeated nonce.

A nonce is often combined with a timestamp and a request signature. A nonce alone does not authenticate a user or guarantee request integrity; it is one part of a security design.

### Q3. What is the difference between defining a route with `async def` versus a standard synchronous function in FastAPI? [VERY IMPORTANT]

**Answer:**

FastAPI supports both asynchronous and synchronous path operation functions.

- `async def`: Runs as a coroutine on the event loop. Use it when the code calls asynchronous libraries and awaits I/O, such as an async HTTP client or async database driver.
- `def`: FastAPI runs the path operation function in an external thread pool, which helps prevent ordinary blocking synchronous code from blocking the event loop.

```python
from fastapi import FastAPI
import asyncio

app = FastAPI()

@app.get("/async")
async def async_route():
    await asyncio.sleep(1)  # Non-blocking wait
    return {"message": "done"}

@app.get("/sync")
def sync_route():
    import time
    time.sleep(1)  # Blocking, but FastAPI runs this sync route in a thread pool
    return {"message": "done"}
```

**Important:** Declaring a function `async` does not automatically make blocking code non-blocking. Calling a synchronous database driver or `time.sleep()` directly inside `async def` can block the event loop. Use compatible async libraries or move blocking work to a thread/process worker.

### Q4. What is Uvicorn and its role in FastAPI? [VERY IMPORTANT]

**Answer:**

Uvicorn is an ASGI server. It runs the FastAPI application, accepts incoming network connections, translates HTTP/WebSocket traffic into ASGI events, and passes those events to the application.

FastAPI is the application framework; Uvicorn is commonly the server that serves it.

```bash
uvicorn main:app --host 0.0.0.0 --port 8000 --reload
```

- `main`: Python module/file (`main.py`).
- `app`: FastAPI application object.
- `--host 0.0.0.0`: Listen on all network interfaces.
- `--port 8000`: Listen on port 8000.
- `--reload`: Restart on code changes; intended for development.

### Q5. What is connection pooling?

**Answer:**

Connection pooling means keeping a reusable pool of database connections rather than opening and closing a new connection for every request.

**Typical flow:**
1. The application requests a connection from the pool.
2. The pool returns an available connection.
3. The application runs database operations.
4. The connection is returned to the pool for reuse.

**Benefits:** Lower connection setup overhead, better throughput, controlled database connection usage and improved performance under concurrent load.

**Caution:** Pool size must be configured with the database's connection limit and the number of application workers in mind. A pool that is too large can overload the database.

### Q6. What is the difference between FastAPI and Django? [VERY IMPORTANT]

**Answer:**

| Feature | FastAPI | Django |
|---|---|---|
| Main purpose | API-first framework | Full-featured web framework |
| Architecture | ASGI-native | Supports WSGI and ASGI |
| ORM | No built-in ORM; commonly paired with SQLAlchemy or others | Built-in Django ORM |
| Admin interface | Not built in | Built-in admin site |
| Validation | Pydantic models | Django forms, serializers through Django REST Framework, and model validation |
| Documentation | Automatic OpenAPI docs | Usually added through packages |
| Best suited for | Lightweight/high-performance APIs and async services | Full web applications and systems needing integrated components |

Django can support asynchronous views and some asynchronous operations. However, whether an entire request path is truly asynchronous depends on the middleware, ORM operations and libraries being used.

### Q7. What is `SessionLocal` in SQLAlchemy? [IMPORTANT]

**Answer:**

`SessionLocal` is a commonly used variable name for a SQLAlchemy session factory. It creates a new database session when called. It is not a special SQLAlchemy keyword.

```python
from sqlalchemy import create_engine
from sqlalchemy.orm import sessionmaker

DATABASE_URL = "sqlite:///./app.db"

engine = create_engine(
    DATABASE_URL,
    connect_args={"check_same_thread": False}
)

SessionLocal = sessionmaker(
    autocommit=False,
    autoflush=False,
    bind=engine
)
```

A FastAPI dependency can create a session for a request and close it afterward:

```python
def get_db():
    db = SessionLocal()
    try:
        yield db
    finally:
        db.close()
```

The endpoint receives the session through dependency injection. For async database access, use SQLAlchemy's `AsyncSession` and an async engine instead.

### Q8. What is FastAPI middleware, and what are its common roles and use cases? [VERY IMPORTANT]

**Answer:**

Middleware is code that runs around request processing. It can inspect or modify an incoming request before it reaches the route and inspect or modify the response before it is sent to the client.

**Common use cases:**
- Request/response logging.
- Measuring request duration.
- Adding security headers.
- CORS configuration.
- Correlation/request IDs.
- Request tracing and metrics.
- General request-level processing.

Example:

```python
import time
from fastapi import FastAPI, Request

app = FastAPI()

@app.middleware("http")
async def measure_request_time(request: Request, call_next):
    start = time.perf_counter()
    response = await call_next(request)
    response.headers["X-Process-Time"] = str(
        time.perf_counter() - start
    )
    return response
```

Middleware generally applies across routes. Use dependencies when logic is specific to a route or router, such as checking the current user's permissions.

### Q9. What are `async` and `await` in FastAPI? [VERY IMPORTANT]

**Answer:**

`async` declares a coroutine function. `await` pauses that coroutine until an awaitable operation completes, allowing the event loop to run other tasks while the operation is waiting.

This is especially useful for I/O-bound operations such as network requests and async database queries.

```python
import httpx
from fastapi import FastAPI

app = FastAPI()

@app.get("/external-data")
async def get_external_data():
    async with httpx.AsyncClient() as client:
        response = await client.get("https://example.com")
    return {"status": response.status_code}
```

`await` does not make CPU-heavy work faster. Long-running CPU-bound work may need a process pool or separate worker service.

### Q10. What is dependency injection in FastAPI? [VERY IMPORTANT]

**Answer:**

Dependency injection (DI) is a pattern in which a function receives the objects or services it needs instead of constructing them itself.

FastAPI provides `Depends()` to declare dependencies.

```python
from fastapi import Depends, FastAPI

app = FastAPI()

def get_current_user():
    return {"username": "arbaaz"}

@app.get("/profile")
def profile(user: dict = Depends(get_current_user)):
    return user
```

**Common uses:**
- Database sessions.
- Authentication and authorization.
- Shared services and configuration.
- Pagination parameters.
- Reusable validation logic.

**Benefits:** Reusability, separation of concerns, easier testing and centralized management of shared logic.

### Q11. What is the N+1 query problem, and how do you prevent it?

**Answer:**

The N+1 query problem occurs when an application first runs one query to fetch a list of parent records and then runs one additional query for each parent to fetch related data.

For example, fetching 100 orders and then separately fetching each order's customer can produce 1 + 100 queries.

**Ways to prevent it:**
- Use ORM eager loading, such as SQLAlchemy `joinedload()` or `selectinload()`.
- Fetch related data with an appropriate SQL join.
- Use batch queries with `IN (...)`.
- Review generated SQL and query counts.
- Add indexes where appropriate.

The right strategy depends on relationship cardinality and the amount of data being loaded.

## 2. HTTP, REST and API Design

### Q12. What is cross-validation?

**Answer:**

Cross-validation is a model evaluation technique used in machine learning. It divides the available dataset into multiple folds. The model is trained on some folds and evaluated on the remaining fold, repeating the process so each fold is used for validation.

In k-fold cross-validation:
1. Split the data into `k` folds.
2. Train on `k-1` folds.
3. Validate on the remaining fold.
4. Repeat `k` times.
5. Average the evaluation scores.

It gives a more reliable estimate of how a model may perform on unseen data than relying on a single train/validation split. Preprocessing must be fitted inside each training fold to avoid data leakage.

### Q13. What is a FastAPI response model, and what should a POST endpoint return when data is confidential? [VERY IMPORTANT]

**Answer:**

A response model is a Pydantic model declared using `response_model`. FastAPI uses it to validate, document and filter the response.

For confidential data, return only the fields the client is allowed to see. Do not return passwords, password hashes, private tokens or internal-only fields.

```python
from pydantic import BaseModel, EmailStr
from fastapi import FastAPI

app = FastAPI()

class UserCreate(BaseModel):
    name: str
    email: EmailStr
    password: str

class UserResponse(BaseModel):
    id: int
    name: str
    email: EmailStr

@app.post("/users", response_model=UserResponse, status_code=201)
def create_user(user: UserCreate):
    # Save securely; hash the password before storing it.
    saved_user = {
        "id": 1,
        "name": user.name,
        "email": user.email,
        "password_hash": "never-return-this"
    }
    return saved_user
```

FastAPI filters the returned object to the declared response model fields. The password is not included in the response.

### Q14. What is the difference between PUT and PATCH?

**Answer:**

- `PUT` generally replaces the target resource with the representation supplied by the client. It is idempotent.
- `PATCH` applies a partial modification to a resource. It is not guaranteed to be idempotent; that depends on the patch operation.

Example:
- `PUT /users/10` may send the complete user representation.
- `PATCH /users/10` may send only `{"city": "Delhi"}`.

An API should document whether omitted fields are preserved, reset or treated as defaults.

### Q15. What are the types of dependency injection in Spring, and how is `@Autowired` used? [IMPORTANT]

**Answer:**

The common types of dependency injection in Spring are:

1. **Constructor injection:** Dependencies are provided through the class constructor. This is generally recommended for required dependencies.
2. **Setter injection:** Dependencies are provided through setter methods. Useful for optional or changeable dependencies.
3. **Field injection:** Dependencies are injected directly into fields, often using `@Autowired`. It is concise but makes testing and immutability harder.

Example of recommended constructor injection:

```java
@Service
public class UserService {
    private final UserRepository userRepository;

    public UserService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }
}
```

`@Autowired` tells Spring to resolve and inject a matching bean. With a single constructor, modern Spring can inject it without explicitly writing `@Autowired`.

### Q16. What does idempotency mean in REST APIs?

**Answer:**

An operation is idempotent if making the same request multiple times has the same intended effect on the server as making it once.

Examples:
- `GET`: Safe and idempotent.
- `PUT`: Idempotent.
- `DELETE`: Idempotent in its intended effect, even if later responses may differ.
- `POST`: Not inherently idempotent.

For payment or order creation, an API can support an idempotency key. The server stores the result associated with the key and returns the same result for a retry instead of creating a duplicate operation.

### Q17. How do you build REST APIs in FastAPI? [Scenario Based] [VERY IMPORTANT]

**Answer:**

A typical process is:

1. Create a FastAPI application.
2. Define Pydantic request and response models.
3. Create routes using HTTP methods such as GET, POST, PUT, PATCH and DELETE.
4. Add validation and dependency injection.
5. Connect a database through a repository/service layer.
6. Handle expected errors with appropriate HTTP status codes and exceptions.
7. Add authentication, authorization, logging and tests.
8. Run the application using an ASGI server such as Uvicorn.

Example:

```python
from fastapi import FastAPI, HTTPException
from pydantic import BaseModel

app = FastAPI()
items = {}

class ItemCreate(BaseModel):
    name: str
    price: float

@app.post("/items/{item_id}", status_code=201)
def create_item(item_id: int, item: ItemCreate):
    if item_id in items:
        raise HTTPException(status_code=409, detail="Item already exists")
    items[item_id] = item.model_dump()
    return {"id": item_id, **items[item_id]}

@app.get("/items/{item_id}")
def get_item(item_id: int):
    if item_id not in items:
        raise HTTPException(status_code=404, detail="Item not found")
    return items[item_id]
```

This is an in-memory demonstration. In a real service, persist data in a database and use appropriate transaction handling.

### Q18. Which status code indicates an endpoint validation error?

**Answer:**

`422 Unprocessable Entity` is FastAPI's default response status for request data that fails validation, such as a missing required field or a value with the wrong type.

Other APIs may choose `400 Bad Request` for validation errors, depending on their conventions. The important point is to use a consistent documented contract.

### Q19. Which HTTP method is idempotent but not safe in RESTful services?

**Answer:**

`PUT` and `DELETE` are idempotent but not safe.

- **Safe** means the method is intended to read data without changing server state.
- **Idempotent** means repeating the same request has the same intended effect as making it once.

`PUT` changes or replaces a resource. `DELETE` removes a resource. Both can be idempotent even though they are not safe.

### Q20. Which statement about HTTP/2 features is accurate?

**Answer:**

HTTP/2 supports:
- **Multiplexing:** Multiple request/response streams can share one TCP connection.
- **Binary framing:** HTTP messages are transmitted using a binary framing layer.
- **Header compression:** HPACK reduces repeated header overhead.
- **Stream prioritization:** Clients can express preferences for stream handling, though prioritization behavior is not uniformly implemented.
- **Server push:** The protocol includes server push, but browser support and practical use have been limited.

HTTP/2 does not require a separate TCP connection for every request. It typically uses one connection for multiple concurrent streams.

### Q21. How would you use FastAPI to build an API endpoint accepting JSON user data and saving it to a database, including validation and error handling? [Scenario Based] [VERY IMPORTANT]

**Answer:**

I would separate the request schema, database access and endpoint logic.

**Request flow:**
1. FastAPI receives JSON.
2. Pydantic validates the fields.
3. A dependency provides a database session.
4. The service checks business rules, such as duplicate email.
5. The record is saved within a transaction.
6. The endpoint returns a safe response model.

Illustrative SQLAlchemy-style example:

```python
from fastapi import FastAPI, Depends, HTTPException
from pydantic import BaseModel, EmailStr
from sqlalchemy.orm import Session

app = FastAPI()

class UserCreate(BaseModel):
    name: str
    email: EmailStr
    password: str

class UserOut(BaseModel):
    id: int
    name: str
    email: EmailStr

def get_db():
    db = SessionLocal()
    try:
        yield db
    finally:
        db.close()

@app.post("/users", response_model=UserOut, status_code=201)
def create_user(payload: UserCreate, db: Session = Depends(get_db)):
    existing = db.query(User).filter(User.email == payload.email).first()
    if existing:
        raise HTTPException(status_code=409, detail="Email already registered")

    user = User(
        name=payload.name,
        email=payload.email,
        password_hash=hash_password(payload.password)
    )
    try:
        db.add(user)
        db.commit()
        db.refresh(user)
    except Exception:
        db.rollback()
        raise HTTPException(status_code=500, detail="Unable to create user")

    return user
```

`SessionLocal`, `User`, `hash_password` and the database model are assumed to be defined elsewhere. In production, catch and translate expected database exceptions specifically, avoid exposing internal exception details, and enforce a unique constraint on email at the database level to handle concurrent requests safely.

### Q22. How would you structure a Pydantic model for JSON validation, handling optional fields and enforcing email format? [Scenario Based]

**Answer:**

Use Pydantic field types and constraints. `EmailStr` validates email format and requires the `email-validator` package.

```bash
pip install email-validator
```

```python
from pydantic import BaseModel, EmailStr, Field
from typing import Optional

class UserRequest(BaseModel):
    name: str = Field(min_length=2, max_length=80)
    email: EmailStr
    age: Optional[int] = Field(default=None, ge=18)
    phone: Optional[str] = None
```

- `name` is required and constrained to 2–80 characters.
- `email` is required and validated as an email address.
- `age` is optional; if provided, it must be at least 18.
- `phone` is optional.

In Pydantic v2, `Optional[int]` without a default is still a required field that may contain `None`; use `= None` to make it optional to omit.

### Q23. How would you create a FastAPI endpoint that accepts JSON with a "text" key and returns the reversed text, including error handling? [Scenario Based] [VERY IMPORTANT]

**Answer:**

Use a Pydantic model to validate the request body and return a JSON response.

```python
from fastapi import FastAPI
from pydantic import BaseModel, Field

app = FastAPI()

class TextRequest(BaseModel):
    text: str = Field(min_length=1)

@app.post("/reverse")
def reverse_text(payload: TextRequest):
    return {"reversed_text": payload.text[::-1]}
```

Example request:

```json
{"text": "hello"}
```

Example response:

```json
{"reversed_text": "olleh"}
```

If `text` is missing or has the wrong type, FastAPI returns a validation error (normally `422`). If an empty string should be allowed, remove `min_length=1`. Add a custom exception only if the application needs a different error contract.

## 3. FastAPI Experience and Core Behavior

### Q24. Have you worked on FastAPI with microservices? [VERY IMPORTANT]

**Answer (sample interview response — adapt to your actual work):**

Yes. I have used FastAPI to build backend APIs that can be deployed as independently managed services. In a microservices architecture, each service owns a specific responsibility and exposes APIs for other services or clients.

For example, a system may have separate user, interview, file-processing and evaluation services. Services communicate through REST APIs or messaging systems, depending on whether communication is synchronous or asynchronous.

I focus on:
- Clear service boundaries and API contracts.
- Pydantic request/response validation.
- Dependency injection for shared resources.
- Authentication and service-to-service authorization.
- Timeouts, retries and error handling.
- Centralized logs, metrics and tracing.
- Independent deployment and configuration.

Only mention services and technologies you have actually implemented.

### Q25. How do WebSockets work and how are they different from REST APIs? [IMPORTANT]

**Answer:**

REST APIs usually follow a request-response pattern: the client sends a request and the server sends a response. WebSockets provide a persistent, full-duplex connection, allowing both client and server to send messages after the connection is established.

| Feature | REST | WebSocket |
|---|---|---|
| Communication | Usually request-response | Full-duplex |
| Connection | Commonly separate HTTP requests | Persistent connection |
| Server can send unsolicited messages | Not normally over the same request | Yes |
| Typical uses | CRUD, standard APIs | Chat, live notifications, streaming updates |

WebSockets begin with an HTTP upgrade handshake and then use the WebSocket protocol. FastAPI supports WebSocket endpoints.

### Q26. Have you worked with FastAPI? What is its default port? [VERY IMPORTANT]

**Answer (sample interview response):**

Yes, I have used FastAPI to build Python backend APIs. I use Pydantic models for request validation, dependencies for reusable logic such as database sessions and authentication, and exception handlers for consistent error responses.

FastAPI itself does not impose a default network port. The commonly used Uvicorn command examples use port **8000**:

```bash
uvicorn main:app --reload
```

Uvicorn's default port is 8000 unless another port is configured.

### Q27. Does Django support asynchronous calls like FastAPI, and how do they compare? [VERY IMPORTANT]

**Answer:**

Yes. Modern Django supports asynchronous views and can run under ASGI. It also provides asynchronous variants for many operations, although some parts of the Django ecosystem and ORM workflows may still require synchronous handling or adaptation.

FastAPI is designed around ASGI and async-first API development. Django is a full-stack framework with an integrated ORM, admin, authentication and other components.

The practical difference is not that Django cannot be asynchronous. It is that FastAPI makes typed, API-focused async development central to its design, while Django offers async capabilities within a broader full-stack framework.

### Q28. What FastAPI concepts have you used in building APIs? [VERY IMPORTANT]

**Answer (sample response):**

The main FastAPI concepts I have used or would discuss are:

- `FastAPI()` to create the application.
- Path operations such as `@app.get()` and `@app.post()`.
- Pydantic models for request and response validation.
- `Depends()` for dependency injection.
- Middleware for cross-cutting request/response logic.
- `HTTPException` and custom exception handlers.
- `APIRouter` to organize endpoints into modules.
- SQLAlchemy sessions for database access.
- OAuth2/JWT-based authentication patterns.
- CORS middleware for browser-based frontend access.
- WebSocket endpoints for real-time communication.
- OpenAPI documentation through Swagger UI and ReDoc.
- Background tasks for lightweight work that can run after a response.

Be prepared to explain one real endpoint you built and how request validation, authentication, database access and error handling work together.

### Q29. How do you implement and register global exception handlers in FastAPI? [Scenario Based] [VERY IMPORTANT]

**Answer:**

Use `@app.exception_handler()` to register a handler for a specific exception type. The handler can return a consistent JSON response.

```python
from fastapi import FastAPI, Request
from fastapi.responses import JSONResponse

app = FastAPI()

class BusinessError(Exception):
    def __init__(self, message: str):
        self.message = message

@app.exception_handler(BusinessError)
async def business_error_handler(
    request: Request,
    exc: BusinessError
):
    return JSONResponse(
        status_code=400,
        content={"error": "business_error", "detail": exc.message}
    )
```

Raise it from a route or service:

```python
@app.get("/demo")
def demo():
    raise BusinessError("The requested operation is not allowed")
```

FastAPI also has handlers for `HTTPException` and request validation errors. A global handler should log internal details securely but avoid returning stack traces or secrets to clients.

### Q30. When would you use async/await versus multithreading? [Scenario Based]

**Answer:**

Choose based on the workload and the libraries involved.

- **Async/await:** Best for many concurrent I/O-bound tasks when the libraries support asynchronous operations, such as calling multiple external APIs or using an async database driver.
- **Multithreading:** Useful when integrating blocking I/O libraries or running blocking operations without blocking the event loop. Threads are not generally a way to speed up CPU-heavy Python code because of the GIL in common CPython builds.
- **Multiprocessing:** Often more appropriate for CPU-bound Python work because separate processes can use multiple CPU cores.

For example, an async FastAPI endpoint can call an async HTTP client with `await`. If it must call a blocking SDK, that call can be moved to a worker thread. Heavy model inference may need a separate process or inference service.

### Q31. How would you use dependency injection and middleware for authentication? [Scenario Based]

**Answer:**

I would use middleware for general request-level concerns and dependencies for route-level authentication and authorization.

A typical design:
1. The client sends a bearer token in the `Authorization` header.
2. A reusable dependency extracts and validates the token.
3. The dependency loads the current user or claims.
4. A second dependency checks roles or permissions.
5. Protected endpoints declare these dependencies.
6. Public endpoints do not declare the authentication dependency.

Middleware may be useful for correlation IDs, broad logging or enforcing a global policy, but it should not become a complicated replacement for route-level permission checks.

### Q32. Should authentication use middleware or dependency injection?

**Answer:**

For most FastAPI applications, use dependency injection for authentication and authorization.

**Dependency injection is preferable when:**
- Only selected routes require authentication.
- Different routes need different permissions or scopes.
- The route needs the authenticated user object.
- The logic should be easy to override in tests.

**Middleware is useful when:**
- A rule must run for nearly every request.
- You need request-wide identity context, tracing or logging.
- You want to reject certain requests before route dispatch.

A common approach is to use middleware for request-wide concerns and dependencies for authentication/authorization decisions. Public endpoints remain public by not attaching the protected dependency.

### Q33. Can you explain the FastAPI request lifecycle? [VERY IMPORTANT]

**Answer:**

A simplified FastAPI request lifecycle is:

1. **Client request:** The client sends an HTTP request to the server.
2. **ASGI server:** Uvicorn receives the network request and passes it to the FastAPI application as ASGI events.
3. **Middleware:** The request passes through the configured middleware stack.
4. **Routing:** FastAPI matches the HTTP method and path to a route.
5. **Dependency resolution:** FastAPI resolves declared dependencies, including nested dependencies.
6. **Input parsing and validation:** Path, query, header, cookie and body values are parsed and validated using type hints and Pydantic.
7. **Path operation execution:** FastAPI calls the endpoint function. Sync endpoints run in a thread pool; async endpoints run as coroutines.
8. **Response processing:** The returned value is converted to a response and validated/filtered if a response model is declared.
9. **Middleware response phase:** The response passes back through middleware.
10. **Client receives response:** The ASGI server sends the response to the client.

If an exception occurs, registered exception handlers can convert it into an HTTP response. Some background tasks run after the response has been sent.

## 4. Machine Learning, Monitoring and Advanced Scenarios

### Q34. How does cross-validation help detect overfitting and underfitting? [IMPORTANT]

**Answer:**

Cross-validation evaluates a model across multiple train/validation splits.

- **Overfitting:** The model performs very well on training data but substantially worse on validation folds. This suggests it has learned training-specific patterns.
- **Underfitting:** The model performs poorly on both training and validation data. It may be too simple, poorly configured or insufficiently trained.
- **Good generalization:** Training and validation performance are both strong and reasonably close.

Cross-validation helps estimate how stable performance is across data splits. It does not by itself prove that a model will generalize to every production distribution.

### Q35. Can you import FastAPI directly without installing it? [VERY IMPORTANT]

**Answer:**

No, not in a normal Python environment where the package is not installed. You need to install FastAPI in the environment used by your application.

```bash
pip install fastapi
```

To run the app with Uvicorn:

```bash
pip install "uvicorn[standard]"
```

Then import it:

```python
from fastapi import FastAPI

app = FastAPI()
```

If the package is already installed in the active environment, you can import it without reinstalling it. Make sure your IDE, terminal and deployment use the same Python environment.

### Q36. Can you show the response model?

**Answer:**

A response model defines the structure of the data an endpoint returns.

```python
from fastapi import FastAPI
from pydantic import BaseModel, EmailStr

app = FastAPI()

class UserResponse(BaseModel):
    id: int
    name: str
    email: EmailStr

@app.get("/users/{user_id}", response_model=UserResponse)
def get_user(user_id: int):
    return {
        "id": user_id,
        "name": "Arbaaz",
        "email": "arbaaz@example.com",
        "password_hash": "internal-only"
    }
```

The response sent to the client contains only the declared response fields. The extra `password_hash` field is filtered out.

### Q37. What HTTP status codes are commonly used?

**Answer:**

| Status | Meaning | Common use |
|---|---|---|
| 200 | OK | Successful GET or general success |
| 201 | Created | Resource successfully created |
| 202 | Accepted | Request accepted for asynchronous processing |
| 204 | No Content | Successful operation with no response body |
| 400 | Bad Request | Invalid request according to API rules |
| 401 | Unauthorized | Missing or invalid authentication |
| 403 | Forbidden | Authenticated user lacks permission |
| 404 | Not Found | Resource or route not found |
| 409 | Conflict | Duplicate or conflicting state |
| 422 | Unprocessable Content | Request validation failure, commonly used by FastAPI |
| 429 | Too Many Requests | Rate limit exceeded |
| 500 | Internal Server Error | Unexpected server error |
| 502 | Bad Gateway | Invalid response from an upstream server |
| 503 | Service Unavailable | Service temporarily unavailable |
| 504 | Gateway Timeout | Upstream service did not respond in time |

### Q38. How did you monitor the response time of the token validation process? [Scenario Based]

**Answer:**

I would measure token validation separately from the full endpoint duration so that authentication latency can be identified.

A practical approach:
1. Record a high-resolution start time before token parsing and verification.
2. Measure the elapsed time after signature, expiry and claim checks.
3. Record the duration as a metric, such as `auth_token_validation_duration_seconds`.
4. Add route, outcome and environment labels, but never log the raw token.
5. Track p50, p95 and p99 latency and error rates in a monitoring dashboard.
6. Use distributed tracing to see whether time is spent in local cryptographic verification or an external identity provider/JWKS lookup.

Example:

```python
import time
import logging

logger = logging.getLogger(__name__)

def validate_token_with_metrics(token: str):
    start = time.perf_counter()
    try:
        return validate_token(token)
    finally:
        duration = time.perf_counter() - start
        logger.info(
            "token_validation_duration_seconds=%.6f",
            duration
        )
```

For production, prefer structured metrics and tracing over relying only on log lines. Never include secrets or full bearer tokens in logs.

### Q39. How would you structure a FastAPI service to load a saved ML model for sentiment analysis, accept input, and return predictions with error handling and scalability? [Scenario Based] [VERY IMPORTANT]

**Answer:**

I would load the model once during application startup rather than loading it for every request.

**High-level design:**
1. Store the trained model and preprocessing artifacts in a versioned model registry or controlled storage.
2. Load the model during application lifespan/startup.
3. Validate incoming text using a Pydantic request model.
4. Apply the same preprocessing used during training.
5. Run inference and return a typed response.
6. Handle invalid input and inference failures safely.
7. Add metrics for latency, throughput and prediction errors.
8. Scale using multiple workers or separate inference services, depending on model size and hardware.

Illustrative structure:

```python
from contextlib import asynccontextmanager
from fastapi import FastAPI, HTTPException, Request
from pydantic import BaseModel, Field

@asynccontextmanager
async def lifespan(app: FastAPI):
    app.state.model = load_model("sentiment-model")
    yield
    app.state.model = None

app = FastAPI(lifespan=lifespan)

class SentimentRequest(BaseModel):
    text: str = Field(min_length=1, max_length=10000)

class SentimentResponse(BaseModel):
    sentiment: str
    confidence: float

@app.post("/predict", response_model=SentimentResponse)
def predict(payload: SentimentRequest, request: Request):
    try:
        model = request.app.state.model
        result = model.predict(payload.text)
        return SentimentResponse(
            sentiment=result.label,
            confidence=float(result.confidence)
        )
    except Exception:
        # Log the detailed exception internally.
        raise HTTPException(
            status_code=500,
            detail="Prediction failed"
        )
```

`load_model()` and the model's `predict()` interface are illustrative placeholders. For CPU/GPU-heavy inference, avoid blocking the event loop; use a worker process, task queue or dedicated model-serving service. Ensure each worker's model memory usage is considered before increasing worker count.

### Q40. How do you handle blocking database calls in a FastAPI microservice? [Scenario Based] [VERY IMPORTANT]

**Answer:**

First, identify whether the database driver is synchronous or asynchronous.

**Option 1: Use an async database driver**

Use an async SQLAlchemy engine and `AsyncSession`, or another async-compatible database client. Then await database operations inside `async def` routes.

**Option 2: Keep synchronous database access**

Use a normal `def` path operation so FastAPI runs it in its thread pool, or explicitly offload blocking calls from async code to a worker thread.

**Important practices:**
- Do not call blocking database operations directly inside an `async def` route.
- Use connection pooling.
- Keep transactions short.
- Set query and connection timeouts.
- Use indexes and efficient queries.
- Monitor pool saturation and query latency.
- Avoid creating a global session shared across concurrent requests; create and close sessions with request-scoped dependencies.

### Q41. How would you handle a request timeout or enforce a global execution timeout in FastAPI? [Scenario Based] [VERY IMPORTANT]

**Answer:**

Timeouts should be enforced at the correct layer.

- **Outbound HTTP calls:** Configure connect/read/write timeouts in the HTTP client.
- **Database operations:** Configure database/driver statement and connection timeouts.
- **Async application operations:** Use `asyncio.timeout()` (Python 3.11+) or `asyncio.wait_for()` for cancellable async work.
- **Server/proxy:** Configure request or upstream timeouts in the reverse proxy/load balancer and ASGI server environment as appropriate.
- **Long-running jobs:** Return `202 Accepted` with a job ID and process the work in a background worker instead of holding an HTTP request open.

Example:

```python
import asyncio
from fastapi import FastAPI, HTTPException

app = FastAPI()

@app.get("/slow")
async def slow_operation():
    try:
        async with asyncio.timeout(5):
            return await do_async_work()
    except TimeoutError:
        raise HTTPException(
            status_code=504,
            detail="Operation timed out"
        )
```

`do_async_work()` is illustrative. A timeout around an async task does not reliably stop arbitrary blocking code running in a thread or CPU-bound code. A reverse proxy timeout alone may stop waiting for a response without cancelling the underlying work. There is no single FastAPI setting that safely enforces every kind of global execution timeout.

### Q42. How do you handle CORS issues in FastAPI? [Scenario Based] [VERY IMPORTANT]

**Answer:**

CORS (Cross-Origin Resource Sharing) is a browser security mechanism that controls whether a frontend from one origin can access a backend from another origin.

FastAPI provides `CORSMiddleware`.

```python
from fastapi import FastAPI
from fastapi.middleware.cors import CORSMiddleware

app = FastAPI()

app.add_middleware(
    CORSMiddleware,
    allow_origins=["http://localhost:3000"],
    allow_credentials=True,
    allow_methods=["GET", "POST", "PUT", "PATCH", "DELETE"],
    allow_headers=["Authorization", "Content-Type"],
)
```

**Troubleshooting steps:**
1. Check the exact frontend origin, including scheme, host and port.
2. Allow the required methods and headers.
3. Handle preflight `OPTIONS` requests through the middleware.
4. If credentials are enabled, specify explicit allowed origins rather than `*`.
5. Check proxy/gateway configuration and ensure CORS headers are not being removed.

CORS is enforced by browsers; it is not a replacement for authentication or authorization.

### Q43. How would you handle authentication in FastAPI, including protected and public endpoints? [Scenario Based] [VERY IMPORTANT]

**Answer:**

I would use a standard bearer-token approach, commonly JWT, with FastAPI security utilities and dependencies.

**Flow:**
1. The user submits credentials to a login endpoint.
2. The server verifies the credentials.
3. The server issues a signed access token with appropriate claims and expiry.
4. The client sends it as `Authorization: Bearer <token>`.
5. A dependency validates the token and retrieves the current user.
6. Protected endpoints require that dependency; public endpoints do not.

Illustrative pattern:

```python
from fastapi import Depends, FastAPI
from fastapi.security import OAuth2PasswordBearer

app = FastAPI()
oauth2_scheme = OAuth2PasswordBearer(tokenUrl="/login")

def get_current_user(token: str = Depends(oauth2_scheme)):
    claims = verify_and_decode_token(token)
    return load_user(claims["sub"])

@app.get("/public")
def public_endpoint():
    return {"message": "Anyone can access this"}

@app.get("/private")
def private_endpoint(user=Depends(get_current_user)):
    return {"username": user.username}
```

`verify_and_decode_token()` and `load_user()` are application-specific placeholders. In production, use secure password hashing, signing-key management, token expiry and rotation/revocation strategy as appropriate, HTTPS, and role/permission checks. Do not store raw passwords or log tokens.

### Q44. How would you optimize a FastAPI application serving thousands of concurrent users? [Scenario Based] [VERY IMPORTANT]

**Answer:**

I would optimize based on measurements rather than adding workers blindly.

1. **Profile first:** Measure p50/p95/p99 latency, throughput, CPU, memory, error rate and database wait time.
2. **Use async I/O correctly:** Use async HTTP/database clients for I/O-bound workloads; avoid blocking the event loop.
3. **Database optimization:** Use connection pooling, indexes, efficient SQL, pagination and eager loading to avoid N+1 queries.
4. **Caching:** Cache frequently requested data using Redis or an appropriate cache, with clear expiry/invalidation rules.
5. **Scale horizontally:** Run multiple application instances behind a load balancer. Choose worker counts based on CPU, memory and workload.
6. **Move heavy work out of requests:** Use a queue and background workers for long-running jobs.
7. **Protect the service:** Add rate limiting, request-size limits, timeouts and backpressure.
8. **Reduce response overhead:** Avoid unnecessary serialization and return only required fields.
9. **Observe continuously:** Use structured logs, metrics and distributed tracing.
10. **Load test:** Test realistic concurrency and traffic patterns before deployment.

Thousands of concurrent users do not necessarily mean thousands of simultaneous database connections. Bound resource usage and design for graceful overload.

### Q45. How would you design a REST API to efficiently read large datasets? [Scenario Based]

**Answer:**

I would avoid returning the entire dataset in one response.

**Design choices:**
- Use pagination. Offset/limit is simple for smaller datasets; cursor/keyset pagination is often more efficient and stable for large datasets.
- Support filters and sorting using indexed columns.
- Return only required fields.
- Set a maximum page size.
- Add indexes based on common filter and sort patterns.
- Use compression where beneficial.
- Consider streaming or asynchronous export jobs for very large downloads.
- Include pagination metadata or a next-page cursor.

Example:

```http
GET /orders?limit=100&after=eyJpZCI6MTAwMH0=
```

A cursor should encode a stable ordering key, such as `(created_at, id)`, so records are not skipped or duplicated as easily when new rows are inserted. Validate filter and sort parameters to avoid unsafe dynamic SQL.

### Q46. How would you design a weather API for one city and for an array of 30 cities? [Scenario Based]

**Answer:**

I would provide both single-city and batch endpoints while reusing the same service layer.

**Single city:**

```http
GET /weather?city=Delhi
```

**Multiple cities:**

```http
POST /weather/batch
Content-Type: application/json

{
  "cities": ["Delhi", "Mumbai", "Bengaluru"]
}
```

For a batch of up to 30 cities:
1. Validate the list and enforce a maximum of 30 entries.
2. Normalize city identifiers and handle duplicates.
3. Fetch weather data concurrently using an async HTTP client, subject to a concurrency limit.
4. Cache results based on the provider's update frequency.
5. Return one result per city, including per-city errors if partial success is allowed.
6. Handle provider rate limits, timeouts and retries with backoff.
7. Avoid making 30 sequential calls if the provider offers a batch API.

Example response shape:

```json
{
  "results": [
    {"city": "Delhi", "temperature": 24, "status": "success"},
    {"city": "Mumbai", "temperature": 29, "status": "success"},
    {"city": "Bengaluru", "status": "error", "message": "Provider timeout"}
  ]
}
```

The temperature values are illustrative. The actual schema and units should be documented. If the upstream provider supports a true multi-city request, prefer it over 30 independent calls.

### Q47. What strategies do you use to optimize endpoint performance? [Scenario Based]

**Answer:**

I use a measurement-driven approach:

- Measure endpoint latency and identify slow sections.
- Optimize database queries, indexes and N+1 access patterns.
- Use connection pooling and appropriate transaction boundaries.
- Use async I/O for supported I/O-bound operations.
- Cache expensive, frequently reused results.
- Paginate large responses and select only needed columns.
- Avoid repeated computation and unnecessary serialization.
- Move long-running or CPU-intensive work to background workers or dedicated services.
- Set sensible timeouts and concurrency limits.
- Use load testing to validate improvements.
- Monitor p50, p95 and p99 latency after deployment.

**Interview approach:** Explain the bottleneck you measured, the change you made, and the before/after result. Do not claim a performance improvement without measurements.
