# My Blog

A simple, elegant blog website built with HTML, CSS, and JavaScript.

## Features

- 📱 Fully responsive design
- 📝 Markdown support for blog posts
- 🎨 Clean, modern UI
- 🚀 No build process required
- 📄 Multiple pages (Home, Blog, About, Post)

## Structure

- `index.html` - Homepage with featured posts
- `blog.html` - All blog posts listing
- `about.html` - About page
- `post.html` - Individual post view
- `styles.css` - All styling
- `script.js` - JavaScript for dynamic content and markdown rendering

## Getting Started

1. Open `index.html` in your web browser
2. Navigate through the site using the navigation menu
3. Click on any post to read the full content

## Adding New Posts

Edit the `posts` array in `script.js` to add new blog posts. Each post should have:
- `id`: Unique identifier
- `title`: Post title
- `date`: Publication date (YYYY-MM-DD)
- `excerpt`: Short description
- `content`: Full post content in Markdown

## Customization

- Modify colors in the `:root` section of `styles.css`
- Update site name in all HTML files
- Add your own content to the About page
- Change the footer copyright information

## Technologies Used

- HTML5
- CSS3 (with CSS Grid and Flexbox)
- Vanilla JavaScript
- [Marked.js](https://marked.js.org/) for Markdown parsing

## Browser Support

Works in all modern browsers (Chrome, Firefox, Safari, Edge).
