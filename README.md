# AnnotatorWebApp

AnnotatorWebApp is a Django backend for managing image-annotation projects. It provides API endpoints for creating projects, uploading images, saving annotations, exporting dataset outputs, and serving generated downloads.

The project is structured as a Django app under `AnnotatorWebApp/` with most API behavior implemented in `rest_api/`. Requests are routed under `/api/`, and controller classes are discovered dynamically from the controllers package.

## Features

- Create and manage annotation projects
- Upload images and store related assets
- Save project annotation data
- Export annotations as JSON bundles
- Export segmentation masks
- Export Pascal VOC outputs
- Save exported files to S3-backed storage
- Serve generated downloads during development

## Project Structure

- `manage.py`: Django entrypoint
- `AnnotatorWebApp/settings.py`: core Django settings and storage/database configuration
- `AnnotatorWebApp/urls.py`: root URL routing
- `rest_api/models.py`: project, image, annotation, and export models
- `rest_api/controllers/`: controller-based API endpoints
- `rest_api/utils/`: helper utilities for routing, image handling, and exports
- `requirements.txt`: pinned Python dependencies
- `Dockerfile`: container definition

## Setup

1. Create and activate a virtual environment.
2. Install dependencies:

```bash
pip install -r requirements.txt
```

3. Create `AnnotatorWebApp/local_settings.py` using `AnnotatorWebApp/local_settings.example.py` as a template.
4. Fill in your AWS and database credentials in the local settings file.
5. Review `AnnotatorWebApp/settings.py` and adjust database engine, host, port, or database name if needed for your environment.
6. Run migrations:

```bash
python manage.py migrate
```

7. Start the development server:

```bash
python manage.py runserver
```

## Local Secrets

Secrets are intentionally excluded from version control.

Create `AnnotatorWebApp/local_settings.py` with values in this shape:

```python
AWS_ACCESS_KEY_ID = "your-aws-access-key-id"
AWS_SECRET_ACCESS_KEY = "your-aws-secret-access-key"

DATABASE_USER = "your-database-username"
DATABASE_PASSWORD = "your-database-password"
```

The tracked example file `AnnotatorWebApp/local_settings.example.py` shows the expected structure without storing real credentials.

## API Overview

- Base API prefix: `/api/`
- Admin site: `/admin/`
- Download route: `/api/download/<filename>/`

Additional API routes are loaded dynamically from controller classes in `rest_api/controllers/`. The codebase includes flows for project creation, image upload, annotation persistence, and export operations.

## Export Support

The backend includes export workflows for:

- JSON annotation archives
- Segmentation mask images and RGB masks
- Pascal VOC output bundles

Generated assets are written through the configured storage backend and can be zipped for download.

## Docker

Build the image:

```bash
docker build -t annotator-webapp .
```

Run the container:

```bash
docker run -p 8000:8000 annotator-webapp
```

Provide environment-specific secrets and database access before using Docker in a shared or production-like environment.

## Notes

- `AnnotatorWebApp/local_settings.py` is gitignored and should remain local-only.
- `.DS_Store` is ignored to keep macOS metadata out of the repository.
- If collaborators still have older local history, they should resync carefully after the earlier secret-history rewrite.
