# Component Library

## Overview

Our component library provides a consistent set of reusable UI components built with React, TypeScript, and Tailwind CSS.

## Core Components

### Button

A versatile button component with multiple variants.

**Props:**

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| label | string | required | Button text |
| variant | 'primary' \| 'secondary' \| 'danger' | 'primary' | Visual style |
| size | 'sm' \| 'md' \| 'lg' | 'md' | Button size |
| disabled | boolean | false | Disabled state |
| loading | boolean | false | Loading state |
| onClick | () => void | - | Click handler |

**Example:**

```tsx
import { Button } from 'components/common/Button';

<Button 
  label="Submit" 
  variant="primary"
  onClick={handleSubmit}
/>
```

### Input

Text input component with validation support.

**Props:**

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| label | string | - | Input label |
| type | string | 'text' | Input type |
| placeholder | string | - | Placeholder text |
| error | string | - | Error message |
| required | boolean | false | Required field |
| onChange | (value: string) => void | - | Change handler |

**Example:**

```tsx
import { Input } from 'components/common/Input';

<Input 
  label="Email"
  type="email"
  placeholder="Enter your email"
  error={errors.email}
  onChange={(value) => setEmail(value)}
/>
```

### Card

Container component for grouping content.

**Example:**

```tsx
import { Card } from 'components/common/Card';

<Card title="User Profile" subtitle="Manage your account">
  <p>Card content goes here</p>
</Card>
```

### Modal

Dialog component for overlays and confirmations.

**Example:**

```tsx
import { Modal } from 'components/common/Modal';

<Modal
  isOpen={isModalOpen}
  onClose={() => setIsModalOpen(false)}
  title="Confirm Action"
>
  <p>Are you sure you want to proceed?</p>
  <div className="flex gap-2 mt-4">
    <Button label="Cancel" variant="secondary" onClick={handleCancel} />
    <Button label="Confirm" variant="primary" onClick={handleConfirm} />
  </div>
</Modal>
```

### Table

Data table with sorting and pagination.

**Example:**

```tsx
import { Table } from 'components/common/Table';

const columns = [
  { key: 'name', label: 'Name' },
  { key: 'email', label: 'Email' },
  { key: 'role', label: 'Role' },
];

const data = [
  { id: 1, name: 'John Doe', email: 'john@example.com', role: 'Admin' },
  { id: 2, name: 'Jane Smith', email: 'jane@example.com', role: 'User' },
];

<Table 
  columns={columns}
  data={data}
  onRowClick={(row) => console.log(row)}
/>
```

## Form Components

### FormField

Wrapper for form inputs with label and validation.

```tsx
import { FormField } from 'components/forms/FormField';
import { useForm } from 'react-hook-form';

const { register, formState: { errors } } = useForm();

<FormField
  label="Username"
  error={errors.username?.message}
  required
>
  <input 
    {...register('username')} 
    className="input"
  />
</FormField>
```

### Select

Dropdown selection component.

```tsx
import { Select } from 'components/forms/Select';

const options = [
  { value: 'admin', label: 'Administrator' },
  { value: 'user', label: 'User' },
  { value: 'guest', label: 'Guest' },
];

<Select
  label="Role"
  options={options}
  value={selectedRole}
  onChange={setSelectedRole}
/>
```

### DatePicker

Date selection component.

```tsx
import { DatePicker } from 'components/forms/DatePicker';

<DatePicker
  label="Birth Date"
  value={birthDate}
  onChange={setBirthDate}
  minDate={new Date('1900-01-01')}
/>
```

## Layout Components

### Header

Application header with navigation.

```tsx
import { Header } from 'components/layout/Header';

<Header 
  user={currentUser}
  onLogout={handleLogout}
/>
```

### Sidebar

Navigation sidebar component.

```tsx
import { Sidebar } from 'components/layout/Sidebar';

const menuItems = [
  { icon: 'dashboard', label: 'Dashboard', path: '/dashboard' },
  { icon: 'users', label: 'Users', path: '/users' },
  { icon: 'settings', label: 'Settings', path: '/settings' },
];

<Sidebar items={menuItems} />
```

### Container

Responsive content container.

```tsx
import { Container } from 'components/layout/Container';

<Container maxWidth="lg">
  <h1>Page Content</h1>
</Container>
```

## Feedback Components

### Alert

Notification alert component.

```tsx
import { Alert } from 'components/common/Alert';

<Alert 
  type="success"
  title="Success!"
  message="Your changes have been saved."
  onClose={handleClose}
/>
```

### Toast

Toast notification for temporary messages.

```tsx
import { useToast } from 'hooks/useToast';

const { showToast } = useToast();

showToast({
  type: 'info',
  message: 'Profile updated successfully',
  duration: 3000,
});
```

### Spinner

Loading spinner component.

```tsx
import { Spinner } from 'components/common/Spinner';

<Spinner size="lg" />
```

## Best Practices

### Component Composition

Build complex UIs by composing smaller components:

```tsx
const UserCard = ({ user }) => (
  <Card>
    <div className="flex items-center gap-4">
      <Avatar src={user.avatar} alt={user.name} />
      <div>
        <h3>{user.name}</h3>
        <p className="text-gray-600">{user.email}</p>
      </div>
    </div>
    <div className="mt-4">
      <Button label="Edit Profile" variant="secondary" />
    </div>
  </Card>
);
```

### Prop Types

Always define prop types with TypeScript interfaces:

```tsx
interface ComponentProps {
  title: string;
  subtitle?: string;
  children: React.ReactNode;
  onAction?: () => void;
}

export const Component: FC<ComponentProps> = ({ 
  title, 
  subtitle, 
  children,
  onAction 
}) => {
  // Component implementation
};
```

### Accessibility

Ensure components are accessible:

```tsx
<button
  aria-label="Close dialog"
  aria-pressed={isActive}
  role="button"
  onClick={handleClick}
>
  Close
</button>
```
