---
layout: section
title: HTML Forms
nav_order: 2
parent: HTML Tutorials
grand_parent: HTML Tutorials
nav_order: 1
---

# HTML Forms

Forms are used to collect user input on web pages.

## Basic Form Structure

```html
<form action="/submit" method="post">
    <label for="username">Username:</label>
    <input type="text" id="username" name="username">
    
    <label for="password">Password:</label>
    <input type="password" id="password" name="password">
    
    <input type="submit" value="Submit">
</form>
```

## Common Form Elements

### Text Input
```html
<input type="text" placeholder="Enter your name">
```

### Radio Buttons
```html
<input type="radio" name="gender" value="male"> Male
<input type="radio" name="gender" value="female"> Female
```

### Checkboxes
```html
<input type="checkbox" name="hobby" value="reading"> Reading
<input type="checkbox" name="hobby" value="gaming"> Gaming
```

### Select Dropdown
```html
<select name="country">
    <option value="usa">USA</option>
    <option value="uk">UK</option>
    <option value="canada">Canada</option>
</select>
```

## Form Validation

```html
<input type="email" required>
<input type="tel" pattern="[0-9]{3}-[0-9]{3}-[0-9]{4}">
```

## Best Practices

1. Use proper labels
2. Include form validation
3. Group related fields
4. Provide clear instructions
5. Make forms accessible
