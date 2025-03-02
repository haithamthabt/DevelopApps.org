# Changelog

## [1.1.3] - 2025-03-02 - Content Improvements
### Added
- Added comprehensive table of contents to DbContext tutorial
- Added table of contents to Database Approaches tutorial
- Created detailed tutorials for database approaches and DbContext in MVC section
- Added extensive best practices section for DbContext management

## [1.1.2] - 2025-03-02 - Navigation Cleanup
### Changed
- Modified homepage navigation to only show top-level sections
- Moved ASP.NET subsections (MVC, Razor Pages, Blazor, Web API) under ASP.NET section
- Improved navigation hierarchy clarity and reduced homepage clutter

## [1.1.1] - 2025-03-02 - Navigation Update
### Changed
- Standardized layouts: using 'section' for all content pages except homepage
- Ensured consistent navigation structure across all sections and tutorials

## [1.1.0] - 2025-03-02 - Working Version
### Added
- Implemented clear two-level navigation structure
- Homepage shows all sections in left navigation
- Section pages show their tutorials in left navigation
- Tutorial pages show their parent section and sibling tutorials
- Top navigation bar for quick section access
- Search functionality across all content


## [0.2.0] - 2025-03-02

### Changed
- Simplified URL structure (e.g., /html/intro/ instead of /tutorials/html/2025-03-02-html-basics)
- Fixed navigation hierarchy to show proper section grouping
- Restored top navigation with consistent section names
- Fixed expandable sections in left sidebar
- Ensured each section only shows its own tutorials

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
