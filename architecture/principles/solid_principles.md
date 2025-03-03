# SOLID Principles Architecture Guidelines

## Overview
This document outlines how to apply SOLID principles in our projects, with practical examples using our Vue.js and Python tech stack. These principles help create maintainable, flexible, and robust software systems.

## Single Responsibility Principle (SRP)
"A class/module should have only one reason to change"

### Guidelines
- Each class/module should focus on a single, well-defined task
- Break down complex functionality into smaller, focused components
- Consider responsibilities in terms of actors who might request changes

### Examples

#### Python
```python
# Bad - Multiple responsibilities
class UserManager:
    def create_user(self, data):
        # Handle user creation
        pass
        
    def send_welcome_email(self, user):
        # Handle email sending
        pass
        
    def log_user_activity(self, user, action):
        # Handle logging
        pass

# Good - Single responsibility
class UserCreator:
    def create_user(self, data):
        # Handle only user creation
        pass

class EmailService:
    def send_welcome_email(self, user):
        # Handle only email sending
        pass

class ActivityLogger:
    def log_activity(self, user, action):
        # Handle only logging
        pass
```

#### Vue.js
```typescript
// Bad - Component handling too many concerns
export default {
  name: 'UserDashboard',
  data() {
    return {
      user: null,
      posts: [],
      notifications: []
    }
  },
  methods: {
    async fetchUserData() { /* ... */ },
    async fetchPosts() { /* ... */ },
    async handleNotifications() { /* ... */ },
    formatData() { /* ... */ },
    updateUI() { /* ... */ }
  }
}

// Good - Split into focused components
// UserProfile.vue
export default {
  name: 'UserProfile',
  props: {
    userId: String
  },
  data() {
    return {
      user: null
    }
  },
  methods: {
    async fetchUserData() { /* ... */ }
  }
}

// UserPosts.vue
export default {
  name: 'UserPosts',
  props: {
    userId: String
  },
  data() {
    return {
      posts: []
    }
  },
  methods: {
    async fetchPosts() { /* ... */ }
  }
}
```

## Open/Closed Principle (OCP)
"Software entities should be open for extension but closed for modification"

### Guidelines
- Use interfaces and abstract classes to define contracts
- Implement new functionality through inheritance or composition
- Utilize strategy pattern for varying behaviors
- Use dependency injection to swap implementations

### Examples

#### Python
```python
from abc import ABC, abstractmethod

# Good - Open for extension
class PaymentProcessor(ABC):
    @abstractmethod
    def process_payment(self, amount: float) -> bool:
        pass

class CreditCardProcessor(PaymentProcessor):
    def process_payment(self, amount: float) -> bool:
        # Process credit card payment
        pass

class PayPalProcessor(PaymentProcessor):
    def process_payment(self, amount: float) -> bool:
        # Process PayPal payment
        pass

# New payment method can be added without modifying existing code
class CryptoProcessor(PaymentProcessor):
    def process_payment(self, amount: float) -> bool:
        # Process cryptocurrency payment
        pass
```

#### Vue.js
```typescript
// Good - Component open for extension
interface DataFormatter {
  format(data: any): string;
}

class DefaultFormatter implements DataFormatter {
  format(data: any): string {
    return String(data);
  }
}

class CurrencyFormatter implements DataFormatter {
  format(data: any): string {
    return new Intl.NumberFormat('en-US', {
      style: 'currency',
      currency: 'USD'
    }).format(data);
  }
}

// Vue component using formatter
export default {
  name: 'DataDisplay',
  props: {
    data: Any,
    formatter: {
      type: Object as () => DataFormatter,
      default: () => new DefaultFormatter()
    }
  },
  computed: {
    formattedData() {
      return this.formatter.format(this.data);
    }
  }
}
```

## Liskov Substitution Principle (LSP)
"Subtypes must be substitutable for their base types"

### Guidelines
- Ensure derived classes can replace base classes without breaking functionality
- Maintain consistent behavior in inheritance hierarchies
- Honor contracts defined by base classes
- Avoid throwing unexpected exceptions in derived classes

### Examples

#### Python
```python
from abc import ABC, abstractmethod

class Bird(ABC):
    @abstractmethod
    def fly(self, altitude: int) -> None:
        pass

# Bad - Violates LSP
class Penguin(Bird):  # Penguins can't fly!
    def fly(self, altitude: int) -> None:
        raise NotImplementedError("Penguins can't fly")

# Good - Proper abstraction
class Animal(ABC):
    @abstractmethod
    def move(self) -> None:
        pass

class FlyingBird(Animal):
    def move(self) -> None:
        self.fly()
    
    def fly(self) -> None:
        # Implement flying
        pass

class WalkingBird(Animal):
    def move(self) -> None:
        self.walk()
    
    def walk(self) -> None:
        # Implement walking
        pass
```

