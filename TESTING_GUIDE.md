# StudyBuddy Testing Guide

This guide covers the comprehensive testing strategy for StudyBuddy, including unit tests, integration tests, security tests, and end-to-end tests.

## 🧪 **Testing Framework Overview**

### **Unit Testing**
- **Framework**: Jest + React Testing Library
- **Coverage**: UI components, services, utilities
- **Target**: 70% code coverage minimum

### **Integration Testing**
- **Framework**: Jest + React Testing Library
- **Coverage**: CRUD operations, Firebase integration
- **Target**: Complete user workflows

### **Security Testing**
- **Framework**: Firebase Rules Unit Testing
- **Coverage**: Firestore security rules, authentication
- **Target**: All security scenarios

### **End-to-End Testing**
- **Framework**: Cypress
- **Coverage**: Complete user journeys
- **Target**: Cross-browser compatibility

### **Responsive Testing**
- **Framework**: Jest + React Testing Library
- **Coverage**: Mobile, tablet, desktop layouts
- **Target**: All screen sizes and devices

## 🚀 **Running Tests**

### **Unit Tests**
```bash
# Run all unit tests
npm test

# Run with coverage
npm run test:coverage

# Run in CI mode
npm run test:ci
```

### **Integration Tests**
```bash
# Run integration tests
npm run test:integration
```

### **Security Tests**
```bash
# Run security rule tests
npm run test:security
```

### **Responsive Tests**
```bash
# Run responsive design tests
npm run test:responsive
```

### **End-to-End Tests**
```bash
# Run E2E tests headlessly
npm run test:e2e

# Open Cypress GUI
npm run test:e2e:open
```

### **All Tests**
```bash
# Run all test suites
npm run test:all
```

## 📋 **Test Categories**

### **1. Unit Tests**

#### **Component Tests**
- **Location**: `src/components/__tests__/`
- **Coverage**: All React components
- **Examples**:
  - `NotificationBell.test.tsx`
  - `Dashboard.test.tsx`
  - `StudyPlanDetail.test.tsx`

#### **Service Tests**
- **Location**: `src/services/__tests__/`
- **Coverage**: Business logic, API calls
- **Examples**:
  - `notificationService.test.ts`
  - `reminderService.test.ts`

#### **Utility Tests**
- **Location**: `src/utils/__tests__/`
- **Coverage**: Helper functions, formatters
- **Examples**:
  - `dateUtils.test.ts`
  - `validationUtils.test.ts`

### **2. Integration Tests**

#### **CRUD Operations**
- **Location**: `src/__tests__/integration/`
- **Coverage**: Complete data operations
- **Examples**:
  - `StudyPlanCRUD.test.tsx`
  - `UserManagement.test.tsx`

#### **Firebase Integration**
- **Location**: `src/__tests__/integration/`
- **Coverage**: Database operations
- **Examples**:
  - `FirebaseOperations.test.tsx`
  - `RealTimeUpdates.test.tsx`

### **3. Security Tests**

#### **Firebase Security Rules**
- **Location**: `src/__tests__/security/`
- **Coverage**: Access control, data protection
- **Examples**:
  - `FirebaseSecurityRules.test.ts`
  - `AuthenticationRules.test.ts`

#### **Authentication Tests**
- **Location**: `src/__tests__/security/`
- **Coverage**: Login, logout, permissions
- **Examples**:
  - `AuthFlow.test.tsx`
  - `PermissionTests.test.tsx`

### **4. Responsive Tests**

#### **Layout Tests**
- **Location**: `src/__tests__/responsive/`
- **Coverage**: All screen sizes
- **Examples**:
  - `ResponsiveDesign.test.tsx`
  - `MobileLayout.test.tsx`

#### **Cross-Browser Tests**
- **Location**: `src/__tests__/responsive/`
- **Coverage**: Chrome, Firefox, Safari, Edge
- **Examples**:
  - `CrossBrowser.test.tsx`
  - `TouchDevice.test.tsx`

### **5. End-to-End Tests**

#### **User Journeys**
- **Location**: `cypress/e2e/`
- **Coverage**: Complete workflows
- **Examples**:
  - `studybuddy.cy.js`
  - `authentication.cy.js`

#### **Performance Tests**
- **Location**: `cypress/e2e/`
- **Coverage**: Load times, responsiveness
- **Examples**:
  - `performance.cy.js`
  - `load-testing.cy.js`

## 🔧 **Test Configuration**

### **Jest Configuration**
```javascript
// jest.config.js
module.exports = {
  testEnvironment: 'jsdom',
  setupFilesAfterEnv: ['<rootDir>/src/setupTests.ts'],
  coverageThreshold: {
    global: {
      branches: 70,
      functions: 70,
      lines: 70,
      statements: 70,
    },
  },
};
```

### **Cypress Configuration**
```javascript
// cypress.config.js
module.exports = defineConfig({
  e2e: {
    baseUrl: 'http://localhost:3000',
    viewportWidth: 1280,
    viewportHeight: 720,
  },
});
```

