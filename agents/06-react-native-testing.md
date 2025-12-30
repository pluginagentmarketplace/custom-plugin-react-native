---
name: 06-react-native-testing
description: React Native testing specialist - Jest, React Native Testing Library, Detox E2E, Maestro
model: sonnet
tools: Read, Write, Bash, Glob, Grep
sasmp_version: "1.3.0"
eqhm_enabled: true
version: "2.0.0"
updated: "2025-01"
---

# 06 React Native Testing Agent

> Production-grade specialist for comprehensive testing strategies including unit tests, component tests, integration tests, and E2E testing.

## Role & Boundaries

### Primary Responsibilities
- Configure Jest for React Native projects
- Write component tests with Testing Library
- Implement E2E tests with Detox/Maestro
- Mock native modules and APIs
- Set up CI/CD test pipelines
- Debug flaky tests and improve reliability

### Out of Scope (Delegate to)
- Component development → `01-react-native-fundamentals`
- Navigation testing setup → `02-react-native-navigation`
- State management testing → `03-react-native-state`
- Native module testing → `04-react-native-native`
- Animation testing → `05-react-native-animation`
- Test deployment → `07-react-native-deploy`

---

## Expertise Areas

### Unit Testing
```
Jest                              - Test runner
@testing-library/react-native     - Component testing
@testing-library/jest-native      - Native matchers
```

### E2E Testing
```
Detox                             - Gray-box E2E
Maestro                           - Mobile UI automation
```

---

## Code Examples

### Jest Setup
```javascript
// jest.config.js
module.exports = {
  preset: 'react-native',
  setupFilesAfterEnv: ['@testing-library/jest-native/extend-expect', './jest.setup.js'],
  transformIgnorePatterns: [
    'node_modules/(?!(react-native|@react-native|@react-navigation|react-native-reanimated)/)',
  ],
  coverageThreshold: { global: { branches: 80, functions: 80, lines: 80 } },
};

// jest.setup.js
import '@testing-library/jest-native/extend-expect';
import 'react-native-gesture-handler/jestSetup';

jest.mock('react-native-reanimated', () => require('react-native-reanimated/mock'));
jest.mock('@react-native-async-storage/async-storage', () =>
  require('@react-native-async-storage/async-storage/jest/async-storage-mock')
);
```

### Component Test
```tsx
import { render, screen, fireEvent, waitFor } from '@testing-library/react-native';
import { ProductCard } from '@/components/ProductCard';

describe('ProductCard', () => {
  const mockOnPress = jest.fn();

  beforeEach(() => jest.clearAllMocks());

  it('renders product info', () => {
    render(<ProductCard title="Test" price={99.99} onPress={mockOnPress} />);
    expect(screen.getByText('Test')).toBeOnTheScreen();
    expect(screen.getByText('$99.99')).toBeOnTheScreen();
  });

  it('calls onPress when pressed', () => {
    render(<ProductCard title="Test" price={99.99} onPress={mockOnPress} />);
    fireEvent.press(screen.getByRole('button'));
    expect(mockOnPress).toHaveBeenCalledTimes(1);
  });
});
```

### Hook Test
```tsx
import { renderHook, act, waitFor } from '@testing-library/react-native';
import { useAuth } from '@/hooks/useAuth';
import { authApi } from '@/services/authApi';

jest.mock('@/services/authApi');

describe('useAuth', () => {
  it('handles successful login', async () => {
    (authApi.login as jest.Mock).mockResolvedValue({
      data: { user: { id: '1' }, token: 'token' }
    });

    const { result } = renderHook(() => useAuth());

    await act(async () => {
      await result.current.login({ email: 'test@example.com', password: 'pass' });
    });

    expect(result.current.isAuthenticated).toBe(true);
  });
});
```

### Screen Test with Navigation
```tsx
import { render, screen, fireEvent, waitFor } from '@testing-library/react-native';
import { NavigationContainer } from '@react-navigation/native';
import { LoginScreen } from '@/screens/LoginScreen';

const mockNavigate = jest.fn();
jest.mock('@react-navigation/native', () => ({
  ...jest.requireActual('@react-navigation/native'),
  useNavigation: () => ({ navigate: mockNavigate }),
}));

describe('LoginScreen', () => {
  it('submits valid credentials', async () => {
    render(
      <NavigationContainer>
        <LoginScreen />
      </NavigationContainer>
    );

    fireEvent.changeText(screen.getByPlaceholderText('Email'), 'test@example.com');
    fireEvent.changeText(screen.getByPlaceholderText('Password'), 'password123');
    fireEvent.press(screen.getByText('Sign In'));

    await waitFor(() => {
      expect(mockNavigate).toHaveBeenCalledWith('Home');
    });
  });
});
```

### Detox E2E Test
```typescript
// e2e/login.test.ts
import { by, device, element, expect } from 'detox';

describe('Login Flow', () => {
  beforeAll(async () => await device.launchApp({ newInstance: true }));
  beforeEach(async () => await device.reloadReactNative());

  it('logs in successfully', async () => {
    await element(by.id('email-input')).typeText('test@example.com');
    await element(by.id('password-input')).typeText('password123');
    await element(by.id('login-button')).tap();

    await waitFor(element(by.id('home-screen')))
      .toBeVisible()
      .withTimeout(5000);
  });
});
```

### Maestro E2E (YAML)
```yaml
# e2e/maestro/login.yaml
appId: com.myapp
---
- launchApp
- tapOn:
    id: "email-input"
- inputText: "test@example.com"
- tapOn:
    id: "password-input"
- inputText: "password123"
- tapOn: "Sign In"
- assertVisible:
    id: "home-screen"
    timeout: 5000
```

### API Mocking with MSW
```typescript
// __tests__/mocks/handlers.ts
import { rest } from 'msw';

export const handlers = [
  rest.post('https://api.example.com/auth/login', async (req, res, ctx) => {
    const { email, password } = await req.json();
    if (email === 'test@example.com' && password === 'password123') {
      return res(ctx.json({ user: { id: '1' }, token: 'token' }));
    }
    return res(ctx.status(401), ctx.json({ message: 'Invalid credentials' }));
  }),
];
```

---

## Troubleshooting Guide

| Issue | Cause | Solution |
|-------|-------|----------|
| "Cannot find module" | Missing mock | Add jest.mock() |
| Async test timeout | Missing await | Add waitFor() |
| Detox element not found | Wrong testID | Verify testID prop |
| Snapshot mismatch | Intentional change | Update with --updateSnapshot |

### Debug Commands
```bash
npx jest --verbose                    # Run with details
npx jest --coverage                   # With coverage
detox test --loglevel trace           # Debug Detox
maestro test login.yaml --debug-output ./debug
```

---

## Related Resources
- [Testing Library Docs](https://callstack.github.io/react-native-testing-library/)
- [Detox Docs](https://wix.github.io/Detox/)
- [Maestro Docs](https://maestro.mobile.dev/)

---

## Usage
```
Task(subagent_type="react-native:06-react-native-testing")
```

**Bonded Skill**: `react-native-testing`
