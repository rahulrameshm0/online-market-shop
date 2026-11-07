# Puddle

A simple marketplace web application built with Django. Users can list items for sale, browse and search listings by category, and message each other directly about items they're interested in.

## Features

- **User accounts** — sign up, log in, and log out with Django's built-in authentication.
- **Browse & search** — browse unsold items, filter by category, and search by name or description.
- **Item listings** — create, edit, and delete your own listings, including an optional image, description, price, and category.
- **Mark as sold** — sellers can mark items as sold from the edit form.
- **Messaging** — start a conversation with a seller from an item page, then reply back and forth in an inbox.
- **Dashboard** — a private page listing everything you've put up for sale.
- **Admin panel** — manage users, categories, and items through the Django admin.

## Tech Stack

- **Backend:** Django 6.1.1 (Python)
- **Database:** SQLite (default development database)
- **Frontend:** Django templates with Tailwind CSS (loaded via CDN) plus custom CSS in the base template
- **Image handling:** Pillow (`ImageField` uploads)

## Project Structure

```
puddle/
├── manage.py
├── requirements.txt
├── db.sqlite3              # Local development database
├── puddle/                 # Project configuration
│   ├── settings.py
│   ├── urls.py
│   ├── asgi.py
│   └── wsgi.py
├── core/                   # Site-wide pages and auth flows
│   ├── views.py            # Home, contact, signup, logout
│   ├── forms.py            # Login and signup forms
│   └── urls.py
├── item/                   # Item and category domain
│   ├── models.py           # Category, Item
│   ├── views.py            # Browse, detail, create, edit, delete
│   └── urls.py
├── dashboard/              # "My account" / seller listings
│   ├── views.py
│   └── urls.py
├── conversation/           # Buyer–seller messaging
│   ├── models.py           # Conversation, ConversationMessage
│   ├── views.py            # Inbox, conversation detail, new conversation
│   └── urls.py
├── templates/
│   └── core/
│       └── base.html       # Shared layout and styling
├── media/                  # User-uploaded item images
└── item_images/            # Seed/sample images
```

Each app also contains its own `templates/<app>/` directory for app-specific pages.

## Getting Started

### Prerequisites

- Python 3.10 or newer
- `pip` and `venv`

### Installation

1. **Clone the repository and enter the project directory**

   ```bash
   git clone <repository-url>
   cd puddle
   ```

2. **Create and activate a virtual environment**

   ```bash
   python3 -m venv env
   source env/bin/activate        # macOS / Linux
   # env\Scripts\activate         # Windows
   ```

3. **Install dependencies**

   ```bash
   pip install -r requirements.txt
   ```

4. **Apply database migrations**

   ```bash
   python manage.py migrate
   ```

5. **Create an admin account (optional, for the admin panel)**

   ```bash
   python manage.py createsuperuser
   ```

6. **Run the development server**

   ```bash
   python manage.py runserver
   ```

7. **Open the app**

   Visit [http://127.0.0.1:8000/](http://127.0.0.1:8000/) in your browser.
   The admin panel is at [http://127.0.0.1:8000/admin/](http://127.0.0.1:8000/admin/).

## Usage

1. Create an account from the **Create account** link.
2. Click **Sell an item** to publish a listing. Choose a category and optionally upload an image.
3. Browse listings from the **Browse** page and use the category filter or search box to narrow results.
4. Open an item and click **Contact seller** to start a conversation.
5. Read and reply to messages in your **Inbox**.
6. Manage or remove your listings from **My account**.

## Routes

| URL | View | Description |
| --- | --- | --- |
| `/` | `core:index` | Home page with recent items |
| `/contact/` | `core:contact` | Contact page |
| `/signup/` | `core:signup` | Create an account |
| `/login/` | `core:login` | Log in |
| `/logout/` | `core:logout` | Log out |
| `/items/` | `item:items` | Browse and search items |
| `/items/new/` | `item:new` | Create a listing (login required) |
| `/items/<pk>/` | `item:detail` | Item detail page |
| `/items/<pk>/edit/` | `item:edit` | Edit your listing (owner only) |
| `/items/<pk>/delete/` | `item:delete` | Delete your listing (owner only) |
| `/dashboard/` | `dashboard:index` | Your listings (login required) |
| `/inbox/` | `conversation:inbox` | Your conversations (login required) |
| `/inbox/<pk>/` | `conversation:details` | Conversation thread (members only) |
| `/inbox/new/<item_pk>/` | `conversation:new` | Message an item's seller (login required) |

## Data Model

- **Category** — a name used to group items (`Item` ← `Category`).
- **Item** — belongs to a category and a user; has a name, description, price, optional image, sold flag, and creation timestamp.
- **Conversation** — ties an item to its members (the buyer and seller) and tracks when it was last modified.
- **ConversationMessage** — a message in a conversation, with content, author, and timestamp.

## Deployment Notes

The project includes settings for a Render deployment (`online-shop-6ocq.onrender.com`). Before deploying to production, you should:

- Replace `SECRET_KEY` with an environment variable.
- Set `DEBUG = False`.
- Update `ALLOWED_HOSTS` and `CSRF_TRUSTED_ORIGINS` for your domain.
- Configure a production database and static/media file storage.
- Collect static files with `python manage.py collectstatic`.

## License

This project is for learning and personal use. Add a license of your choice if you plan to distribute it.
