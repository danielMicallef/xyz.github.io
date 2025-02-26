# Why *Two Scoops of Django* Recommends a Standalone Templates Folder

When structuring a Django project, the book *Two Scoops of Django* suggests placing all templates in a standalone `templates` folder at the project root, rather than scattering them across individual app directories. The recommended structure looks something like this:
```
├── manage.py
├── mysite
│   ├── __init__.py
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
├── myapp
│   ├── __init__.py
│   ├── models.py
│   ├── urls.py
│   └── views.py
├── another_app
│   ├── ...
│   └── views.py
├── assets
│   ├── javascript
│   └── styles
├── static
│   ├── css
│   ├── images
│   └── js  
└── templates
    ├── base.html
    ├── 404.html
    ├── myapp
    │   ├── home.html
    │   └── detail.html
    └── another_app
        ├── list.html
        └── form.html
```


But what makes this approach worth adopting? Below, we explore the key benefits of organizing your Django templates this way.

## 1. Separation of Concerns

By isolating templates in their own top-level folder, you draw a clean line between presentation logic (HTML templates) and application logic (models, views, etc.). This separation makes the project easier to navigate and understand, especially as it grows in complexity. Instead of hunting through app folders for templates, you know exactly where to look.

## 2. Improved Reusability

A centralized `templates` folder simplifies sharing templates across multiple apps. For instance, a `base.html` file or reusable components can sit at the root of `templates` and be extended or included by any app in the project. This cuts down on duplication and keeps your codebase DRY (Don’t Repeat Yourself).

## 3. Clarity for Larger Projects

In projects with several apps, each having its own `templates` subfolder can quickly become messy. A single `templates` directory—often with app-specific subfolders like `templates/myapp/`—offers a unified, predictable location for all HTML files. This clarity is a boon for teams and new developers who need to jump into the codebase without a steep learning curve.

## 4. Easier Configuration

Django’s `TEMPLATES` setting in `settings.py` can point to a single directory (e.g., `BASE_DIR / 'templates'`) instead of requiring a list of every app’s template folder. While Django’s default app directories loader works well for small projects, a standalone folder gives you explicit control over template discovery—an advantage in more intricate setups.

## 5. Consistency with Static Files

This structure aligns nicely with the common practice of using a top-level `static` folder for CSS, JavaScript, and images. Pairing `static` and `templates` at the same level creates a logical symmetry for front-end assets and templates, making the project layout feel intuitive and cohesive.

## 6. Scalability

As your project expands, you might need templates that don’t belong to a specific app—think error pages like `404.html` or `500.html`, or site-wide layouts. A standalone `templates` folder provides a natural home for these files, saving you from the awkward decision of which app should house them.

## Putting It Into Practice

To make this work, you’d typically organize app-specific templates in subfolders (e.g., `templates/myapp/`), which also taps into Django’s template namespacing to prevent naming conflicts between apps. The authors of *Two Scoops* champion this approach because it strikes a balance between simplicity, maintainability, and flexibility—particularly for projects that grow beyond a single app or a small codebase.

In short, adopting a standalone `templates` folder isn’t just a quirk of style; it’s a practical choice that pays off in clarity, scalability, and ease of management. Whether you’re building a modest site or a sprawling application, this structure is worth considering.
