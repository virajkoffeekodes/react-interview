## Q1. What is HTML, and what are its key components?

==> HTML is the standard language used to create and structure web pages. It provides the basic framework for web content by defining elements like headings, paragraphs, links, images, and more.  

## Key Components of HTML:  

1. **Doctype Declaration (`<!DOCTYPE html>`)** – Specifies the version of HTML being used.  
2. **HTML Element (`<html>`)** – The root element that wraps all content on the page.  
3. **Head (`<head>`)** – Contains metadata such as the title, character encoding, styles, and scripts.  
4. **Title (`<title>`)** – Defines the title of the web page, shown in the browser tab.  
5. **Meta Tags (`<meta>`)** – Provide metadata like character set, viewport settings, and SEO details.  
6. **Body (`<body>`)** – Contains the main content of the web page, such as text, images, and other elements.  
7. **Headings (`<h1>` to `<h6>`)** – Define hierarchical headings.  
8. **Paragraphs (`<p>`)** – Define blocks of text.  
9. **Links (`<a>`)** – Create hyperlinks to other pages or resources.  
10. **Images (`<img>`)** – Embed images into the page.  
11. **Lists (`<ul>`, `<ol>`, `<li>`)** – Define ordered and unordered lists.  
12. **Tables (`<table>`, `<tr>`, `<td>`)** – Create structured tabular data.  
13. **Forms (`<form>`, `<input>`, `<textarea>`, `<button>`)** – Collect user input.  
14. **Semantic Elements (`<section>`, `<article>`, `<header>`, `<footer>`)** – Provide meaning and structure to web pages.  


## Q2. What are the new features introduced in HTML5?

==> 
# New Features Introduced in **HTML5**  

HTML5 brought several enhancements to web development, improving **semantics, multimedia, form controls, APIs, and performance**.  

## **1. New Semantic Elements** 🏷️  

HTML5 introduced **semantic elements** to improve readability and SEO:  

- `<header>` – Defines the header section.  
- `<nav>` – Represents navigation links.  
- `<section>` – Groups content thematically.  
- `<article>` – Represents standalone content like blogs or news articles.  
- `<aside>` – Defines side content (e.g., ads, sidebars).  
- `<footer>` – Contains footer-related content.  
- `<figure>` & `<figcaption>` – For images, diagrams, and captions.  

## **2. Multimedia Elements** 🎥🎵  

HTML5 replaced Flash with built-in support for audio and video:  

- `<audio>` – Embeds audio files (.mp3, .wav, .ogg).  
- `<video>` – Embeds video files (.mp4, .webm, .ogg).  
- `<source>` – Defines multiple media sources for compatibility.  
- `<track>` – Adds captions or subtitles to videos.  

## **3. Enhanced Form Elements** 📝  

HTML5 introduced new input types and attributes for better form handling:  

### **New Input Types**  
- `email` – Validates email input.  
- `tel` – Input for phone numbers.  
- `url` – Validates URL input.  
- `date`, `datetime-local`, `time`, `month`, `week` – For date and time selection.  
- `range` – A slider for selecting a numeric value.  
- `color` – Color picker.  
- `number` – Allows numeric input with increment/decrement buttons.  

## **4. Canvas API** 🎨  
The `<canvas>` element allows for dynamic drawing of graphics, animations, and charts using JavaScript.  

## **5. Local Storage & Session Storage** 💾  
HTML5 introduced the Web Storage API to store data locally in the browser.  

- **LocalStorage** – Stores data persistently.  
- **SessionStorage** – Stores data only for the session.  

## **6. Geolocation API** 📍  
Enables websites to fetch the user's location.  

📌 **Example:**  
```
javascript
navigator.geolocation.getCurrentPosition(function(position) {
  console.log("Latitude: " + position.coords.latitude);
  console.log("Longitude: " + position.coords.longitude);
});
```

## **7. WebSockets** 📡  
Enables real-time communication between the client and server.  

## **8. Drag & Drop API** 🖱️  
Allows elements to be dragged and dropped using JavaScript.  

## **9. Web Workers** 🏃  
Allows running scripts in the background without affecting the main thread. 


## Q3. **What Are Semantic HTML Elements?**  

==> Semantic HTML elements are elements that convey meaning about their content to both the browser and developers. Unlike non-semantic elements (`<div>`, `<span>`), semantic elements clearly define their purpose, improving **SEO, accessibility, and readability**.  

For example:  
✅ `<article>` → Represents an independent content block (like a blog post).  
❌ `<div class="article">` → Requires extra styling & description to give meaning.  

## **Examples of Semantic HTML Elements** 🏷️  

| **Semantic Element** | **Purpose** |
|----------------------|------------|
| `<header>` | Represents the top section of a page or article (usually contains a logo, navigation, etc.). |
| `<nav>` | Defines navigation links (menus, breadcrumbs, etc.). |
| `<section>` | Groups related content thematically. |
| `<article>` | Represents self-contained content like a blog post or news article. |
| `<aside>` | Defines side content, such as ads, sidebars, or related links. |
| `<footer>` | Contains footer information (copyright, links, etc.). |
| `<main>` | Identifies the main content of the document. |
| `<figure>` | Wraps images, illustrations, charts, etc. |
| `<figcaption>` | Provides a caption for the `<figure>` element. |
| `<mark>` | Highlights text for emphasis. |
| `<time>` | Represents a date or time. |


## Q4. What is the difference between `<section>`, `<article>`, `<aside>`, and `<div>`?
==> **Difference Between `<section>`, `<article>`, `<aside>`, and `<div>`**  

Each of these elements serves a different structural purpose in HTML.  

| **Element**   | **Purpose**  | **When to Use?**  |
|--------------|-------------|------------------|
| `<section>`  | Groups related content within a webpage.  | When content is thematically related but not a standalone article.  |
| `<article>`  | Represents a self-contained, independent piece of content.  | When content can be shared or distributed separately (blog post, news, etc.).  |
| `<aside>`    | Represents supplementary content, like sidebars or related links.  | When content is not part of the main flow but adds context (ads, links, etc.).  |
| `<div>`      | A generic container with no specific meaning.  | When no semantic element fits, used mainly for styling and layout.  |


## Q5. What is the difference between inline and block-level elements?

==> **Key Differences: Block-Level vs. Inline Elements**  

| **Feature**       | **Block-Level Elements**  | **Inline Elements**  |
|------------------|-------------------------|---------------------|
| **Line Break**   | Always start on a new line.  | Stays in the same line.  |
| **Width**        | Takes the full width of its container.  | Takes only the required width.  |
| **Containment**  | Can contain both block and inline elements.  | Can contain only text or other inline elements.  |
| **Use Case**     | Used for layout structuring.  | Used for styling small portions of text.  |
| **Examples**     | `<div>`, `<p>`, `<section>`, `<article>`, `<header>`, `<footer>`  | `<span>`, `<a>`, `<strong>`, `<em>`, `<label>`, `<abbr>`  |
