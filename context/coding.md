# Coding Guidelines

This document defines the technology stack, coding standards, and development patterns for all applications. Claude must follow these guidelines when generating code.

---

## Technology Stack

| Layer | Technology | Version |
|-------|------------|---------|
| Backend | Python | 3.11+ |
| Frontend | JavaScript (ES6+) | - |
| UI Framework | shadcn/ui | Latest |
| CSS | Tailwind CSS | 3.x |
| AI/Agents | AWS Bedrock | - |
| Infrastructure | AWS | - |

---

## Backend: Python

### Project Structure

```
/backend
  /src
    /api
      __init__.py
      routes/
      middleware/
    /core
      __init__.py
      config.py
      exceptions.py
    /services
      __init__.py
    /models
      __init__.py
    /repositories
      __init__.py
    /agents
      __init__.py
  /tests
    /unit
    /integration
  requirements.txt
  pyproject.toml
  main.py
```

### Framework and Libraries

| Purpose | Library |
|---------|---------|
| Web Framework | FastAPI |
| Validation | Pydantic |
| Database ORM | SQLAlchemy |
| Async Support | asyncio, httpx |
| Testing | pytest, pytest-asyncio |
| Linting | ruff |
| Type Checking | mypy |

### Coding Standards

**General Rules:**
- Use type hints for all function signatures
- Follow PEP 8 style guidelines
- Maximum line length: 100 characters
- Use async/await for I/O operations
- Document all public functions with docstrings

**Naming Conventions:**
```python
# Classes: PascalCase
class UserService:
    pass

# Functions and variables: snake_case
def get_user_by_id(user_id: str) -> User:
    pass

# Constants: UPPER_SNAKE_CASE
MAX_RETRY_ATTEMPTS = 3

# Private methods: leading underscore
def _validate_input(data: dict) -> bool:
    pass
```

**Function Structure:**
```python
from typing import Optional
from pydantic import BaseModel

class UserResponse(BaseModel):
    id: str
    name: str
    email: str

async def get_user(user_id: str) -> Optional[UserResponse]:
    """
    Retrieve a user by their unique identifier.
    
    Args:
        user_id: The unique identifier of the user.
        
    Returns:
        UserResponse if found, None otherwise.
        
    Raises:
        ValidationError: If user_id format is invalid.
    """
    # Implementation
    pass
```

**Error Handling:**
```python
from fastapi import HTTPException, status
from core.exceptions import ApplicationError

# Define custom exceptions
class UserNotFoundError(ApplicationError):
    def __init__(self, user_id: str):
        super().__init__(f"User not found: {user_id}")
        self.user_id = user_id

# Use structured error responses
async def get_user(user_id: str) -> UserResponse:
    user = await repository.find_by_id(user_id)
    if not user:
        raise HTTPException(
            status_code=status.HTTP_404_NOT_FOUND,
            detail={"error": "USER_NOT_FOUND", "user_id": user_id}
        )
    return user
```

### API Design

- Use RESTful conventions
- Version APIs: `/api/v1/...`
- Return consistent response structures
- Use appropriate HTTP status codes

```python
from fastapi import APIRouter, status
from pydantic import BaseModel

router = APIRouter(prefix="/api/v1/users", tags=["users"])

class ApiResponse(BaseModel):
    success: bool
    data: Optional[dict] = None
    error: Optional[str] = None

@router.get("/{user_id}", response_model=ApiResponse)
async def get_user(user_id: str):
    user = await user_service.get_user(user_id)
    return ApiResponse(success=True, data=user.model_dump())

@router.post("/", status_code=status.HTTP_201_CREATED)
async def create_user(request: CreateUserRequest):
    user = await user_service.create_user(request)
    return ApiResponse(success=True, data=user.model_dump())
```

---

## Frontend: JavaScript with shadcn/ui

### Project Structure

```
/frontend
  /src
    /components
      /ui          # shadcn components
      /features    # feature-specific components
      /layouts     # layout components
    /pages
    /hooks
    /services
    /utils
    /styles
    /context
  /public
  package.json
  tailwind.config.js
  jsconfig.json
```

### Framework and Libraries

| Purpose | Library |
|---------|---------|
| Framework | React 18+ |
| Build Tool | Vite |
| UI Components | shadcn/ui |
| Styling | Tailwind CSS |
| State Management | React Context / Zustand |
| HTTP Client | fetch / axios |
| Forms | React Hook Form |
| Validation | Zod |
| Routing | React Router |

### Coding Standards

**General Rules:**
- Use functional components with hooks
- Prefer named exports over default exports
- Use JSDoc comments for complex functions
- Keep components focused and single-purpose
- Extract reusable logic into custom hooks

**Naming Conventions:**
```javascript
// Components: PascalCase
function UserProfile({ user }) { }

// Hooks: camelCase with 'use' prefix
function useUserData(userId) { }

// Utilities: camelCase
function formatCurrency(amount) { }

// Constants: UPPER_SNAKE_CASE
const API_BASE_URL = '/api/v1';

// Event handlers: handle prefix
function handleSubmit(event) { }
```

