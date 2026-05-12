# ResearchCollab - Dynamic Academic Research Collaboration Platform

## 🎯 Project Overview

ResearchCollab is a fully dynamic Django-based platform for academic research collaboration. All pages are dynamically powered by backend APIs using Django's JsonResponse, with frontend AJAX calls via fetch API.

## 📋 System Requirements

- Python 3.13+
- PostgreSQL 12+
- Django 6.0+
- Virtual Environment

## ⚙️ Setup Instructions

### 1. Install Dependencies

```bash
cd academic_collab
pip install -r requirements.txt
```

Or manually:

```bash
pip install Django==6.0.4
pip install psycopg2-binary
pip install djangorestframework
```

### 2. Configure Database

Update `academic_collab/settings.py` database configuration:

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': 'academic_db',
        'USER': 'postgres',
        'PASSWORD': 'your_password',
        'HOST': 'localhost',
        'PORT': '5432',
    }
}
```

### 3. Run Migrations

```bash
python manage.py migrate
```

### 4. Create Superuser

```bash
python manage.py createsuperuser
```

### 5. Run Development Server

```bash
python manage.py runserver
```

Visit: http://127.0.0.1:8000/

## 🏗️ Project Structure

```
academic_collab/
├── academic_collab/          # Main project settings
│   ├── settings.py          # Django settings
│   ├── urls.py              # Main URL routes
│   ├── views.py             # Home page view
│   └── wsgi.py
├── accounts/                # User authentication
│   ├── models.py            # CustomUser model
│   ├── views.py             # Auth views
│   └── urls.py
├── analytics_dashboard/     # Core collaboration app
│   ├── models.py            # ResearchPaper, Researcher, Collaboration, Notification
│   ├── views.py             # API endpoints & page views
│   ├── urls.py              # API routes
│   └── migrations/
├── projects/                # Project management
│   ├── models.py            # Project model
│   └── urls.py
├── documents/               # Document sharing
│   ├── models.py            # Document model
│   └── urls.py
├── communication/           # Messaging
│   ├── models.py            # Message model
│   └── urls.py
├── templates/               # HTML templates
│   ├── base.html            # Main layout with sidebar
│   ├── home.html            # Landing page
│   └── analytics/
│       ├── admin/           # Admin pages
│       └── user/            # User pages
├── static/
│   ├── css/
│   │   └── site.css         # Custom styling
│   └── js/
│       ├── admin_portal.js  # Admin dashboard JS
│       └── user_portal.js   # User dashboard JS
└── manage.py
```

## 🗄️ Database Models

### ResearchPaper

- `title` - Paper title
- `abstract` - Paper abstract
- `authors` - M2M with Researcher
- `publication_date` - Publication date
- `citations` - Citation count
- `domain` - Research domain
- `created_by` - User who created
- `created_at` - Creation timestamp
- `updated_at` - Update timestamp

### Researcher

- `user` - OneToOne with User
- `name` - Researcher name
- `institution` - Institution name
- `field` - Research field
- `created_at` - Creation timestamp

### Collaboration

- `researcher1` - FK to Researcher
- `researcher2` - FK to Researcher
- `project_name` - Project name
- `date` - Collaboration date

### CollaborationRequest

- `sender` - FK to Researcher
- `recipient` - FK to Researcher
- `project_name` - Project name
- `message` - Request message
- `status` - pending/accepted/rejected
- `created_at` - Creation timestamp
- `updated_at` - Update timestamp

### Notification

- `user` - FK to User
- `message` - Notification message
- `is_read` - Read status
- `created_at` - Creation timestamp

## 🔌 API Endpoints

### Admin APIs

- `GET /analytics/api/admin/summary/` - Platform statistics
- `GET /analytics/api/admin/charts/` - Chart data
- `GET /analytics/api/admin/users/` - List all users
- `POST /analytics/api/admin/users/action/` - Activate/Deactivate/Delete user
- `GET/POST /analytics/api/admin/papers/` - List/Create papers
- `PUT/DELETE /analytics/api/admin/papers/<id>/` - Update/Delete paper
- `GET/POST /analytics/api/admin/researchers/` - List/Create researchers
- `GET /analytics/api/admin/reports/preview/` - Preview reports
- `GET /analytics/api/admin/reports/export/` - Export reports as CSV

### User APIs

- `GET /analytics/api/user/summary/` - User statistics
- `GET/PUT /analytics/api/user/profile/` - User profile
- `GET/POST /analytics/api/user/papers/` - User papers
- `GET/POST /analytics/api/user/collaborations/` - Collaboration requests
- `POST /analytics/api/user/collaborations/<id>/action/` - Accept/Reject request
- `GET /analytics/api/user/notifications/` - Notifications
- `GET /analytics/api/user/search/` - Search papers & researchers
- `GET /analytics/api/user/personal-analytics/` - User analytics data

### Shared APIs

- `GET /analytics/api/filters/` - Available filters
- `GET /analytics/api/options/researchers/` - Researcher options

## 🎨 Features

### 1. User Management

- User registration and authentication
- Profile management (name, institution, field)
- Role-based access (Admin/Researcher)

### 2. Research Papers

- Create research papers with abstract and metadata
- Multiple authors support
- Citation tracking
- Domain-based organization
- Full CRUD operations

### 3. Collaboration System

- Send collaboration requests to other users
- Accept/reject collaboration requests
- Track active collaborations
- Automatic notifications

### 4. Notifications System

- Real-time notifications for:
  - Collaboration requests
  - Request acceptance/rejection
- Mark as read functionality
- Dynamic notification list

### 5. Analytics & Reports

- Publication trends (monthly/yearly)
- Collaboration statistics
- Top researchers
- Domain-wise distribution
- CSV report generation with filters

### 6. Search & Discovery

- Advanced search by:
  - Keywords
  - Domain
  - Publication date
  - Researcher
- Global paper and researcher discovery

## 🔐 Security Features

- CSRF protection on all POST/PUT/DELETE requests
- User authentication required for all endpoints
- Admin-only access for management pages
- Superuser validation for admin operations
- Secure password handling via Django Auth

## 🚀 Key Technical Details

### Dynamic Architecture

- **No static HTML data** - All content fetched from APIs
- **Fetch API** - AJAX calls for dynamic updates
- **JsonResponse** - All APIs return JSON
- **Chart.js** - For analytics visualization
- **Bootstrap 5** - Responsive UI framework

### CSRF Token Handling

JavaScript automatically extracts CSRF token from:

1. Form input: `<input name="csrfmiddlewaretoken">`
2. Cookie: `csrftoken`

And adds it to POST/PUT/DELETE requests via `X-CSRFToken` header.

### Error Handling

- Loading indicators while fetching
- User-friendly error messages
- Fallback to default values

## 📝 Page Structures

### Admin Dashboard

- System statistics (users, researchers, papers, collaborations)
- Recent projects list
- Publication trends chart
- Citations trend chart
- Domain distribution

### User Dashboard

- Personal statistics
- My papers count
- My collaborations count
- My citations total

### Admin Management Pages

- **Manage Users** - View/Activate/Deactivate/Delete users
- **Manage Papers** - CRUD operations on research papers
- **Manage Researchers** - CRUD operations on researchers
- **Manage Collaborations** - View/Delete collaborations
- **Reports** - Generate and export reports as CSV

### User Pages

- **My Profile** - Edit personal information
- **My Papers** - Create/Edit/Delete research papers
- **Collaborations** - View/Send/Accept collaboration requests
- **Search** - Find papers and researchers
- **Notifications** - View and mark notifications as read
- **Personal Analytics** - Personal publication and citation trends

## 🧪 Testing the System

### 1. Test Admin Features

1. Login as superuser
2. Navigate to "Analytics Admin Dashboard"
3. Test Manage Users page:
   - Activate/Deactivate buttons
   - Delete button
4. Test Manage Papers page:
   - Create new paper
   - Edit paper
   - Delete paper

### 2. Test User Features

1. Create multiple user accounts
2. Login as user
3. Navigate to "My Research Papers"
4. Create a paper
5. Navigate to "Collaborations"
6. Send collaboration request to another user
7. Switch to other user and accept request
8. Verify notifications are created

### 3. Test Search & Analytics

1. Create several papers
2. Use Search page to find papers
3. Check Personal Analytics for charts

## 🐛 Troubleshooting

### Database Connection Error

```
psycopg2.OperationalError: could not connect to server
```

Solution: Ensure PostgreSQL is running and credentials are correct in settings.py

### CSRF Token Error

```
CSRF verification failed
```

Solution: Ensure X-CSRFToken header is being sent. Check browser console for errors.

### Table Does Not Exist

```
ProgrammingError: relation "analytics_dashboard_researchpaper" does not exist
```

Solution: Run migrations:

```bash
python manage.py migrate
```

## 📚 Migration Commands

```bash
# Create migrations for changes
python manage.py makemigrations

