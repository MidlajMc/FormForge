# FormForge

A dynamic JSON-driven form rendering framework built with SwiftUI.

FormForge generates complete forms from JSON configuration, eliminating the need to hardcode screens while providing validation, state management, accessibility support, theming, and automated testing.

The framework is designed to be reusable, extensible, and easy to integrate into SwiftUI applications.

---

## 📖 Medium Article

I published a detailed technical walkthrough covering the architecture, design decisions, implementation details, testing strategy, and lessons learned while building FormForge.

Read the full article:

🔗 https://medium.com/@midhlag55/building-formforge-a-dynamic-json-driven-form-engine-in-swiftui-debf72af1d31

Topics covered:

- JSON-driven UI architecture
- Dynamic form rendering
- MVVM implementation
- Validation engine design
- Accessibility support
- Testing strategy
- Product decisions
- Engineering challenges
- Future improvements

---

## ✨ Features

- Dynamic form generation from JSON
- SwiftUI-first architecture
- MVVM-based design
- Runtime validation engine
- Configurable theming
- Accessibility support
- Automated unit testing
- Automated UI testing
- Reusable field components
- Error handling and validation feedback
- Easy extensibility for new field types

---

## 🚀 Why FormForge?

Forms are one of the most common UI patterns in modern applications:

- User Registration
- Onboarding
- Marketing Campaign Creation
- Surveys
- Checkout Flows
- Administrative Dashboards
- Internal Business Tools

Traditional form development often requires:

1. Creating new SwiftUI screens
2. Writing validation logic
3. Managing form state
4. Maintaining duplicate UI code

As applications grow, this approach becomes difficult to scale.

FormForge approaches the problem differently:

> What if forms could be described as data instead of code?

By using JSON as the source of truth, the framework can dynamically render complete user interfaces without creating new screens for every requirement.

---

## 🏗 Architecture

The project follows a clean MVVM architecture.

text FormScreen     ↓ FormViewModel     ↓ FormValidator     ↓ Form Models (decoded from JSON) 

### Benefits

- Clear separation of concerns
- Improved testability
- Easier maintenance
- Better scalability
- Reusable UI components

---

## ⚙️ Core Components

### Models

The form structure is completely driven by JSON configuration.

Core models include:

- Form
- Section
- Field
- ValidationRule
- Theme

Example:

json {   "form_title": "Campaign Setup",   "fields": [     {       "id": "campaign_name",       "type": "TEXT",       "label": "Campaign Name",       "required": true     }   ] } 

The JSON acts as the single source of truth for the UI.

---

### View Layer

Each field type is rendered using dedicated SwiftUI components.

Examples:

- DynamicTextField
- DynamicCheckbox
- DynamicToggle
- DynamicDropdown
- DynamicButton

Benefits:

- Encapsulated rendering logic
- Easier maintenance
- Simplified extensibility
- Reusable components

---

### ViewModel

The FormViewModel acts as the single source of truth.

Responsibilities include:

- Managing field values
- Tracking validation state
- Managing error messages
- Handling submissions
- Synchronizing UI state

Example:

swift @Published var textValues: [String: String] = [:] @Published var toggleValues: [String: Bool] = [:] @Published var dropdownValues: [String: String] = [:] @Published var validationErrors: [String: String] = [:] 

---

### Validation Layer

Validation logic is centralized within a dedicated validator.

Supported validations:

- Required fields
- Minimum length
- Maximum length
- URL validation
- Numeric validation
- Custom rules

Benefits:

- Cleaner views
- Independent testing
- Reusable validation logic
- Better maintainability

---

## 🎨 Dynamic Theming

FormForge supports runtime theming through configuration.

Example:

json {   "background_color": "#121212",   "text_color": "#E0E0E0",   "border_color": "#333333",   "error_color": "#CF6679" } 

Benefits:

- White-label applications
- Brand customization
- Dark mode support
- Client-specific styling

---

## 📝 Supported Field Types

### Text Input

Supports:

- Plain text
- Numeric input
- URL input
- Secure text entry

Use cases:

- Name
- Email
- Budget
- Website URL
- Password

---

### Dropdown

Supports:

- Single selection
- Multi-selection (future enhancement)

Use cases:

- Country selection
- User roles
- Categories
- Campaign types

---

### Toggle

Boolean controls.

Use cases:

- Notifications
- Feature flags
- Marketing preferences
- AI optimization

---

### Checkbox

Agreement and consent handling.

Use cases:

- Terms & Conditions
- Privacy Policy
- Legal Agreements

---

### Color Picker

Supports:

- Branding customization
- Theme selection
- Product personalization

---

## 🧠 Product Decisions

