---
name: frontend-technologies
description: Master HTML, CSS, JavaScript, TypeScript, and modern frontend frameworks like React, Next.js, Vue, and Angular. Use this skill when learning web technologies, building UI components, or working with frontend frameworks.
sasmp_version: "1.3.0"
bonded_agent: 01-frontend-web-development
bond_type: PRIMARY_BOND
---

# Frontend Technologies Skill

## Quick Start

Frontend development involves creating interactive user interfaces using web technologies. Here's a quick path:

### HTML5 Fundamentals
```html
<!-- Semantic HTML structure -->
<header>
  <nav>
    <a href="/">Home</a>
  </nav>
</header>
<main>
  <article>
    <h1>Page Title</h1>
    <p>Content here</p>
  </article>
</main>
<footer>
  <p>&copy; 2024</p>
</footer>
```

### CSS3 Modern Layouts
```css
/* Flexbox for layouts */
.container {
  display: flex;
  justify-content: space-between;
  gap: 1rem;
}

/* Grid for complex layouts */
.grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 2rem;
}
```

### JavaScript ES6+
```javascript
// Arrow functions and destructuring
const users = [
  { id: 1, name: 'Alice', email: 'alice@example.com' },
  { id: 2, name: 'Bob', email: 'bob@example.com' }
];

// Array methods
const emails = users.map(user => user.email);
const active = users.filter(user => user.id > 0);

// Async/await for API calls
async function fetchData() {
  try {
    const response = await fetch('/api/data');
    const data = await response.json();
    return data;
  } catch (error) {
    console.error('Error:', error);
  }
}
```

### TypeScript Type Safety
```typescript
interface User {
  id: number;
  name: string;
  email: string;
  role: 'admin' | 'user';
}

function getUser(id: number): Promise<User> {
  return fetch(`/api/users/${id}`).then(r => r.json());
}

// Generics for reusable components
function useLocalStorage<T>(key: string, initialValue: T) {
  const [value, setValue] = useState<T>(initialValue);
  return [value, setValue] as const;
}
```

### React Hooks Pattern
```typescript
import { useState, useEffect, useCallback } from 'react';

function UserProfile({ userId }: { userId: string }) {
  const [user, setUser] = useState<User | null>(null);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    fetch(`/api/users/${userId}`)
      .then(r => r.json())
      .then(data => {
        setUser(data);
        setLoading(false);
      });
  }, [userId]);

  const updateUser = useCallback(async (updates: Partial<User>) => {
    const response = await fetch(`/api/users/${userId}`, {
      method: 'PATCH',
      body: JSON.stringify(updates)
    });
    return response.json();
  }, [userId]);

  if (loading) return <div>Loading...</div>;
  if (!user) return <div>User not found</div>;

  return (
    <div>
      <h1>{user.name}</h1>
      <p>{user.email}</p>
    </div>
  );
}
```

## Key Areas

### 1. HTML5
- Semantic markup
- Forms and validation
- Accessibility (a11y)
- Meta tags and SEO
- Canvas and SVG

### 2. CSS3
- Selectors and specificity
- Box model
- Flexbox and Grid
- Animations and transitions
- Responsive design
- CSS-in-JS solutions

### 3. JavaScript/TypeScript
- Variables and scope
- Closures and callbacks
- Promises and async/await
- ES6+ features
- DOM manipulation
- Event handling

### 4. React
- Functional components
- Hooks (useState, useEffect, useContext)
- Component composition
- State management
- Props and data flow
- Performance optimization

### 5. Next.js
- File-based routing
- Server-side rendering (SSR)
- Static generation (SSG)
- API routes
- Image optimization
- Link prefetching

### 6. Testing
- Unit testing with Jest
- Component testing with React Testing Library
- E2E testing with Cypress/Playwright
- Snapshot testing

## Resources

- **MDN Web Docs**: https://developer.mozilla.org/
- **React Documentation**: https://react.dev
- **TypeScript Handbook**: https://www.typescriptlang.org/docs/
- **Web.dev**: https://web.dev/learn/
- **CSS Tricks**: https://css-tricks.com/

## Real-World Projects

1. **TODO App**: State management and list rendering
2. **Weather App**: API integration and data fetching
3. **E-commerce Product Page**: Complex UI interactions
4. **Blog Platform**: Routing and content management
5. **Collaborative Editor**: Real-time data synchronization

---

**Next Steps:** Practice with small projects, master one framework deeply, and focus on web performance optimization.
