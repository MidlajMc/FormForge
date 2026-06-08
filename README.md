FormForge

FormForge is a dynamic form-rendering framework built with SwiftUI that generates UI from a JSON configuration. The goal was to create a reusable, extensible form engine that supports validation, state management, accessibility, theming, and automated testing while remaining simple to integrate into any SwiftUI application.

⸻

Overall Approach & Architecture

Architecture

The project follows an MVVM-based architecture:

FormScreen
    ↓
FormViewModel
    ↓
FormValidator
    ↓
Form Models (decoded from JSON)

Core Components

Models

The form structure is driven entirely by JSON configuration.

Examples:

* Form
* Section
* Field
* ValidationRule
* Theme

These models define what should be rendered without hardcoding UI.

View Layer

Each field type is rendered using a dedicated SwiftUI component:

* DynamicTextField
* DynamicCheckbox
* DynamicToggle
* DynamicDropdown
* DynamicButton

This keeps rendering logic isolated and makes adding new field types straightforward.

ViewModel

FormViewModel acts as the single source of truth for:

* Field values
* Validation state
* Error messages
* Submission handling

Responsibilities include:

* Managing bindings
* Tracking form state
* Running validations
* Triggering submission events

Validation Layer

Validation logic is centralized in a dedicated validator rather than scattered throughout views.

Supported validations include:

* Required fields
* Minimum length
* Maximum length
* URL validation
* Numeric validation
* Custom rules

This separation makes validation easy to test independently.

⸻

Product Decisions

Several behaviors were not explicitly defined in the requirements, so I made the following decisions.

1. Validation Runs on Submit First, Then Live Updates

Decision

Fields are not immediately shown as invalid when the screen loads.

Validation is triggered when:

* User taps Save
* User edits a field that previously failed validation

Why

Showing errors before any interaction creates a poor user experience. Users should only see validation feedback once they attempt submission or interact with the field.

⸻

2. Missing or Unsupported Field Types Fail Gracefully

Decision

If the JSON contains an unknown field type, the form does not crash.

Instead:

* The field is skipped
* A debug log is generated

Why

JSON-driven systems often evolve independently of client releases. Failing gracefully makes the framework more resilient to configuration issues.

⸻

3. Empty Optional Fields Are Considered Valid

Decision

Optional fields bypass validation unless they contain a value.

Example:

Website URL

If optional:

* Empty → Valid
* Invalid URL entered → Invalid

Why

Users should not be forced to provide optional information, but any supplied data should still be validated.

⸻

Accessibility Considerations

Accessibility identifiers were added to support UI testing and assistive technologies.

Examples:

campaign_name
daily_budget
destination_url
accept_legal
save_button

This provides:

* Reliable UI automation
* Improved VoiceOver support
* Better maintainability for test suites

⸻

Testing Strategy

Unit Tests

Focused on:

* Validation logic
* State updates
* Binding behavior
* Error clearing

Examples:

* Required field validation
* Checkbox validation
* Text binding updates
* Error state removal after correction

UI Tests

Focused on:

* Form rendering
* User interaction flows
* Validation alerts
* Successful submissions

Examples:

* Empty form submission
* Required checkbox validation
* Valid form submission
* Toggle interaction

⸻

Challenges & How I Worked Through Them

SwiftUI Alert Testing

One challenge was testing SwiftUI alerts in UI tests.

Initially, the tests looked for:

app.alerts["Form Submitted"]

However, the implementation used:

.alert(
    "Form Status",
    isPresented: ...
)

which resulted in mismatched alert titles.

Resolution

I updated the tests to validate against the actual alert title and message presented by the application.

⸻

ScrollView + UI Testing

The Save button occasionally became non-hittable during automated tests because it was positioned below the visible area after interacting with dropdowns and text fields.

Resolution

I explicitly scrolled the view before tapping:

saveButton.scrollToElement()
saveButton.tap()

and used hittability checks before interaction.

⸻

Dynamic State Synchronization

Keeping validation state synchronized with dynamically generated fields introduced edge cases where errors could persist after values changed.

Resolution

Validation errors are cleared automatically whenever a field receives valid input, ensuring UI state remains consistent.

⸻

What I Would Improve With More Time

1. Conditional Field Visibility

Support rules such as:

Show Budget Field
only if
Campaign Type == Paid

This would enable more advanced form workflows.

⸻

2. Async Validation

Add support for:

* API-backed validation
* Username availability checks
* Server-side business rules

using Swift Concurrency (async/await).

⸻

3. Better Analytics Hooks

Provide callbacks for:

* Field interactions
* Validation failures
* Abandoned forms
* Submission success rates

to support product analytics.

⸻

4. Form Persistence

Allow users to resume partially completed forms.

Potential implementations:

* UserDefaults
* Core Data
* Cloud synchronization

⸻

5. Expanded Test Coverage

Additional tests for:

* Dropdown edge cases
* Dynamic section rendering
* Invalid JSON configurations
* Accessibility verification
* Snapshot testing

⸻

Summary

FormForge was designed as a reusable, JSON-driven form framework that prioritizes:

* Flexibility
* Testability
* Accessibility
* Maintainability

The architecture separates rendering, validation, and state management into independent layers, making it easy to extend with additional field types and validation rules while maintaining a clean SwiftUI codebase.