Several behaviors were intentionally designed even though they were not explicitly specified.

### 1. Validation Runs on Submit First

Validation is triggered when:

- User taps Save
- User edits a field that previously failed validation

Why?

Showing errors immediately on screen load creates a poor user experience.

Users should receive validation feedback only after interaction.

---

### 2. Unsupported Fields Fail Gracefully

If JSON contains an unknown field type:

- The form continues rendering
- Unsupported fields are skipped
- Debug logs are generated

Why?

JSON-driven systems often evolve independently of client releases.

Graceful failure improves resilience.

---

### 3. Optional Fields Remain Optional

Example:

Website URL

Behavior:

- Empty → Valid
- Invalid URL entered → Invalid

Why?

Users should not be forced to enter optional information, but any provided information should still be validated.

---

## ♿ Accessibility

Accessibility was treated as a first-class feature.

Accessibility identifiers include:

text campaign_name daily_budget destination_url accept_legal save_button 

Benefits:

- Better VoiceOver support
- Reliable UI automation
- Easier test maintenance
- Improved usability

---

## 🧪 Testing Strategy

### Unit Tests

Focused on:

- Validation logic
- State management
- Binding behavior
- Error clearing

Examples:

- Required field validation
- Checkbox validation
- Text binding updates
- Error state removal

---

### UI Tests

Focused on:

- Form rendering
- User interactions
- Validation alerts
- Successful submissions

Examples:

- Empty form submission
- Required checkbox validation
- Successful form completion
- Toggle interactions

---

## ⚡ Challenges & Solutions

### SwiftUI Alert Testing

Challenge:

UI tests initially looked for:

swift app.alerts["Form Submitted"] 

However, the actual implementation used:

swift .alert(     "Form Status",     isPresented: ... ) 

Resolution:

Updated tests to validate against the actual alert title and content.

---

### ScrollView + UI Testing

Challenge:

The Save button occasionally became non-hittable because it was positioned below the visible area.

Resolution:

swift saveButton.scrollToElement() saveButton.tap() 

Combined with hittability checks before interaction.

---

### Dynamic State Synchronization

Challenge:

Validation errors could persist after values were corrected.

Resolution:

Errors are automatically cleared when valid input is entered, ensuring UI consistency.

---

## 📊 Real-World Use Cases

FormForge can be used for:

### Marketing Platforms

- Campaign Creation
- Ad Configuration
- Budget Management

### SaaS Applications

- Account Settings
- User Management
- Permission Configuration

### Survey Platforms

- Customer Feedback
- Employee Engagement
- Market Research

### E-Commerce

- Product Customization
- Checkout Flows
- Delivery Preferences

### Enterprise Tools

- Internal Forms
- Administrative Workflows
- Configuration Dashboards

---

## 🔮 Future Improvements

### Conditional Field Visibility

Example:

text Show Budget Field only if Campaign Type == Paid 

---

### Async Validation

Support:

- Username availability checks
- API-backed validation
- Server-side business rules

using Swift Concurrency (async/await).

---

### Analytics Hooks

Track:

- Field interactions
- Validation failures
- Form abandonment
- Submission success rates

---

### Form Persistence

Potential implementations:

- UserDefaults
- Core Data
- Cloud synchronization

Allow users to resume partially completed forms.

---

### Expanded Test Coverage

Additional testing for:

- Dropdown edge cases
- Dynamic section rendering
- Invalid JSON configurations
- Accessibility verification
- Snapshot testing

---

## 📂 Project Structure

text FormForge ├── Models ├── ViewModels ├── Views │   ├── Components │   └── Screens ├── Validators ├── Resources │   └── JSON ├── Tests │   ├── UnitTests │   └── UITests └── SupportingFiles 

---

## 🎯 Key Learnings

Building FormForge reinforced several important engineering principles:

- Configuration is often more scalable than hardcoded UI.
- Dynamic rendering reduces maintenance overhead.
- Centralized validation improves consistency.
- Accessibility should be considered from the beginning.
- Automated testing increases confidence and maintainability.
- SwiftUI provides an excellent foundation for reusable UI systems.

---

## Summary

FormForge is a reusable, JSON-driven form framework built with SwiftUI that prioritizes:

- Flexibility
- Testability
- Accessibility
- Maintainability
- Scalability

By separating rendering, validation, and state management into independent layers, the framework remains easy to extend while maintaining a clean and modern SwiftUI architecture.

If you found this project interesting, check out the detailed Medium article for a deeper dive into the architecture, implementation decisions, and lessons learned.

📖 Medium Article:
https://medium.com/@midhlag55/building-formforge-a-dynamic-json-driven-form-engine-in-swiftui-debf72af1d31