#### Vue.js
```typescript
// Good - Proper component inheritance
interface ButtonProps {
  label: string;
  onClick: () => void;
}

// Base button component
const BaseButton = {
  props: {
    label: String,
    onClick: Function
  },
  template: '<button @click="handleClick">{{ label }}</button>',
  methods: {
    handleClick() {
      this.onClick();
    }
  }
}

// Specialized button maintaining base behavior
const ConfirmButton = {
  extends: BaseButton,
  methods: {
    handleClick() {
      // Add confirmation before calling parent behavior
      if (confirm('Are you sure?')) {
        this.onClick();
      }
    }
  }
}
```

## Interface Segregation Principle (ISP)
"Clients should not be forced to depend on interfaces they don't use"

### Guidelines
- Create focused, minimal interfaces
- Split large interfaces into smaller ones
- Design interfaces based on client needs
- Avoid "fat" interfaces with too many methods

### Examples

#### Python
```python
from abc import ABC, abstractmethod

# Bad - Fat interface
class Worker(ABC):
    @abstractmethod
    def work(self):
        pass
    
    @abstractmethod
    def eat(self):
        pass
    
    @abstractmethod
    def sleep(self):
        pass

# Good - Segregated interfaces
class Workable(ABC):
    @abstractmethod
    def work(self):
        pass

class Eatable(ABC):
    @abstractmethod
    def eat(self):
        pass

class Sleepable(ABC):
    @abstractmethod
    def sleep(self):
        pass

# Classes implement only needed interfaces
class Human(Workable, Eatable, Sleepable):
    def work(self):
        pass
    
    def eat(self):
        pass
    
    def sleep(self):
        pass

class Robot(Workable):
    def work(self):
        pass
```

#### Vue.js
```typescript
// Bad - Component prop interface too broad
interface UserProps {
  id: string;
  name: string;
  email: string;
  address: string;
  paymentInfo: object;
  orderHistory: array;
}

// Good - Segregated interfaces
interface UserBasicInfo {
  id: string;
  name: string;
  email: string;
}

interface UserAddress {
  address: string;
}

interface UserPayment {
  paymentInfo: object;
}

interface UserOrders {
  orderHistory: array;
}

// Components use only required interfaces
const UserProfile = {
  props: {
    user: Object as () => UserBasicInfo
  }
}

const UserAddressForm = {
  props: {
    address: Object as () => UserAddress
  }
}
```

## Dependency Inversion Principle (DIP)
"High-level modules should not depend on low-level modules. Both should depend on abstractions"

### Guidelines
- Depend on abstractions rather than concrete implementations
- Use dependency injection
- Define clear interfaces for components
- Avoid tight coupling between modules

### Examples

#### Python
```python
# Bad - Direct dependency on concrete class
class OrderProcessor:
    def __init__(self):
        self.payment_processor = CreditCardProcessor()
    
    def process_order(self, order):
        self.payment_processor.process_payment(order.total)

# Good - Depends on abstraction
class OrderProcessor:
    def __init__(self, payment_processor: PaymentProcessor):
        self.payment_processor = payment_processor
    
    def process_order(self, order):
        self.payment_processor.process_payment(order.total)

# Usage
order_processor = OrderProcessor(CreditCardProcessor())
# or
order_processor = OrderProcessor(PayPalProcessor())
```

#### Vue.js
```typescript
// Good - Using Vue's dependency injection
// Service interfaces
interface AuthService {
  login(credentials: any): Promise<void>;
  logout(): Promise<void>;
}

// Service implementation
class ApiAuthService implements AuthService {
  async login(credentials: any): Promise<void> {
    // Implementation
  }
  
  async logout(): Promise<void> {
    // Implementation
  }
}

// Main app providing the service
const app = createApp({
  setup() {
    // Provide the service
    provide('auth', new ApiAuthService());
  }
});

// Components depending on abstraction
export default {
  setup() {
    // Inject the service
    const auth = inject<AuthService>('auth');
    
    const handleLogin = async (credentials: any) => {
      await auth.login(credentials);
    };
    
    return {
      handleLogin
    };
  }
}
```

## Implementation Checklist
- [ ] Apply SRP by breaking down large classes/components
- [ ] Use interfaces/abstract classes to make code extensible (OCP)
- [ ] Ensure inheritance hierarchies are sound (LSP)
- [ ] Create focused interfaces (ISP)
- [ ] Implement dependency injection (DIP)
- [ ] Write tests to verify SOLID compliance
- [ ] Review existing code for SOLID violations
- [ ] Document architectural decisions

## Common Pitfalls
1. Over-engineering simple solutions
2. Creating unnecessary abstractions
3. Breaking LSP in pursuit of features
4. Implementing interfaces that are too granular
5. Circular dependencies in DIP implementations

## Further Reading
- Clean Code by Robert C. Martin
- Head First Design Patterns
- Agile Principles, Patterns, and Practices in C#

## Review Process
Regular reviews should check for:
- SOLID principles compliance
- Code maintainability
- Test coverage
- Documentation completeness
