# Testing Guide

## Overview

We use Vitest and React Testing Library for comprehensive testing of our React application.

## Testing Stack

- **Test Runner**: Vitest
- **Component Testing**: React Testing Library (RTL)
- **Mocking**: Vitest mocks
- **Coverage**: c8 (built into Vitest)
- **E2E**: Playwright (separate suite)

## Running Tests

```bash
# Run all tests
npm test

# Run tests in watch mode
npm run test:watch

# Run tests with coverage
npm run test:coverage

# Run specific test file
npm test -- user.test.tsx

# Run tests matching pattern
npm test -- --grep "authentication"
```

## Unit Testing

### Component Tests

Test components in isolation:

```tsx
// src/components/Button.test.tsx
import { render, screen, fireEvent } from '@testing-library/react';
import { describe, it, expect, vi } from 'vitest';
import { Button } from './Button';

describe('Button', () => {
  it('renders with label', () => {
    render(<Button label="Click me" />);
    expect(screen.getByText('Click me')).toBeInTheDocument();
  });

  it('calls onClick when clicked', () => {
    const handleClick = vi.fn();
    render(<Button label="Click" onClick={handleClick} />);
    
    fireEvent.click(screen.getByText('Click'));
    expect(handleClick).toHaveBeenCalledTimes(1);
  });

  it('is disabled when disabled prop is true', () => {
    render(<Button label="Click" disabled />);
    expect(screen.getByRole('button')).toBeDisabled();
  });

  it('shows loading state', () => {
    render(<Button label="Submit" loading />);
    expect(screen.getByRole('button')).toHaveAttribute('aria-busy', 'true');
  });
});
```

### Hook Tests

Test custom hooks:

```tsx
// src/hooks/useAuth.test.ts
import { renderHook, act } from '@testing-library/react';
import { describe, it, expect } from 'vitest';
import { useAuth } from './useAuth';
import { Provider } from 'react-redux';
import { store } from 'store';

const wrapper = ({ children }) => <Provider store={store}>{children}</Provider>;

describe('useAuth', () => {
  it('initializes with no user', () => {
    const { result } = renderHook(() => useAuth(), { wrapper });
    
    expect(result.current.user).toBeNull();
    expect(result.current.isAuthenticated).toBe(false);
  });

  it('sets user on login', () => {
    const { result } = renderHook(() => useAuth(), { wrapper });
    
    act(() => {
      result.current.login({ id: '1', email: 'test@example.com', name: 'Test User' });
    });

    expect(result.current.user).toEqual({
      id: '1',
      email: 'test@example.com',
      name: 'Test User',
    });
    expect(result.current.isAuthenticated).toBe(true);
  });
});
```

## Integration Testing

Test feature workflows:

```tsx
// src/features/auth/Login.test.tsx
import { render, screen, fireEvent, waitFor } from '@testing-library/react';
import { describe, it, expect, vi } from 'vitest';
import { LoginPage } from './LoginPage';
import { authService } from 'services/authService';

vi.mock('services/authService');

describe('LoginPage', () => {
  it('submits login form successfully', async () => {
    const mockLogin = vi.fn().mockResolvedValue({
      user: { id: '1', email: 'test@example.com' },
      token: 'fake-token',
    });
    authService.login = mockLogin;

    render(<LoginPage />);

    fireEvent.change(screen.getByLabelText(/email/i), {
      target: { value: 'test@example.com' },
    });
    fireEvent.change(screen.getByLabelText(/password/i), {
      target: { value: 'password123' },
    });

    fireEvent.click(screen.getByRole('button', { name: /sign in/i }));

    await waitFor(() => {
      expect(mockLogin).toHaveBeenCalledWith({
        email: 'test@example.com',
        password: 'password123',
      });
    });
  });

  it('shows error on login failure', async () => {
    authService.login = vi.fn().mockRejectedValue(new Error('Invalid credentials'));

    render(<LoginPage />);

    fireEvent.change(screen.getByLabelText(/email/i), {
      target: { value: 'wrong@example.com' },
    });
    fireEvent.change(screen.getByLabelText(/password/i), {
      target: { value: 'wrongpass' },
    });

    fireEvent.click(screen.getByRole('button', { name: /sign in/i }));

    await waitFor(() => {
      expect(screen.getByText(/invalid credentials/i)).toBeInTheDocument();
    });
  });
});
```

## Mocking

### API Mocking

Mock API calls:

```tsx
import { vi } from 'vitest';
import axios from 'axios';

vi.mock('axios');

describe('UserService', () => {
  it('fetches user data', async () => {
    const mockUser = { id: '1', name: 'John Doe' };
    axios.get.mockResolvedValue({ data: mockUser });

    const user = await userService.getUser('1');
    
    expect(user).toEqual(mockUser);
    expect(axios.get).toHaveBeenCalledWith('/users/1');
  });
});
```

### Redux Store Mocking

```tsx
import { configureStore } from '@reduxjs/toolkit';
import authReducer from 'features/auth/authSlice';

const createMockStore = (initialState = {}) => {
  return configureStore({
    reducer: {
      auth: authReducer,
    },
    preloadedState: initialState,
  });
};

const mockStore = createMockStore({
  auth: {
    user: { id: '1', email: 'test@example.com' },
    isAuthenticated: true,
  },
});
```

## Testing Utilities

### Custom Render

Create a custom render function with providers:

```tsx
// tests/utils.tsx
import { render } from '@testing-library/react';
import { Provider } from 'react-redux';
import { BrowserRouter } from 'react-router-dom';
import { store } from 'store';

export const renderWithProviders = (ui: React.ReactElement, options = {}) => {
  const Wrapper = ({ children }) => (
    <Provider store={store}>
      <BrowserRouter>
        {children}
      </BrowserRouter>
    </Provider>
  );

  return render(ui, { wrapper: Wrapper, ...options });
};
```

### Test Data Factories

```tsx
// tests/factories/user.factory.ts
export const createMockUser = (overrides = {}) => ({
  id: '1',
  email: 'test@example.com',
  name: 'Test User',
  role: 'user',
  createdAt: new Date().toISOString(),
  ...overrides,
});
```

## Coverage Goals

Maintain high test coverage:

- **Statements**: > 80%
- **Branches**: > 75%
- **Functions**: > 80%
- **Lines**: > 80%

```bash
# Generate coverage report
npm run test:coverage

# View HTML report
open coverage/index.html
```

## Best Practices

### 1. Test User Behavior, Not Implementation

```tsx
// ❌ Bad - testing implementation
expect(component.state.count).toBe(1);

// ✅ Good - testing user-facing behavior
expect(screen.getByText('Count: 1')).toBeInTheDocument();
```

### 2. Use Semantic Queries

```tsx
// ❌ Bad
screen.getByTestId('submit-button');

// ✅ Good
screen.getByRole('button', { name: /submit/i });
```

### 3. Avoid Testing External Libraries

Don't test third-party code - trust their tests.

### 4. Keep Tests Focused

One assertion concept per test:

```tsx
describe('UserProfile', () => {
  it('displays user name');
  it('displays user email');
  it('allows editing profile');
});
```

### 5. Use Descriptive Test Names

```tsx
it('shows error message when email is invalid');
it('disables submit button while loading');
it('redirects to dashboard after successful login');
```

## Debugging Tests

```tsx
import { screen, debug } from '@testing-library/react';

// Print DOM
debug();

// Print specific element
debug(screen.getByRole('button'));

// Use logRoles to find accessible roles
logRoles(container);
```

## Continuous Integration

Tests run automatically on:
- Pull requests
- Commits to main branch
- Pre-deployment

CI configuration ensures all tests pass before merging.
