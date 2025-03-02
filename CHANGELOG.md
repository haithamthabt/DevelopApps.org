# Changelog

## [0.1.0] - 2025-03-02

### Added
- Initial Jekyll blog setup with Just the Docs theme
- Created main section pages:
  - Programming Fundamentals
  - C# Tutorials
  - HTML Tutorials
- Created tutorial structure under `/docs` directory
- Added initial tutorials:
  - Programming Fundamentals:
    - What is Programming?
    - Variables and Data Types
    - Control Structures
  - C# Tutorials:
    - Introduction to C#
    - C# Variables and Types
    - C# Classes and Objects
  - HTML Tutorials:
    - HTML Basics
    - HTML Forms
    - HTML Tables

### Changed
- Reorganized file structure to use `/docs` instead of `/_tutorials`
- Updated navigation structure:
  - Main sections in top navigation
  - Individual tutorials in left sidebar
  - Landing pages for each section with clear topic listings
- Updated `_config.yml` to support new navigation structure
- Modified tutorial front matter to support proper nesting and ordering

### Technical Details
- Using Jekyll static site generator
- Theme: Just the Docs
- Navigation: Custom implementation using Jekyll's built-in navigation features
- File Structure:
  ```
  .
  ├── _config.yml
  ├── docs/
  │   ├── programming-fundamentals/
  │   ├── csharp/
  │   └── html/
  ├── programming-fundamentals.md
  ├── csharp.md
  └── html.md
  ```

### Next Steps
- Add search functionality
- Implement syntax highlighting for code blocks
- Add more tutorials to each section
- Create a contributing guide
- Add responsive images and diagrams
- Implement comments system
