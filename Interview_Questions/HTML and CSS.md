# HTML & CSS MCQ Notes (Most Asked Questions)
_Last Minute Interview & MCQ Preparation_

---

# HTML

## 1. What does HTML stand for?

**Answer:**
HyperText Markup Language

---

## 2. Is HTML a programming language?

**Answer:**
No.
HTML is a **markup language** used to structure web pages.

---

## 3. What is the latest version of HTML?

**Answer:**
HTML5

---

## 4. Which tag creates the largest heading?

```html
<h1>
```

Answer:
`<h1>`

---

## 5. Which tag creates a paragraph?

```html
<p>
```

Answer:
`<p>`

---

## 6. Which tag inserts a line break?

```html
<br>
```

Answer:
`<br>`

---

## 7. Which tag creates a hyperlink?

```html
<a href="https://example.com">Visit</a>
```

Answer:
`<a>`

---

## 8. Which attribute specifies the destination of a link?

Answer:

```text
href
```

---

## 9. Which tag displays an image?

```html
<img src="image.jpg" alt="image">
```

Answer:
`<img>`

---

## 10. Which attribute specifies image description?

Answer:

```text
alt
```

---

## 11. Which tag creates an unordered list?

```html
<ul>
```

Answer:
`<ul>`

---

## 12. Which tag creates an ordered list?

```html
<ol>
```

Answer:
`<ol>`

---

## 13. Which tag defines a list item?

```html
<li>
```

Answer:
`<li>`

---

## 14. Which HTML tag creates a table?

```html
<table>
```

Answer:
`<table>`

---

## 15. Table row tag?

Answer:

```html
<tr>
```

---

## 16. Table header tag?

Answer:

```html
<th>
```

---

## 17. Table data tag?

Answer:

```html
<td>
```

---

## 18. Which tag creates a form?

```html
<form>
```

Answer:
`<form>`

---

## 19. Which attribute sends form data?

Answer:

```text
action
```

---

## 20. Which attribute specifies GET or POST?

Answer:

```text
method
```

---

## 21. Difference between GET and POST

| GET | POST |
|------|------|
| Data in URL | Data in Body |
| Less Secure | More Secure |
| Bookmarkable | Not Bookmarkable |
| Limited Size | Large Size |

---

## 22. Which input type hides password?

```html
<input type="password">
```

Answer:

```text
password
```

---

## 23. Which input type selects only one option?

Answer:

```text
radio
```

---

## 24. Which input type selects multiple options?

Answer:

```text
checkbox
```

---

## 25. Which tag creates a dropdown?

```html
<select>
```

Answer:
`<select>`

---

## 26. Which tag creates a multiline input?

```html
<textarea>
```

Answer:
`<textarea>`

---

## 27. Difference between id and class

| id | class |
|----|------|
| Unique | Multiple Elements |
| # | . |

---

## 28. Which attribute gives unique identification?

Answer:

```text
id
```

---

## 29. Which attribute groups elements?

Answer:

```text
class
```

---

## 30. Which semantic tag defines navigation?

```html
<nav>
```

Answer:
`<nav>`

---

## 31. HTML5 Semantic Tags

```text
<header>
<nav>
<main>
<section>
<article>
<aside>
<footer>
```

---

## 32. Which tag defines the main content?

Answer:

```html
<main>
```

---

## 33. Which tag defines page footer?

Answer:

```html
<footer>
```

---

## 34. Which tag defines page header?

Answer:

```html
<header>
```

---

## 35. Which tag embeds video?

```html
<video>
```

---

## 36. Which tag embeds audio?

```html
<audio>
```

---

## 37. Which attribute opens link in new tab?

```html
target="_blank"
```

---

## 38. Difference between block and inline elements

### Block

- div
- p
- h1
- section

Take full width.

### Inline

- span
- a
- strong
- em

Take only required width.

---

## 39. What is the purpose of DOCTYPE?

Answer:

Tells the browser the document is HTML5.

```html
<!DOCTYPE html>
```

---

# CSS

---

## 40. What does CSS stand for?

Answer:

Cascading Style Sheets

---

## 41. Purpose of CSS?

Answer:

Styles HTML elements.

---

## 42. Three ways to add CSS

1. Inline
2. Internal
3. External ✅ Best Practice

---

## 43. Which selector selects all elements?

```css
*
```

---

## 44. ID selector

```css
#header
```

---

## 45. Class selector

```css
.container
```

---

## 46. Element selector

```css
p
```

---

## 47. Which property changes text color?

```css
color
```

---

## 48. Which property changes background?

```css
background-color
```

---

## 49. Which property changes font size?

```css
font-size
```

---

## 50. Which property makes text bold?

```css
font-weight: bold;
```

---

## 51. Which property centers text?