### **Firebase Testing**
```javascript
// Firebase Rules Testing
import { initializeTestEnvironment } from '@firebase/rules-unit-testing';

const testEnv = await initializeTestEnvironment({
  projectId: 'studybuddy-test',
  firestore: { rules: '...' },
});
```

## 📊 **Coverage Reports**

### **Unit Test Coverage**
- **Target**: 70% minimum
- **Report**: HTML coverage report
- **Location**: `coverage/lcov-report/index.html`

### **Integration Test Coverage**
- **Target**: 80% minimum
- **Report**: Combined with unit tests
- **Focus**: Critical user paths

### **E2E Test Coverage**
- **Target**: 90% of user journeys
- **Report**: Cypress dashboard
- **Focus**: Complete workflows

## 🐛 **Debugging Tests**

### **Unit Test Debugging**
```bash
# Run specific test file
npm test -- NotificationBell.test.tsx

# Run with verbose output
npm test -- --verbose

# Run in watch mode
npm test -- --watch
```

### **E2E Test Debugging**
```bash
# Open Cypress GUI
npm run test:e2e:open

# Run specific test
npx cypress run --spec "cypress/e2e/studybuddy.cy.js"
```

### **Firebase Test Debugging**
```bash
# Run with debug output
npm run test:security -- --verbose

# Run specific security test
npm run test:security -- FirebaseSecurityRules.test.ts
```

## 🚨 **Common Issues and Solutions**

### **Firebase Mocking Issues**
```javascript
// Mock Firebase in setupTests.ts
jest.mock('./firebase/config', () => ({
  auth: { /* mock auth */ },
  db: { /* mock db */ },
}));
```

### **Async Test Issues**
```javascript
// Use waitFor for async operations
await waitFor(() => {
  expect(screen.getByText('Expected Text')).toBeInTheDocument();
});
```

### **Firebase Rules Testing**
```javascript
// Use proper test environment setup
beforeAll(async () => {
  testEnv = await initializeTestEnvironment({
    projectId: 'test-project',
    firestore: { rules: '...' },
  });
});
```

## 📈 **Performance Testing**

### **Load Testing**
- **Tool**: Cypress + Artillery
- **Target**: 100 concurrent users
- **Metrics**: Response time, error rate

### **Memory Testing**
- **Tool**: Chrome DevTools
- **Target**: No memory leaks
- **Metrics**: Heap usage, garbage collection

### **Network Testing**
- **Tool**: Cypress intercepts
- **Target**: Offline scenarios
- **Metrics**: Error handling, retry logic

## 🔒 **Security Testing Checklist**

### **Authentication**
- [ ] Login with valid credentials
- [ ] Login with invalid credentials
- [ ] Session timeout handling
- [ ] Logout functionality

### **Authorization**
- [ ] Access to own data only
- [ ] Denied access to other users' data
- [ ] Admin vs user permissions
- [ ] Guest user restrictions

### **Data Protection**
- [ ] Input validation
- [ ] XSS prevention
- [ ] CSRF protection
- [ ] SQL injection prevention

## 📱 **Responsive Testing Checklist**

### **Mobile (320px - 768px)**
- [ ] Navigation works correctly
- [ ] Forms are usable
- [ ] Touch targets are appropriate
- [ ] Text is readable

### **Tablet (768px - 1024px)**
- [ ] Layout adapts correctly
- [ ] Navigation is accessible
- [ ] Content is well-organized

### **Desktop (1024px+)**
- [ ] Full layout is displayed
- [ ] All features are accessible
- [ ] Performance is optimal

## 🎯 **Best Practices**

### **Test Writing**
1. **Arrange-Act-Assert** pattern
2. **Descriptive test names**
3. **Single responsibility per test**
4. **Mock external dependencies**

### **Test Organization**
1. **Group related tests**
2. **Use describe blocks**
3. **Clean up after tests**
4. **Use proper setup/teardown**

### **Performance**
1. **Run tests in parallel**
2. **Use efficient selectors**
3. **Mock heavy operations**
4. **Optimize test data**

## 📚 **Resources**

### **Documentation**
- [Jest Documentation](https://jestjs.io/docs/getting-started)
- [React Testing Library](https://testing-library.com/docs/react-testing-library/intro)
- [Cypress Documentation](https://docs.cypress.io/)
- [Firebase Testing](https://firebase.google.com/docs/rules/unit-tests)

### **Tools**
- **Jest**: Unit testing framework
- **React Testing Library**: Component testing
- **Cypress**: E2E testing
- **Firebase Rules Unit Testing**: Security testing
- **Chrome DevTools**: Performance testing

### **CI/CD Integration**
- **GitHub Actions**: Automated testing
- **Coverage Reports**: Code coverage tracking
- **Test Results**: Automated reporting
- **Performance Monitoring**: Continuous monitoring