# Apply migrations
python manage.py migrate

# Reset a specific app
python manage.py migrate app_name zero

# Reapply migrations
python manage.py migrate app_name
```

## 🔧 Management Commands

```bash
# Create superuser
python manage.py createsuperuser

# Load seed data
python manage.py seed_analytics_data

# Django shell
python manage.py shell

# System checks
python manage.py check
```

## 📖 URL Navigation

- **Home Page**: `/`
- **Login**: `/login/`
- **Register**: `/signup/`
- **User Dashboard**: `/analytics/user/dashboard/`
- **Admin Dashboard**: `/analytics/admin/dashboard/`
- **Admin Panel**: `/admin/`

## ✅ Verification Checklist

- [x] All models properly defined with migrations
- [x] All API endpoints return JSON
- [x] CSRF tokens working on all POST/PUT/DELETE
- [x] Dynamic data loading from backend APIs
- [x] No hardcoded static data in templates
- [x] Error handling and user feedback
- [x] Responsive Bootstrap 5 design
- [x] Proper role-based navigation
- [x] Notification system working
- [x] Collaboration flow functional
- [x] Analytics and reporting operational
- [x] Search functionality implemented

## 🎓 Key Learning Points

1. **Django JsonResponse** for API design
2. **CSRF protection** for secure form submissions
3. **M2M relationships** for authors/papers
4. **Dynamic templating** with {% url %} tags
5. **Frontend async/await** for API calls
6. **Database migrations** for schema evolution
7. **Role-based access control** via superuser checks
8. **Chart.js integration** for data visualization

## 📞 Support

For issues or questions, check:

1. Django system checks: `python manage.py check`
2. Browser console for JavaScript errors
3. Django debug toolbar in development
4. Server logs for backend errors

---

**Last Updated**: April 29, 2026
**Version**: 1.0.0
**Status**: Production Ready ✅
