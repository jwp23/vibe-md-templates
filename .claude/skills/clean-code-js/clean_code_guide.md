# Clean Code Principles for JavaScript Development

**Essential Reference Guide for AI Code Generation**  
*Based on Robert C. Martin's "Clean Code: A Handbook of Agile Software Craftsmanship"*

---

## Executive Summary

This document contains the **TOP 25 MOST CRITICAL** Clean Code principles for JavaScript development, extracted and translated from Uncle Bob's seminal work. These principles represent the highest-impact practices that will immediately improve code quality, maintainability, and professionalism.

**For Claude Code:** These are mandatory coding standards. Follow these principles in order of priority: (1) Correctness, (2) Readability, (3) Performance. When in doubt, choose the more readable solution.

---

## The Golden Rules (Top 10)

1. **Use intention-revealing names** - No cryptic abbreviations (except `i`, `j`, `k` in small loops)
2. **Keep functions small** - Target <20 lines, ideal 2-4 lines
3. **Do one thing per function** - Single responsibility at single abstraction level
4. **Don't repeat yourself (DRY)** - Extract duplication immediately
5. **Prefer exceptions over error codes** - Separate error handling from happy path
6. **Never return null** - Use exceptions, empty arrays, or Null Object pattern
7. **Follow Single Responsibility Principle** - One reason to change per class
8. **Write self-documenting code** - Code should explain itself; comments are failures
9. **Use dependency injection** - Don't create dependencies; receive them
10. **Test-drive your design** - Let tests guide architecture evolution

---

## Table of Contents

