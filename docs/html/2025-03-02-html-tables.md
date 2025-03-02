---
layout: default
title: HTML Tables
nav_order: 3
parent: Tutorials
grand_parent: HTML Tutorials
---

# HTML Tables

Tables are used to display data in rows and columns.

## Basic Table Structure

```html
<table border="1">
    <thead>
        <tr>
            <th>Name</th>
            <th>Age</th>
            <th>City</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>John</td>
            <td>25</td>
            <td>New York</td>
        </tr>
        <tr>
            <td>Sarah</td>
            <td>30</td>
            <td>London</td>
        </tr>
    </tbody>
</table>
```

## Table Elements

1. `<table>` - Defines a table
2. `<tr>` - Defines a row
3. `<th>` - Defines a header cell
4. `<td>` - Defines a data cell
5. `<thead>` - Groups header content
6. `<tbody>` - Groups body content
7. `<tfoot>` - Groups footer content

## Spanning Cells

### Column Span
```html
<td colspan="2">Spans 2 columns</td>
```

### Row Span
```html
<td rowspan="3">Spans 3 rows</td>
```

## Best Practices

1. Use semantic markup
2. Include proper headers
3. Keep tables simple
4. Consider mobile responsiveness
5. Use CSS for styling
