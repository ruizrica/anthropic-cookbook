---
name: refactoring-assistant
description: Identify code smells and apply refactoring patterns to improve code quality and maintainability
---

# Refactoring Assistant

## Code Smells

### 1. Long Method
```typescript
// ❌ Before: 100+ lines
function processOrder(order) {
  // Validate
  if (!order.items.length) throw new Error('Empty cart');
  // Calculate totals
  let total = 0;
  for (const item of order.items) {
    total += item.price * item.quantity;
  }
  // Apply discounts
  // Process payment
  // Send confirmation email
  // Update inventory
  // ...
}

// ✅ After: Extract methods
function processOrder(order) {
  validateOrder(order);
  const total = calculateTotal(order);
  const finalAmount = applyDiscounts(total, order.discountCode);
  processPayment(order, finalAmount);
  sendConfirmation(order);
  updateInventory(order.items);
}
```

### 2. Duplicate Code
```typescript
// ❌ Before
function createUser(data) {
  const user = new User();
  user.name = data.name;
  user.email = data.email;
  user.createdAt = new Date();
  return user.save();
}

function createAdmin(data) {
  const admin = new User();
  admin.name = data.name;
  admin.email = data.email;
  admin.role = 'admin';
  admin.createdAt = new Date();
  return admin.save();
}

// ✅ After
function createUser(data, role = 'user') {
  return new User({
    ...data,
    role,
    createdAt: new Date()
  }).save();
}
```

### 3. Large Class
```typescript
// ❌ Before: God class with too many responsibilities
class UserManager {
  validateEmail() {}
  hashPassword() {}
  sendEmail() {}
  generateReport() {}
  exportToPDF() {}
}

// ✅ After: Single Responsibility Principle
class UserValidator {
  validateEmail() {}
}

class PasswordHasher {
  hash() {}
  verify() {}
}

class EmailService {
  send() {}
}

class ReportGenerator {
  generate() {}
  exportToPDF() {}
}
```

### 4. Primitive Obsession
```typescript
// ❌ Before
function calculateShipping(country: string, weight: number, express: boolean) {
  // Complex logic with primitives
}

// ✅ After
class ShippingRequest {
  constructor(
    public destination: Country,
    public package: Package,
    public deliverySpeed: DeliverySpeed
  ) {}
}

function calculateShipping(request: ShippingRequest) {
  return request.deliverySpeed.calculate(request.destination, request.package);
}
```

## Refactoring Patterns

### Extract Function
```typescript
// Before
const orderTotal = order.items.reduce((sum, item) =>
  sum + item.price * item.quantity * (1 - item.discount), 0);

// After
function calculateOrderTotal(order: Order): number {
  return order.items.reduce((sum, item) =>
    sum + calculateItemTotal(item), 0);
}

function calculateItemTotal(item: OrderItem): number {
  return item.price * item.quantity * (1 - item.discount);
}
```

### Replace Conditional with Polymorphism
```typescript
// Before
function getSpeed(type: string) {
  switch (type) {
    case 'car': return 100;
    case 'plane': return 500;
    case 'bike': return 20;
  }
}

// After
interface Vehicle {
  getSpeed(): number;
}

class Car implements Vehicle {
  getSpeed() { return 100; }
}

class Plane implements Vehicle {
  getSpeed() { return 500; }
}
```