```css
text-align:center;
```

---

## 52. Which property changes spacing inside element?

Answer:

```css
padding
```

---

## 53. Which property changes spacing outside element?

Answer:

```css
margin
```

---

## 54. CSS Box Model

```text
Margin
 ↓
Border
 ↓
Padding
 ↓
Content
```

---

## 55. Difference between Margin and Padding

| Margin | Padding |
|---------|----------|
| Outside | Inside |

---

## 56. Which property creates border?

```css
border
```

---

## 57. Which property rounds corners?

```css
border-radius
```

---

## 58. Which property adds shadow?

```css
box-shadow
```

---

## 59. Which property controls element width?

```css
width
```

---

## 60. Which property controls height?

```css
height
```

---

## 61. Which property hides an element?

```css
display:none;
```

---

## 62. Difference between display:none and visibility:hidden

| display:none | visibility:hidden |
|--------------|-------------------|
| Removed from layout | Still occupies space |

---

## 63. Flexbox is used for?

Answer:

One-dimensional layouts.

---

## 64. Grid is used for?

Answer:

Two-dimensional layouts.

---

## 65. Center an element using Flexbox

```css
display:flex;
justify-content:center;
align-items:center;
```

---

## 66. Which property makes Flexbox?

```css
display:flex;
```

---

## 67. Which property changes flex direction?

```css
flex-direction
```

---

## 68. justify-content aligns?

Answer:

Main axis.

---

## 69. align-items aligns?

Answer:

Cross axis.

---

## 70. Which property changes mouse cursor?

```css
cursor:pointer;
```

---

## 71. Position Values

```text
static
relative
absolute
fixed
sticky
```

---

## 72. Difference between Relative and Absolute

Relative:
Moves relative to itself.

Absolute:
Moves relative to nearest positioned ancestor.

---

## 73. Which property controls stacking order?

```css
z-index
```

---

## 74. Which pseudo-class changes style on hover?

```css
:hover
```

---

## 75. Which pseudo-class styles focused input?

```css
:focus
```

---

## 76. Which pseudo-element inserts content before?

```css
::before
```

---

## 77. Which pseudo-element inserts content after?

```css
::after
```

---

## 78. Which unit is relative to parent font size?

Answer:

```text
em
```

---

## 79. Which unit is relative to root font size?

Answer:

```text
rem
```

---

## 80. Which unit is percentage of viewport width?

Answer:

```text
vw
```

---

## 81. Which unit is percentage of viewport height?

Answer:

```text
vh
```

---

# Frequently Asked MCQs

### Q1

HTML is a ______ language.

A. Programming

B. Markup

C. Database

D. Scripting

✅ Answer: **B**

---

### Q2

Which tag creates a hyperlink?

A. `<img>`

B. `<link>`

C. `<a>`

D. `<url>`

✅ Answer: **C**

---

### Q3

Which input type hides entered text?

A. email

B. hidden

C. password

D. text

✅ Answer: **C**

---

### Q4

Which CSS property changes text color?

A. background

B. color

C. font

D. text

✅ Answer: **B**

---

### Q5

Which selector uses '#'?

A. Element

B. Universal

C. Class

D. ID

✅ Answer: **D**

---

### Q6

Which property creates Flexbox?

A. display:grid

B. display:block

C. display:flex

D. display:inline

✅ Answer: **C**

---

### Q7

Which property adds spacing outside an element?

A. padding

B. margin

C. border

D. spacing

✅ Answer: **B**

---

### Q8

Which positioning keeps an element fixed during scrolling?

A. relative

B. absolute

C. fixed

D. sticky

✅ Answer: **C**

---

### Q9

Which semantic tag represents navigation?

A. menu

B. navigation

C. nav

D. aside

✅ Answer: **C**

---

### Q10

Which HTTP method is commonly used when submitting a login form?

A. GET

B. POST

C. PUT

D. DELETE

✅ Answer: **B**

---

# Quick Revision (1 Minute)

- HTML = Structure
- CSS = Styling
- `<a>` = Hyperlink
- `<img>` = Image
- `<form>` = Form
- GET = URL
- POST = Request Body
- `id` = Unique (`#`)
- `class` = Multiple (`.`)
- Margin = Outside
- Padding = Inside
- Flexbox = 1D Layout
- Grid = 2D Layout
- `display: none` = Hidden + No Space
- `visibility: hidden` = Hidden + Keeps Space
- `position: fixed` = Fixed on Screen
- `position: absolute` = Relative to Positioned Parent
- `justify-content` = Main Axis
- `align-items` = Cross Axis
- `z-index` = Stacking Order
- `:hover` = Mouse Hover
- `:focus` = Input Focus
- `::before` / `::after` = Insert Generated Content