### Section 1: Meaningful Names
1. [Use Intention-Revealing Names](#1-use-intention-revealing-names)
2. [Avoid Disinformation](#2-avoid-disinformation)
3. [Use Searchable Names](#3-use-searchable-names)
4. [Avoid Encodings](#4-avoid-encodings)
5. [Use Solution Domain Names](#5-use-solution-domain-names)

### Section 2: Functions
6. [Keep Functions Small](#6-keep-functions-small)
7. [Do One Thing](#7-do-one-thing)
8. [One Level of Abstraction Per Function](#8-one-level-of-abstraction-per-function)
9. [Use Descriptive Names](#9-use-descriptive-names)
10. [Minimize Function Arguments](#10-minimize-function-arguments)
11. [Avoid Flag Arguments](#11-avoid-flag-arguments)
12. [Have No Side Effects](#12-have-no-side-effects)
13. [Prefer Exceptions to Error Codes](#13-prefer-exceptions-to-error-codes)
14. [Don't Repeat Yourself (DRY)](#14-dont-repeat-yourself-dry)

### Section 3: Comments
15. [Explain Yourself in Code, Not Comments](#15-explain-yourself-in-code-not-comments)
16. [Never Leave Commented-Out Code](#16-never-leave-commented-out-code)

### Section 4: Error Handling
17. [Use Try-Catch-Finally First](#17-use-try-catch-finally-first)
18. [Provide Context with Exceptions](#18-provide-context-with-exceptions)
19. [Don't Return Null](#19-dont-return-null)
20. [Don't Pass Null](#20-dont-pass-null)

### Section 5: Classes & Objects
21. [Follow Single Responsibility Principle](#21-follow-single-responsibility-principle)
22. [Hide Internal Structure](#22-hide-internal-structure)
23. [Follow the Law of Demeter](#23-follow-the-law-of-demeter)

### Section 6: System Design
24. [Use Dependency Injection](#24-use-dependency-injection)
25. [Organize for Change](#25-organize-for-change)

---

# Section 1: Meaningful Names

## 1. Use Intention-Revealing Names

**Guideline:**  
Choose names that clearly reveal intent, purpose, and usage. A name should answer: why it exists, what it does, and how it's used. If a name requires a comment, it's a bad name.

**Bad Example:**
```javascript
const d = 18; // elapsed time in days
let list = [];

function getThem() {
  const list1 = [];
  for (let x of theList) {
    if (x[0] === 4) {
      list1.push(x);
    }
  }
  return list1;
}
```

**Good Example:**
```javascript
const elapsedTimeInDays = 18;
let activeCustomers = [];

function getFlaggedCells() {
  const flaggedCells = [];
  for (let cell of gameBoard) {
    if (cell.isFlagged()) {
      flaggedCells.push(cell);
    }
  }
  return flaggedCells;
}
```

**Key Rules:**
- Avoid single-letter names except `i`, `j`, `k` in small loops
- If a name needs a comment, choose a better name
- Use specific names: `customerList` not `list`, `userEmail` not `data`
- Names should reveal intent without surrounding context
- Spend time on names—they're read far more than written

---

## 2. Avoid Disinformation

**Guideline:**  
Never use names that convey incorrect information. Don't use words with established meanings (like `List`, `Map`) unless the variable is actually that type. Avoid names that vary in small, hard-to-notice ways.

**Bad Example:**
```javascript
// accountList is not actually a List data structure
let accountList = [];

// hp could mean: horsepower, hewlett-packard, hit points, homepage
const hp = calculateValue();

// These are nearly identical and easily confused
class XYZControllerForEfficientHandlingOfStrings {}
class XYZControllerForEfficientStorageOfStrings {}

// o and O look like 0, l looks like 1
const o = 0;
const l = 1;
```

**Good Example:**
```javascript
// Use plural or generic term if not actually a List
let accounts = [];
let accountGroup = [];

// Be explicit and specific
const hitPoints = calculateCharacterHealth();
const horsepower = calculateEnginePerformance();

// Make distinctions clear and meaningful
class StringHandler {}
class StringRepository {}

// Use clear, unambiguous names
const itemCount = 0;
const lineNumber = 1;
```

**Key Rules:**
- Don't use type names unless variable is that type
- Avoid names that vary in tiny, hard-to-notice ways
- Don't use characters that look similar (O/0, l/1, S/5)
- Each name should be clearly distinguishable at a glance
- Don't use abbreviations with multiple possible meanings

---

## 3. Use Searchable Names

**Guideline:**  
Single-letter names and numeric constants are nearly impossible to find with search tools. Use names you can grep for. The length of a name should correspond to the size of its scope.

**Bad Example:**
```javascript
// Unsearchable magic numbers
for (let j = 0; j < 34; j++) {
  s += (t[j] * 4) / 5;
}

// Single letter is impossible to search for
const e = 7;
if (result > e) {
  calculate();
}
```

**Good Example:**
```javascript
const NUMBER_OF_TASKS = 34;
const WORK_DAYS_PER_WEEK = 5;
const TASK_ESTIMATE_MULTIPLIER = 4;

let totalEstimate = 0;
for (let taskIndex = 0; taskIndex < NUMBER_OF_TASKS; taskIndex++) {
  const estimate = taskEstimates[taskIndex];
  totalEstimate += (estimate * TASK_ESTIMATE_MULTIPLIER) / WORK_DAYS_PER_WEEK;
}

const MAX_CLASSES_PER_STUDENT = 7;
if (enrolledClassCount > MAX_CLASSES_PER_STUDENT) {
  recalculateSchedule();
}
```

**Key Rules:**
- Extract magic numbers to named constants (UPPER_SNAKE_CASE)
- Single letters OK only for loop counters in small scopes
- If you'll grep for it, make it searchable
- Longer names for larger scopes, shorter for smaller scopes
- Every magic number should have a descriptive name

---

## 4. Avoid Encodings

**Guideline:**  
Don't encode type or scope information in names. Modern IDEs provide type information. Avoid Hungarian notation, member prefixes (`m_`, `_`), or interface prefixes (`I`).

**Bad Example:**
```javascript
// Hungarian notation (outdated)
let strName = "John";
let iCount = 0;
let bIsActive = true;

// Member prefixes (unnecessary)
class Customer {
  constructor() {
    this.m_name = "";
    this._address = "";
    this.__privateId = 0;
  }
}

// Interface prefix (avoid)
class IShapeFactory {}
class ShapeFactoryImpl {}
```

**Good Example:**
```javascript
// Clean, unencoded names
let name = "John";
let count = 0;
let isActive = true;

// Use language features for privacy
class Customer {
  constructor() {
    this.name = "";
    this.address = "";
  }
  
  #privateId = 0; // Use # for truly private fields
}

// Natural interface and implementation names
class ShapeFactory {} // Interface/abstract
class CircleFactory {} // Concrete implementation
```

**Key Rules:**
- No Hungarian notation—types are evident from context/IDE
- No `m_` or `_` prefixes for members
- No `I` prefix for interfaces
- Use JavaScript's `#` for private fields instead of naming conventions
- Trust your IDE and type system

---

## 5. Use Solution Domain Names

**Guideline:**  
Use computer science terms, algorithm names, pattern names, and technical terms freely. Your code is read by programmers—technical terminology is appropriate and expected.

**Bad Example:**
```javascript
// Avoiding technical terms makes code less clear
class CustomerContainer {
  add(customer) {
    this.items.push(customer);
  }
  
  getNext() {
    return this.items.shift();
  }
}

// Missing pattern name
class ShapeCreator {
  makeShape(type) { /* Factory pattern */ }
}
```

**Good Example:**
```javascript
// Use proper CS terminology
class CustomerQueue {
  enqueue(customer) {
    this.items.push(customer);
  }
  
  dequeue() {
    return this.items.shift();
  }
}

// Pattern name makes intent clear
class ShapeFactory {
  createShape(type) {
    // Factory pattern clearly identified
  }
}

// Algorithm names
function quickSort(array) { /* ... */ }
function binarySearch(array, target) { /* ... */ }

// Design pattern names
class DatabaseConnectionPool { /* Object Pool pattern */ }
class UserObserver { /* Observer pattern */ }
```

**Key Rules:**
- Use CS terms: `Queue`, `Stack`, `Tree`, `Graph`, `LinkedList`
- Include pattern names: `Factory`, `Builder`, `Observer`, `Strategy`
- Use algorithm names: `QuickSort`, `BinarySearch`, `DijkstraPath`
- Technical terms: `Repository`, `Controller`, `Service`, `Validator`
- Programmers read your code—programming terms are appropriate

---

# Section 2: Functions

## 6. Keep Functions Small

**Guideline:**  
Functions should be small—preferably under 20 lines, ideally 2-4 lines. Small functions are easier to name, understand, test, and maintain. If a function is large, it's doing too much.

**Bad Example:**
```javascript
function processOrder(order) {
  // Validation (10 lines)
  if (!order.customerId) throw new Error("Customer ID required");
  if (!order.items || order.items.length === 0) throw new Error("Order must have items");
  
  // Calculate totals (15 lines)
  let subtotal = 0;
  for (let item of order.items) {
    subtotal += item.price * item.quantity;
  }
  let tax = subtotal * 0.08;
  let shipping = subtotal > 50 ? 0 : 5.99;
  let discount = 0;
  if (order.promoCode === "SAVE10") {
    discount = subtotal * 0.1;
  }
  let total = subtotal + tax + shipping - discount;
  
  // Save to database (5 lines)
  const orderRecord = {
    customerId: order.customerId,
    items: order.items,
    subtotal, tax, shipping, discount, total
  };
  database.save(orderRecord);
  
  // Send email (5 lines)
  emailService.send({
    to: order.customerEmail,
    subject: "Order Confirmation",
    body: `Your order total is $${total}`
  });
  
  return orderRecord;
}
```

**Good Example:**
```javascript
function processOrder(order) {
  validateOrder(order);
  const pricing = calculatePricing(order);
  const savedOrder = saveOrder(order, pricing);
  sendConfirmationEmail(order, pricing);
  return savedOrder;
}

function validateOrder(order) {
  if (!order.customerId) throw new Error("Customer ID required");
  if (!order.items?.length) throw new Error("Order must have items");
}

function calculatePricing(order) {
  const subtotal = calculateSubtotal(order.items);
  const tax = subtotal * 0.08;
  const shipping = subtotal > 50 ? 0 : 5.99;
  const discount = applyDiscount(order.promoCode, subtotal);
  return { subtotal, tax, shipping, discount, total: subtotal + tax + shipping - discount };
}

function calculateSubtotal(items) {
  return items.reduce((sum, item) => sum + (item.price * item.quantity), 0);
}

function applyDiscount(promoCode, subtotal) {
  return promoCode === "SAVE10" ? subtotal * 0.1 : 0;
}

function saveOrder(order, pricing) {
  return database.save({ ...order, ...pricing });
}

function sendConfirmationEmail(order, pricing) {
  emailService.send({
    to: order.customerEmail,
    subject: "Order Confirmation",
    body: `Your order total is $${pricing.total}`
  });
}
```

**Key Rules:**
- Target: functions rarely over 20 lines
- Ideal: 2-4 lines per function
- Indent level should not exceed 1-2 levels
- Blank lines separating sections = extract those sections
- Small functions enable descriptive names
- Small functions are easy to test and reuse

---

## 7. Do One Thing

**Guideline:**  
Functions should do one thing, do it well, and do it only. A function does "one thing" if you cannot meaningfully extract another function from it with a name that isn't just a restatement of its implementation.

**Bad Example:**
```javascript
function emailClients(clients) {
  for (let client of clients) {
    // Doing multiple things: filtering, retrieving, and emailing
    if (client.isActive) {
      const clientRecord = database.lookup(client.id);
      if (clientRecord?.email) {
        const message = `Dear ${clientRecord.name}, ...`;
        emailService.send(clientRecord.email, message);
      }
    }
  }
}

// Mixing different responsibilities
function pay() {
  for (let employee of employees) {
    if (employee.isExempt()) {
      employee.pay(employee.monthlyRate);
    } else {
      employee.pay(employee.hourlyRate * employee.hoursWorked);
    }
  }
}
```

**Good Example:**
```javascript
function emailClients(clients) {
  const activeClients = getActiveClients(clients);
  activeClients.forEach(emailClient);
}

function getActiveClients(clients) {
  return clients.filter(client => client.isActive);
}

function emailClient(client) {
  const clientRecord = getClientRecord(client.id);
  if (clientRecord) {
    sendEmail(clientRecord);
  }
}

function getClientRecord(clientId) {
  const record = database.lookup(clientId);
  return record?.email ? record : null;
}

function sendEmail(clientRecord) {
  const message = createEmailMessage(clientRecord);
  emailService.send(clientRecord.email, message);
}

// Use polymorphism for different types
function pay() {
  employees.forEach(employee => employee.pay());
  // Each employee type knows how to pay itself
}
```

**Key Rules:**
- If you can extract a function with a meaningful name, do it
- If you can describe the function using "and", it does too much
- Functions should have one reason to change
- Sections separated by blank lines often indicate multiple responsibilities
- All statements should be at the same level of abstraction

---

## 8. One Level of Abstraction Per Function

**Guideline:**  
All code within a function should be at the same level of abstraction. Don't mix high-level concepts with low-level details. Functions should read top-to-bottom, each calling functions at the next level down.

**Bad Example:**
```javascript
function renderPage(pageData) {
  // High-level
  const html = "<html><body>";
  
  // Low-level string manipulation
  html += "<h1>" + pageData.title + "</h1>";
  
  // Medium-level
  const formattedContent = formatContent(pageData.content);
  
  // Low-level
  html += "<div>" + formattedContent + "</div>";
  html += renderFooter();
  html += "</body></html>";
  return html;
}
```

**Good Example:**
```javascript
// All statements at high level
function renderPage(pageData) {
  return [
    createHtmlOpen(),
    createHeader(pageData.title),
    createContent(pageData.content),
    createFooter(),
    createHtmlClose()
  ].join("");
}

// Each helper at consistent level
function createHtmlOpen() {
  return "<html><body>";
}

function createHeader(title) {
  return `<h1>${escapeHtml(title)}</h1>`;
}

function createContent(content) {
  return `<div>${formatContent(content)}</div>`;
}

function createFooter() {
  return renderFooter();
}

function createHtmlClose() {
  return "</body></html>";
}
```

**Key Rules:**
- High-level operations shouldn't mix with low-level details
- Each function calls functions at next abstraction level down
- Read top-to-bottom: high-level → detailed
- Consistent abstraction makes code scannable
- Extract details into separate functions

---

## 9. Use Descriptive Names

**Guideline:**  
Choose long, descriptive function names over short, enigmatic ones. A long descriptive name is better than a short enigmatic one or a long descriptive comment. Function names should describe everything the function does.

**Bad Example:**
```javascript
function process(data) { /* What does this process? */ }
function calc() { /* Calculate what? */ }
function handle(input) { /* Handle how? */ }
function go() { /* Go where? */ }
```

**Good Example:**
```javascript
function calculateMonthlyInterestPayment(principal, rate) {
  return (principal * rate) / 12;
}

function sendEmailToActiveCustomers(customers) {
  const activeCustomers = customers.filter(c => c.isActive);
  activeCustomers.forEach(sendPromotionalEmail);
}

function validateUserHasPermissionToDeletePost(user, post) {
  return user.id === post.authorId || user.isAdmin;
}

function formatDateAsISO8601String(date) {
  return date.toISOString();
}

function retrieveCustomerOrderHistoryFromDatabase(customerId) {
  return database.query('SELECT * FROM orders WHERE customer_id = ?', [customerId]);
}
```

**Key Rules:**
- Be descriptive—don't worry about length
- Use verb phrases: `sendEmail`, `calculateTotal`, `validateInput`
- Describe everything the function does
- If the function does more than the name says, fix the function or the name
- Spend time choosing names—it pays off
- Rename as you refactor and understand better

---

## 10. Minimize Function Arguments

**Guideline:**  
Zero arguments is ideal, one is good, two is acceptable, three should be avoided, and more than three requires special justification. Arguments increase complexity and make testing harder.

**Bad Example:**
```javascript
// Too many arguments
function createUser(firstName, lastName, email, password, age, country, city, zipCode, phone, newsletter) {
  // Implementation
}

// Multiple boolean flags
function renderPage(includeHeader, includeNav, includeFooter, includeAds) {
  // Confusing
}

// Output argument (modifies passed object)
function appendFooter(report) {
  report.footer = createFooter(); // Modifying argument
}
```

**Good Example:**
```javascript
// Zero or one argument ideal
function getCurrentUser() { /* no args */ }
function deleteCustomer(customerId) { /* one arg */ }

// Group related arguments into objects
function createUser(userDetails) {
  const { firstName, lastName, email, password, contact, preferences } = userDetails;
}

// Or use builder pattern
const user = new UserBuilder()
  .setName("John", "Doe")
  .setEmail("john@example.com")
  .setAddress("123 Main St", "Springfield", "12345")
  .build();

// Configuration object for multiple options
function renderPage(config) {
  const { includeHeader = true, includeNav = true, includeFooter = true } = config;
}

// Return new value instead of modifying
function withFooter(report) {
  return { ...report, footer: createFooter() };
}
```

**Key Rules:**
- Aim for 0-1 arguments when possible
- 2 arguments acceptable with good reason
- 3+ arguments needs strong justification
- Group related arguments into objects
- Use builder pattern for many initialization params
- Avoid output arguments—return new values
- Make parameters optional with defaults

---

## 11. Avoid Flag Arguments

**Guideline:**  
Never pass boolean flags to functions. Flags proclaim that the function does more than one thing—one thing if the flag is true, another if false. Split into separate functions instead.

**Bad Example:**
```javascript
// Flag indicates function does two things
function save(data, shouldValidate) {
  if (shouldValidate) {
    validate(data);
  }
  database.write(data);
}

// Unclear what true/false means
save(userData, true);
save(systemData, false);

// Multiple flags = exponential complexity
function render(showHeader, showFooter, showAds) {
  if (showHeader) renderHeader();
  renderBody();
  if (showFooter) renderFooter();
  if (showAds) renderAds();
}
```

**Good Example:**
```javascript
// Split into two clear functions
function saveAndValidate(data) {
  validate(data);
  database.write(data);
}

function save(data) {
  database.write(data);
}

// Clear intent
saveAndValidate(userData);
save(systemData);

// Better: composition
function renderPage() {
  return [
    renderHeader(),
    renderBody(),
    renderFooter()
  ].join("");
}

// If configuration truly needed, use named properties
function renderPage(config = {}) {
  if (config.header) renderHeader();
  renderBody();
  if (config.footer) renderFooter();
}
```

**Key Rules:**
- Boolean flags are code smells
- Split functions that take boolean flags
- Function names should reveal what they do
- Call sites should be clear without checking flag values
- Use configuration objects if truly needed
- Never use flags to control behavior

---

## 12. Have No Side Effects

**Guideline:**  
Functions should not have hidden side effects. Side effects are changes beyond the return value: modifying globals, changing passed objects, writing files. Side effects create temporal coupling and make code fragile.

**Bad Example:**
```javascript
// Hidden side effect: modifies session
function checkPassword(userName, password) {
  const user = database.findByName(userName);
  if (user && user.password === password) {
    Session.initialize(); // SIDE EFFECT!
    return true;
  }
  return false;
}

// Side effect: modifies global
let globalCounter = 0;
function processItem(item) {
  globalCounter++; // SIDE EFFECT!
  return item.value * 2;
}

// Side effect: modifies argument
function calculateTotal(order) {
  const total = order.items.reduce((sum, i) => sum + i.price, 0);
  order.total = total; // SIDE EFFECT!
  return total;
}
```

**Good Example:**
```javascript
// Pure function: no side effects
function checkPassword(userName, password) {
  const user = database.findByName(userName);
  return user && user.password === password;
}

// Separate function for side effects
function login(userName, password) {
  if (checkPassword(userName, password)) {
    Session.initialize();
    return true;
  }
  return false;
}

// Return new state instead
function processItem(item, counter) {
  return {
    result: item.value * 2,
    newCounter: counter + 1
  };
}

// Return new object
function calculateTotal(order) {
  const total = order.items.reduce((sum, i) => sum + i.price, 0);
  return { ...order, total };
}

// Or make side effect explicit in name
function calculateAndSetTotal(order) {
  const total = order.items.reduce((sum, i) => sum + i.price, 0);
  order.total = total;
  return total;
}
```

**Key Rules:**
- Functions should only do what their names say
- Don't modify global state
- Don't modify objects passed as arguments (unless name says so)
- Return new objects instead of modifying existing ones
- Make side effects explicit in function names
- Prefer pure functions: same input → same output
- Separate commands (change state) from queries (return data)

---

## 13. Prefer Exceptions to Error Codes

**Guideline:**  
Use exceptions instead of returning error codes. Error codes clutter logic with immediate checks and lead to nested structures. Exceptions separate the happy path from error handling.

**Bad Example:**
```javascript
// Error codes clutter logic
function deleteUser(userId) {
  const user = findUser(userId);
  if (user === null) return ERROR_USER_NOT_FOUND;
  
  const deleteStatus = database.delete(userId);
  if (deleteStatus !== SUCCESS) return ERROR_DATABASE;
  
  const cacheStatus = cache.invalidate(userId);
  if (cacheStatus !== SUCCESS) return ERROR_CACHE;
  
  return SUCCESS;
}

// Caller must check codes
const result = deleteUser(123);
if (result === ERROR_USER_NOT_FOUND) {
  displayError("User not found");
} else if (result === ERROR_DATABASE) {
  displayError("Database error");
}
```

**Good Example:**
```javascript
// Exceptions separate happy path from errors
function deleteUser(userId) {
  const user = findUser(userId); // Throws UserNotFoundError
  database.delete(userId); // Throws DatabaseError
  cache.invalidate(userId); // Throws CacheError
}

// Clean caller with separate error handling
try {
  deleteUser(123);
  displaySuccess("User deleted");
} catch (error) {
  if (error instanceof UserNotFoundError) {
    displayError("User not found");
  } else if (error instanceof DatabaseError) {
    displayError("Database error");
    logCriticalError(error);
  } else if (error instanceof CacheError) {
    logWarning(error); // Less critical
  }
}
```

**Key Rules:**
- Throw exceptions for errors
- Use try-catch at appropriate level
- Keep happy path clean
- Create specific exception types
- Let exceptions propagate to proper handler
- Reserve exceptions for exceptional conditions
- Don't force immediate error checking

---

## 14. Don't Repeat Yourself (DRY)

**Guideline:**  
Duplication is the root of all evil in software. When you see duplicated code, extract it immediately. Duplication multiplies places you must change, increases bugs, and makes code unmaintainable.

**Bad Example:**
```javascript
// Duplicated validation
function createUser(userData) {
  if (!userData.name || userData.name.length < 2) {
    throw new Error("Name must be at least 2 characters");
  }
  if (!userData.email || !userData.email.includes("@")) {
    throw new Error("Invalid email");
  }
  // Create user
}

function updateUser(userId, userData) {
  if (!userData.name || userData.name.length < 2) {
    throw new Error("Name must be at least 2 characters");
  }
  if (!userData.email || !userData.email.includes("@")) {
    throw new Error("Invalid email");
  }
  // Update user
}

// Duplicated calculation
function calculateOrderTotal(order) {
  let total = 0;
  for (let item of order.items) {
    total += item.price * item.quantity;
  }
  return total + (total * 0.08);
}

function generateInvoice(order) {
  let total = 0;
  for (let item of order.items) {
    total += item.price * item.quantity;
  }
  return total + (total * 0.08);
}
```

**Good Example:**
```javascript
// Extract shared validation
function validateUserData(userData) {
  if (!userData.name || userData.name.length < 2) {
    throw new Error("Name must be at least 2 characters");
  }
  if (!userData.email || !userData.email.includes("@")) {
    throw new Error("Invalid email");
  }
}

function createUser(userData) {
  validateUserData(userData);
  // Create user
}

function updateUser(userId, userData) {
  validateUserData(userData);
  // Update user
}

// Extract shared calculation
function calculateSubtotal(items) {
  return items.reduce((sum, item) => sum + (item.price * item.quantity), 0);
}

function calculateTotal(items) {
  const subtotal = calculateSubtotal(items);
  return subtotal + (subtotal * 0.08);
}

function calculateOrderTotal(order) {
  return calculateTotal(order.items);
}

function generateInvoice(order) {
  const total = calculateTotal(order.items);
  // Generate invoice with total
}
```

**Key Rules:**
- Extract duplicated code into functions immediately
- Look for similar (not just identical) code
- Use parameters to handle variations
- Rule of Three: three duplications → refactor
- Never copy-paste code
- Watch for duplicated algorithms, validations, calculations
- Use inheritance, composition, or patterns to eliminate duplication

---

# Section 3: Comments

## 15. Explain Yourself in Code, Not Comments

**Guideline:**  
Comments are failures to express yourself in code. The best comment is a well-named function or variable. Focus on writing self-documenting code. Use comments only when code cannot be made clear enough.

**Bad Example:**
```javascript
// Check to see if employee is eligible for full benefits
if ((employee.flags & HOURLY_FLAG) && (employee.age > 65)) {
  // Process
}

let x = 0; // counter for loop iterations
// Loop through all employees
for (let i = 0; i < employees.length; i++) {
  const e = employees[i]; // Get current employee
  const s = e.annualSalary / 12; // Calculate monthly salary
  x += s; // Add to total
}
```

**Good Example:**
```javascript
// No comment needed—code explains itself
if (employee.isEligibleForFullBenefits()) {
  processBenefits(employee);
}

let totalMonthlySalaries = 0;
for (let employee of employees) {
  const monthlySalary = employee.calculateMonthlySalary();
  totalMonthlySalaries += monthlySalary;
}

// Even better with reduce
const totalMonthlySalaries = employees.reduce(
  (total, employee) => total + employee.calculateMonthlySalary(),
  0
);
```

**Key Rules:**
- If you need a comment to explain, refactor the code instead
- Use meaningful names instead of comments
- Extract complex expressions into well-named functions
- Comments lie—code doesn't
- Every comment represents a failure
- Good code is self-documenting

**Good Comments (when necessary):**
- Legal comments (copyright, license)
- Explanation of WHY, not WHAT
- Warning of consequences
- TODO notes (but track them properly)
- Clarification of obscure code you can't change

---

## 16. Never Leave Commented-Out Code

**Guideline:**  
Delete commented-out code immediately. Don't leave dead code "just in case." Version control remembers. Commented code quickly becomes obsolete, misleading, and creates clutter.

**Bad Example:**
```javascript
function processOrder(order) {
  validateOrder(order);
  
  // const discount = calculateDiscount(order);
  // const tax = calculateTax(order);
  
  const total = calculateTotal(order);
  
  // order.status = "pending";
  // sendEmail(order.customerEmail);
  
  saveOrder(order);
  
  // Alternative implementation below
  // function calculateTotal() {
  //   let sum = 0;
  //   for (let item of order.items) {
  //     sum += item.price;
  //   }
  //   return sum;
  // }
  
  return total;
}
```

**Good Example:**
```javascript
function processOrder(order) {
  validateOrder(order);
  const total = calculateTotal(order);
  saveOrder(order);
  return total;
}

// If historical context needed:
// See commit a3f5b2c for previous implementation
```

**Key Rules:**
- Delete commented code—don't commit it
- Trust version control for history
- Commented code creates confusion
- If needed later, find it in git history
- Commented code isn't maintained and rots
- Never check in commented code

---

# Section 4: Error Handling

## 17. Use Try-Catch-Finally First

**Guideline:**  
When writing code that might throw exceptions, start with try-catch-finally. This defines what users should expect regardless of errors. Try blocks are like transactions—catch should leave the system consistent.

**Bad Example:**
```javascript
// Writing logic first, error handling as afterthought
function loadConfiguration(filename) {
  const file = fs.openSync(filename);
  const data = fs.readFileSync(file);
  const config = JSON.parse(data);
  fs.closeSync(file);
  return config;
  // File won't close on error!
}
```

**Good Example:**
```javascript
// Start with try-catch-finally
function loadConfiguration(filename) {
  let file;
  try {
    file = fs.openSync(filename);
    const data = fs.readFileSync(file);
    return JSON.parse(data);
  } catch (error) {
    throw new ConfigurationError(
      `Failed to load config from ${filename}`,
      error
    );
  } finally {
    if (file) {
      fs.closeSync(file); // Always cleanup
    }
  }
}

// Test error cases
test('should throw ConfigurationError for invalid JSON', () => {
  expect(() => loadConfiguration('invalid.json'))
    .toThrow(ConfigurationError);
});
```

**Key Rules:**
- Start with try-catch-finally structure
- Define expected behavior on error upfront
- Use finally for cleanup that must always happen
- Try-catch-finally defines transaction boundaries
- Write tests for error cases
- Catch should leave system consistent
- Don't let resources leak on exceptions

---

## 18. Provide Context with Exceptions

**Guideline:**  
Each exception should provide enough context to determine source and location of error. Include the operation that failed and relevant identifiers. Stack traces alone are insufficient.

**Bad Example:**
```javascript
// Generic, uninformative errors
function processPayment(order) {
  if (!order.amount) {
    throw new Error("Invalid");
  }
  
  const result = paymentGateway.charge(order.amount);
  if (!result) {
    throw new Error("Failed");
  }
}

// Just rethrowing
try {
  processData(input);
} catch (error) {
  throw error; // Loses context
}
```

**Good Example:**
```javascript
// Specific, informative errors
function processPayment(order) {
  if (!order.amount || order.amount <= 0) {
    throw new PaymentError(
      `Invalid payment amount: ${order.amount} for order ${order.id}`,
      { orderId: order.id, amount: order.amount }
    );
  }
  
  try {
    const result = paymentGateway.charge(order.amount);
    if (!result.success) {
      throw new PaymentError(
        `Payment gateway failed for order ${order.id}: ${result.message}`,
        { orderId: order.id, amount: order.amount, gatewayCode: result.code }
      );
    }
    return result;
  } catch (error) {
    throw new PaymentError(
      `Failed to process payment for order ${order.id}`,
      { orderId: order.id, amount: order.amount, originalError: error }
    );
  }
}

// Custom error with context
class PaymentError extends Error {
  constructor(message, context = {}) {
    super(message);
    this.name = 'PaymentError';
    this.context = context;
    this.timestamp = new Date();
  }
}

// Add context when rethrowing
try {
  processData(input);
} catch (error) {
  throw new DataProcessingError(
    `Failed to process input data for batch ${batchId}`,
    { batchId, inputSize: input.length, originalError: error }
  );
}
```

**Key Rules:**
- Include operation that failed
- Include relevant IDs, names, values
- Create custom error classes
- Attach context objects for debugging
- Wrap lower-level exceptions with context
- Make error messages actionable
- Include info to reproduce the problem

---

## 19. Don't Return Null

**Guideline:**  
Returning null invites errors and clutters code with null checks. Instead: throw exceptions, return empty collections, use Null Object pattern, or use Optional/Maybe.

**Bad Example:**
```javascript
// Returning null forces checks everywhere
function getCustomer(id) {
  const customer = database.query(id);
  return customer || null;
}

// Easy to forget null check
const customer = getCustomer(123);
console.log(customer.name); // Crashes if null!

// Null in collections causes errors
function getActiveOrders() {
  const orders = database.queryOrders();
  return orders.length > 0 ? orders : null;
}

// Crashes if null
for (let order of getActiveOrders()) {
  processOrder(order);
}
```

**Good Example:**
```javascript
// Throw exception instead
function getCustomer(id) {
  const customer = database.query(id);
  if (!customer) {
    throw new CustomerNotFoundError(`Customer ${id} not found`);
  }
  return customer;
}

// Caller gets customer or exception—no null checks
try {
  const customer = getCustomer(123);
  console.log(customer.name); // Safe
} catch (error) {
  handleCustomerNotFound(error);
}

// Return empty collection, never null
function getActiveOrders() {
  return database.queryOrders() || [];
}

// Safe even if empty
for (let order of getActiveOrders()) {
  processOrder(order);
}

// Null Object pattern
class NullCustomer {
  get name() { return "Guest"; }
  get email() { return ""; }
  isNull() { return true; }
}

function getCustomer(id) {
  return database.query(id) || new NullCustomer();
}

// No null checks needed
const customer = getCustomer(123);
console.log(customer.name); // "Guest" if not found

// Optional/Maybe pattern
class Optional {
  constructor(value) {
    this._value = value;
  }
  
  static of(value) {
    return new Optional(value);
  }
  
  static empty() {
    return new Optional(null);
  }
  
  isPresent() {
    return this._value != null;
  }
  
  orElse(defaultValue) {
    return this.isPresent() ? this._value : defaultValue;
  }
}

function findCustomer(id) {
  const customer = database.query(id);
  return customer ? Optional.of(customer) : Optional.empty();
}

const name = findCustomer(123).orElse({ name: "Guest" }).name;
```

**Key Rules:**
- Never return null from collections—return empty arrays
- Throw exceptions for exceptional cases
- Use Null Object pattern for default behavior
- Use Optional/Maybe to make null explicit
- Minimize null checks in codebase
- Make absence of value explicit, not implicit

---

## 20. Don't Pass Null

**Guideline:**  
Avoid passing null as arguments. It forces defensive checks in every method. Design APIs that don't allow null arguments. Use default parameters or separate methods instead.

**Bad Example:**
```javascript
// Accepts null, forces defensive checks
function calculateArea(width, height) {
  if (width === null || height === null) {
    return 0; // Or throw? Unclear!
  }
  return width * height;
}

// Messy calls
calculateArea(10, 20);
calculateArea(null, 20); // What should happen?

// Function handles many null combinations
function registerUser(name, email, phone) {
  if (name === null) throw new Error("Name required");
  if (email !== null) validateEmail(email);
  if (phone !== null) validatePhone(phone);
}

registerUser("John", "john@example.com", null);
registerUser("Jane", null, "555-1234");
```

**Good Example:**
```javascript
// Don't allow null
function calculateArea(width, height) {
  if (typeof width !== 'number' || typeof height !== 'number') {
    throw new Error("Width and height must be numbers");
  }
  return width * height;
}

// Clear usage
calculateArea(10, 20);

// Use object parameters for optional values
function registerUser({ name, email = null, phone = null }) {
  if (!name) throw new Error("Name required");
  if (email) validateEmail(email);
  if (phone) validatePhone(phone);
  return createUser({ name, email, phone });
}

// Clear which are optional
registerUser({ name: "John", email: "john@example.com" });
registerUser({ name: "Jane", phone: "555-1234" });
registerUser({ name: "Bob" });

// Or separate methods
function registerUserWithEmail(name, email) {
  validateEmail(email);
  return createUser({ name, email, phone: null });
}

function registerUserBasic(name) {
  return createUser({ name, email: null, phone: null });
}

// Use default parameters
function sendNotification(message, recipient, priority = 'normal') {
  // priority never null, has default
}

// Assert at boundaries
function processOrder(order) {
  if (!order) throw new Error("Order cannot be null");
  if (!order.items) throw new Error("Order items cannot be null");
  // Rest assumes order is valid
}
```

**Key Rules:**
- Don't accept null unless absolutely necessary
- Use default parameters for optional values
- Use object parameters with defaults
- Validate/assert non-null at method entry
- Design APIs that don't require null
- Make required vs optional explicit
- Throw immediately if null passed where it shouldn't be

---

# Section 5: Classes & Objects

## 21. Follow Single Responsibility Principle

**Guideline:**  
A class should have one, and only one, reason to change. SRP states that a class should have only one responsibility. Multiple responsibilities create coupling—changes to one responsibility may impair others.

**Bad Example:**
```javascript
// Multiple responsibilities
class Employee {
  constructor(name, type) {
    this.name = name;
    this.type = type;
  }
  
  // Responsibility 1: Business rules
  calculatePay() {
    return this.type === 'hourly' ? this.hours * 50 : 4000;
  }
  
  // Responsibility 2: Persistence
  save() {
    database.execute(`INSERT INTO employees VALUES (?, ?)`, [this.name, this.type]);
  }
  
  // Responsibility 3: Reporting
  generateReport() {
    return `<html><body><h1>${this.name}</h1><p>Pay: ${this.calculatePay()}</p></body></html>`;
  }
}

// Changes to any area require changing this class
```

**Good Example:**
```javascript
// Each class has one responsibility

// Responsibility: Represent employee data
class Employee {
  constructor(name, type) {
    this.name = name;
    this.type = type;
  }
}

// Responsibility: Calculate pay
class PayCalculator {
  calculatePay(employee, hours) {
    return employee.type === 'hourly' 
      ? hours * this.getHourlyRate(employee)
      : this.getSalary(employee);
  }
  
  getHourlyRate(employee) { return 50; }
  getSalary(employee) { return 4000; }
}

// Responsibility: Persist data
class EmployeeRepository {
  save(employee) {
    database.execute(
      `INSERT INTO employees (name, type) VALUES (?, ?)`,
      [employee.name, employee.type]
    );
  }
  
  findById(id) {
    const row = database.query('SELECT * FROM employees WHERE id = ?', [id]);
    return row ? new Employee(row.name, row.type) : null;
  }
}

// Responsibility: Generate reports
class EmployeeReportGenerator {
  constructor(payCalculator) {
    this.payCalculator = payCalculator;
  }
  
  generateReport(employee, hours) {
    const pay = this.payCalculator.calculatePay(employee, hours);
    return `<html><body><h1>${employee.name}</h1><p>Pay: ${pay}</p></body></html>`;
  }
}

// Each class changes independently
// Pay rules? → PayCalculator
// Database? → EmployeeRepository  
// Report format? → EmployeeReportGenerator
```

**Key Rules:**
- One reason to change per class
- Group functions that change for same reason
- Separate functions that change for different reasons
- Ask: "What is this class's responsibility?"
- If you use "and", it has multiple responsibilities
- Different actors shouldn't cause same class to change

---

## 22. Hide Internal Structure

**Guideline:**  
Objects hide internal structure and expose operations. Don't expose data through getters/setters—this violates encapsulation. Expose methods that perform operations without revealing implementation.

**Bad Example:**
```javascript
// Exposing internal structure
class Rectangle {
  getWidth() { return this.width; }
  setWidth(width) { this.width = width; }
  getHeight() { return this.height; }
  setHeight(height) { this.height = height; }
}

// Client depends on internal structure
const rect = new Rectangle();
rect.setWidth(10);
rect.setHeight(5);
const area = rect.getWidth() * rect.getHeight();
```

**Good Example:**
```javascript
// Hiding internal structure
class Rectangle {
  #width = 0;
  #height = 0;
  
  constructor(width, height) {
    this.#width = width;
    this.#height = height;
  }
  
  // Expose operations, not data
  getArea() {
    return this.#width * this.#height;
  }
  
  getPerimeter() {
    return 2 * (this.#width + this.#height);
  }
  
  scale(factor) {
    this.#width *= factor;
    this.#height *= factor;
  }
  
  isSquare() {
    return this.#width === this.#height;
  }
}

// Client uses operations
const rect = new Rectangle(10, 5);
const area = rect.getArea(); // Don't need to know how
```

**Key Rules:**
- Hide data with private fields (`#` in JavaScript)
- Don't create automatic getters/setters
- Expose operations, not data accessors
- Think about what object should DO, not CONTAIN
- Clients tell objects what to do, not ask for data
- Implementation changes shouldn't affect clients

---

## 23. Follow the Law of Demeter

**Guideline:**  
A method should only call methods of: itself, its parameters, objects it creates, and its direct components. Don't call methods on objects returned by other methods. This reduces coupling.

**Bad Example:**
```javascript
// Violates Law of Demeter—multiple dots
class OrderProcessor {
  processOrder(order) {
    // Reaching through multiple objects
    const street = order.getCustomer().getAddress().getStreet();
    const city = order.getCustomer().getAddress().getCity();
    
    // More reaching
    const discount = order.getCustomer().getMembershipLevel().getDiscountRate();
    
    // Train wreck
    if (order.getItems().getFirstItem().getPrice() > 100) {
      // Apply discount
    }
  }
}

// Chain of calls
const outputDir = context.getOptions().getScratchDir().getAbsolutePath();
```

**Good Example:**
```javascript
// Follows Law of Demeter
class OrderProcessor {
  processOrder(order) {
    // Ask order for what we need
    const shippingAddress = order.getShippingAddress();
    const discount = order.getCustomerDiscount();
    
    // Even better: tell, don't ask
    if (order.qualifiesForFreeShipping()) {
      order.applyFreeShipping();
    }
    
    order.applyCustomerDiscount();
  }
}

// Order handles internal structure
class Order {
  getShippingAddress() {
    return this.customer.getShippingAddress();
  }
  
  getCustomerDiscount() {
    return this.customer.getApplicableDiscount();
  }
  
  qualifiesForFreeShipping() {
    return this.getTotal() > 50;
  }
  
  applyCustomerDiscount() {
    const discount = this.customer.getApplicableDiscount();
    this.total *= (1 - discount);
  }
}

// Single call instead of chain
const outputDir = context.getAbsolutePathToScratchDirectory();
```

**Key Rules:**
- Only call methods on: self, parameters, created objects, direct components
- Avoid chains: `a.getB().getC().doSomething()`
- "Tell, don't ask"—tell objects what to do
- Each dot represents knowledge of another's structure
- Ask immediate neighbors, not distant objects
- Reduce coupling by minimizing knowledge

---

# Section 6: System Design

## 24. Use Dependency Injection

**Guideline:**  
Use dependency injection to separate construction from use. Objects should receive dependencies from outside, not create them. This makes code testable, flexible, and follows Dependency Inversion Principle.

**Bad Example:**
```javascript
// Hard-coded dependencies
class UserService {
  constructor() {
    // Creates own dependencies
    this.database = new PostgreSQL('localhost', 5432);
    this.cache = new Redis('localhost', 6379);
    this.logger = new FileLogger('/var/log/app.log');
    this.emailer = new SendGridEmailer(process.env.SENDGRID_KEY);
  }
  
  createUser(userData) {
    this.logger.log('Creating user');
    const user = this.database.insert(userData);
    this.cache.set(`user:${user.id}`, user);
    this.emailer.send(user.email, 'Welcome!');
    return user;
  }
}

// Cannot test without real services
// Cannot switch implementations
const userService = new UserService();
```

**Good Example:**
```javascript
// Dependency injection
class UserService {
  constructor(database, cache, logger, emailer) {
    // Dependencies injected
    this.database = database;
    this.cache = cache;
    this.logger = logger;
    this.emailer = emailer;
  }
  
  createUser(userData) {
    this.logger.log('Creating user');
    const user = this.database.insert(userData);
    this.cache.set(`user:${user.id}`, user);
    this.emailer.send(user.email, 'Welcome!');
    return user;
  }
}

// Production config
const productionService = new UserService(
  new PostgreSQL('prod.db.com', 5432),
  new Redis('prod.cache.com', 6379),
  new CloudLogger(),
  new SendGridEmailer(process.env.SENDGRID_KEY)
);

// Test config
const testService = new UserService(
  new InMemoryDatabase(),
  new MockCache(),
  new ConsoleLogger(),
  new MockEmailer()
);

// DI Container for complex graphs
class Container {
  constructor() {
    this.services = new Map();
  }
  
  register(name, factory, isSingleton = false) {
    this.services.set(name, { factory, isSingleton, instance: null });
  }
  
  resolve(name) {
    const service = this.services.get(name);
    if (!service) throw new Error(`Service ${name} not found`);
    
    if (service.isSingleton) {
      if (!service.instance) {
        service.instance = service.factory(this);
      }
      return service.instance;
    }
    
    return service.factory(this);
  }
}

// Register services
const container = new Container();
container.register('database', () => new PostgreSQL(process.env.DB_HOST), true);
container.register('cache', () => new Redis(process.env.REDIS_HOST), true);
container.register('logger', () => new CloudLogger(), true);
container.register('emailer', () => new SendGridEmailer(process.env.SENDGRID_KEY), true);
container.register('userService', (c) => new UserService(
  c.resolve('database'),
  c.resolve('cache'),
  c.resolve('logger'),
  c.resolve('emailer')
));

// Resolve when needed
const userService = container.resolve('userService');
```

**Key Rules:**
- Inject dependencies through constructor
- Make dependencies explicit in signature
- Don't create dependencies inside classes
- Use interfaces/abstractions for dependencies
- Use DI containers for complex graphs
- Easy testing with mocks
- Easy to swap implementations
- Follow Inversion of Control

---

## 25. Organize for Change

**Guideline:**  
Design so changes are localized. Use abstract classes and interfaces to isolate concepts. Prefer dependency injection. Follow Open/Closed Principle: open for extension, closed for modification. Add new code instead of changing existing code.

**Bad Example:**
```javascript
// Rigid—changes require modifying existing code
class ReportGenerator {
  generateReport(data, type) {
    if (type === 'PDF') {
      // PDF logic
      const pdf = new PDFDocument();
      return pdf.save();
    } else if (type === 'HTML') {
      // HTML logic
      return '<html>...</html>';
    } else if (type === 'CSV') {
      // CSV logic
      return 'Title\n...';
    }
    // Adding new format requires modifying this class
  }
}
```

**Good Example:**
```javascript
// Flexible—organized for change

// Abstract interface
class ReportFormatter {
  format(data) {
    throw new Error("Must implement");
  }
}

// Concrete implementations
class PDFFormatter extends ReportFormatter {
  format(data) {
    const pdf = new PDFDocument();
    pdf.addPage();
    pdf.drawText(data.title);
    return pdf.save();
  }
}

class HTMLFormatter extends ReportFormatter {
  format(data) {
    return `<html><body><h1>${data.title}</h1></body></html>`;
  }
}

class CSVFormatter extends ReportFormatter {
  format(data) {
    return `Title\n${data.title}`;
  }
}

// Generator uses dependency injection
class ReportGenerator {
  constructor(formatter) {
    this.formatter = formatter;
  }
  
  generateReport(data) {
    return this.formatter.format(data);
  }
}

// Adding new format = new class, no changes to existing code
class XMLFormatter extends ReportFormatter {
  format(data) {
    return `<report><title>${data.title}</title></report>`;
  }
}

// Easy to switch
const pdfReport = new ReportGenerator(new PDFFormatter());
const htmlReport = new ReportGenerator(new HTMLFormatter());
const xmlReport = new ReportGenerator(new XMLFormatter());

// Factory for creation
class FormatterFactory {
  static create(type) {
    switch (type) {
      case 'PDF': return new PDFFormatter();
      case 'HTML': return new HTMLFormatter();
      case 'CSV': return new CSVFormatter();
      case 'XML': return new XMLFormatter();
      default: throw new Error(`Unknown type: ${type}`);
    }
  }
}
```

**Key Rules:**
- Open for extension, closed for modification
- Use abstract classes/interfaces for contracts
- Depend on abstractions, not concrete implementations
- Use dependency injection for flexibility
- Use strategy pattern for varying behavior
- Use factory pattern for creation
- New functionality = new code, not changes to existing code

---

## Conclusion

These 25 principles represent the most critical Clean Code practices. Following them will result in:

- **More readable code** that others (and future you) can understand
- **Easier maintenance** with changes localized and safe
- **Fewer bugs** through clear structure and good error handling
- **Better testability** with small, focused, injectable components
- **Professional quality** that stands the test of time

## Priority Levels

**MUST** (Always follow):
- Intention-revealing names
- Small functions (do one thing)
- No null returns
- DRY principle
- Single Responsibility

**SHOULD** (Follow unless specific reason not to):
- Searchable names
- Minimize arguments
- No flag arguments
- Exceptions over error codes
- Dependency injection

**CONSIDER** (Apply when appropriate):
- Law of Demeter
- Null Object pattern
- Try-catch-finally first
- Organize for change

---

**Remember:** Clean code is not about following rules dogmatically—it's about writing code that humans can read and maintain. When in doubt, choose the more readable option.

**For Claude Code:** These principles are your coding constitution. Apply them consistently, and you'll generate professional-grade JavaScript that developers will be grateful to work with.