**Component Structure:**
```javascript
import { useState, useEffect } from 'react';
import { Button } from '@/components/ui/button';
import { Card, CardHeader, CardContent } from '@/components/ui/card';

/**
 * UserCard displays user information in a card format.
 * @param {Object} props
 * @param {Object} props.user - The user object to display
 * @param {Function} props.onEdit - Callback when edit is clicked
 */
export function UserCard({ user, onEdit }) {
  const [isLoading, setIsLoading] = useState(false);

  const handleEditClick = () => {
    onEdit(user.id);
  };

  return (
    <Card>
      <CardHeader>
        <h3 className="text-lg font-semibold">{user.name}</h3>
      </CardHeader>
      <CardContent>
        <p className="text-muted-foreground">{user.email}</p>
        <Button onClick={handleEditClick} variant="outline">
          Edit
        </Button>
      </CardContent>
    </Card>
  );
}
```

### shadcn/ui Usage

**Component Installation:**
```bash
npx shadcn-ui@latest add button
npx shadcn-ui@latest add card
npx shadcn-ui@latest add input
npx shadcn-ui@latest add dialog
```

**Preferred Components:**

| Use Case | Component |
|----------|-----------|
| Actions | Button |
| Forms | Input, Select, Checkbox, Form |
| Layout | Card, Separator |
| Feedback | Alert, Toast |
| Overlays | Dialog, Sheet, Popover |
| Data Display | Table, Badge |
| Navigation | Tabs, Navigation Menu |

**Styling Guidelines:**
```javascript
// Use Tailwind classes for custom styling
<Button className="w-full mt-4">Submit</Button>

// Use shadcn variants
<Button variant="destructive">Delete</Button>
<Button variant="outline">Cancel</Button>
<Button variant="ghost">More</Button>

// Combine with Tailwind for responsive design
<Card className="w-full md:w-1/2 lg:w-1/3">
  {/* content */}
</Card>
```

### State Management

```javascript
// Use React Context for global state
import { createContext, useContext, useState } from 'react';

const AuthContext = createContext(null);

export function AuthProvider({ children }) {
  const [user, setUser] = useState(null);

  const login = async (credentials) => {
    const response = await authService.login(credentials);
    setUser(response.user);
  };

  const logout = () => {
    setUser(null);
  };

  return (
    <AuthContext.Provider value={{ user, login, logout }}>
      {children}
    </AuthContext.Provider>
  );
}

export function useAuth() {
  const context = useContext(AuthContext);
  if (!context) {
    throw new Error('useAuth must be used within AuthProvider');
  }
  return context;
}
```

---

## AI/Agents: AWS Bedrock

### Project Structure

```
/backend
  /src
    /agents
      __init__.py
      base_agent.py
      /tools
        __init__.py
      /prompts
        __init__.py
      /workflows
        __init__.py
```

### Libraries

| Purpose | Library |
|---------|---------|
| AWS SDK | boto3 |
| Bedrock Runtime | boto3 bedrock-runtime |
| Bedrock Agents | boto3 bedrock-agent-runtime |

### Agent Implementation

**Base Agent Configuration:**
```python
import boto3
from typing import Any, Optional
from abc import ABC, abstractmethod

class BedrockAgent(ABC):
    """Base class for AWS Bedrock agents."""
    
    def __init__(
        self,
        model_id: str = "anthropic.claude-3-sonnet-20240229-v1:0",
        region: str = "us-east-1"
    ):
        self.client = boto3.client(
            "bedrock-runtime",
            region_name=region
        )
        self.model_id = model_id
    
    @abstractmethod
    def get_system_prompt(self) -> str:
        """Return the system prompt for this agent."""
        pass
    
    async def invoke(
        self,
        message: str,
        context: Optional[dict] = None
    ) -> str:
        """Invoke the agent with a message."""
        response = self.client.converse(
            modelId=self.model_id,
            messages=[
                {"role": "user", "content": [{"text": message}]}
            ],
            system=[{"text": self.get_system_prompt()}]
        )
        return response["output"]["message"]["content"][0]["text"]
```

**Tool-Enabled Agent:**
```python
from dataclasses import dataclass
from typing import Callable

@dataclass
class AgentTool:
    name: str
    description: str
    parameters: dict
    handler: Callable

class ToolEnabledAgent(BedrockAgent):
    """Agent with tool/function calling capabilities."""
    
    def __init__(self, **kwargs):
        super().__init__(**kwargs)
        self.tools: list[AgentTool] = []
    
    def register_tool(self, tool: AgentTool):
        """Register a tool for the agent to use."""
        self.tools.append(tool)
    
    def get_tool_config(self) -> dict:
        """Generate tool configuration for Bedrock."""
        return {
            "tools": [
                {
                    "toolSpec": {
                        "name": tool.name,
                        "description": tool.description,
                        "inputSchema": {
                            "json": tool.parameters
                        }
                    }
                }
                for tool in self.tools
            ]
        }
    
    async def invoke_with_tools(self, message: str) -> str:
        """Invoke agent with tool use enabled."""
        response = self.client.converse(
            modelId=self.model_id,
            messages=[
                {"role": "user", "content": [{"text": message}]}
            ],
            system=[{"text": self.get_system_prompt()}],
            toolConfig=self.get_tool_config()
        )
        
        # Handle tool use in response
        return await self._process_response(response)
```

**Prompt Management:**
```python
from pathlib import Path
from string import Template

class PromptManager:
    """Manage and render agent prompts."""
    
    def __init__(self, prompts_dir: str = "src/agents/prompts"):
        self.prompts_dir = Path(prompts_dir)
    
    def load_prompt(self, name: str) -> str:
        """Load a prompt template by name."""
        path = self.prompts_dir / f"{name}.txt"
        return path.read_text()
    
    def render_prompt(self, name: str, **variables) -> str:
        """Load and render a prompt with variables."""
        template = Template(self.load_prompt(name))
        return template.safe_substitute(**variables)
```

### Agent Patterns

**Conversation Agent:**
```python
class ConversationAgent(BedrockAgent):
    """Agent for multi-turn conversations."""
    
    def __init__(self, **kwargs):
        super().__init__(**kwargs)
        self.conversation_history = []
    
    async def chat(self, message: str) -> str:
        """Send a message and maintain conversation history."""
        self.conversation_history.append({
            "role": "user",
            "content": [{"text": message}]
        })
        
        response = self.client.converse(
            modelId=self.model_id,
            messages=self.conversation_history,
            system=[{"text": self.get_system_prompt()}]
        )
        
        assistant_message = response["output"]["message"]
        self.conversation_history.append(assistant_message)
        
        return assistant_message["content"][0]["text"]
    
    def reset_conversation(self):
        """Clear conversation history."""
        self.conversation_history = []
```

**Workflow Agent:**
```python
from enum import Enum

class WorkflowState(Enum):
    INIT = "init"
    PROCESSING = "processing"
    AWAITING_INPUT = "awaiting_input"
    COMPLETE = "complete"
    ERROR = "error"

class WorkflowAgent(ToolEnabledAgent):
    """Agent for complex multi-step workflows."""
    
    def __init__(self, **kwargs):
        super().__init__(**kwargs)
        self.state = WorkflowState.INIT
        self.workflow_context = {}
    
    async def execute_workflow(self, initial_input: str) -> dict:
        """Execute a complete workflow."""
        self.state = WorkflowState.PROCESSING
        
        try:
            result = await self.invoke_with_tools(initial_input)
            self.state = WorkflowState.COMPLETE
            return {"success": True, "result": result}
        except Exception as e:
            self.state = WorkflowState.ERROR
            return {"success": False, "error": str(e)}
```

---

## Environment Configuration

### Backend (.env)
```
# Application
APP_ENV=development
APP_DEBUG=true
LOG_LEVEL=INFO

# Database
DATABASE_URL=postgresql://user:pass@localhost:5432/dbname

# AWS
AWS_REGION=us-east-1
AWS_ACCESS_KEY_ID=
AWS_SECRET_ACCESS_KEY=

# Bedrock
BEDROCK_MODEL_ID=anthropic.claude-3-sonnet-20240229-v1:0
```

### Frontend (.env)
```
VITE_API_BASE_URL=http://localhost:8000/api/v1
VITE_APP_ENV=development
```

---

## Testing Requirements

### Backend Testing
```python
import pytest
from httpx import AsyncClient

@pytest.mark.asyncio
async def test_get_user():
    async with AsyncClient(app=app, base_url="http://test") as client:
        response = await client.get("/api/v1/users/123")
        assert response.status_code == 200
        assert response.json()["success"] is True
```

### Frontend Testing
```javascript
import { render, screen } from '@testing-library/react';
import { UserCard } from './UserCard';

describe('UserCard', () => {
  it('renders user information', () => {
    const user = { id: '1', name: 'John Doe', email: 'john@example.com' };
    render(<UserCard user={user} onEdit={() => {}} />);
    
    expect(screen.getByText('John Doe')).toBeInTheDocument();
    expect(screen.getByText('john@example.com')).toBeInTheDocument();
  });
});
```

---

## Code Review Checklist

Before submitting code:

- [ ] Type hints/JSDoc present on all functions
- [ ] Error handling implemented
- [ ] Unit tests written
- [ ] No hardcoded secrets or credentials
- [ ] Follows naming conventions
- [ ] shadcn components used where applicable
- [ ] API responses use consistent structure
- [ ] Bedrock agents follow base patterns
