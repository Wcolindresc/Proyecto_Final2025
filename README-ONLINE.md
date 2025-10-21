# Fix Pack — Proyecto_Final2025 (100% Online)

1) Copia estos archivos a tu repo (mismos paths) y haz commit a `main`.
2) Crea Secrets en GitHub: SUPABASE_URL, SUPABASE_ANON_KEY, SUPABASE_SERVICE_ROLE_KEY, DATABASE_URL, FLASK_SECRET_KEY (+ opcionales de Render).
3) Ejecuta `Provision Database` en Actions para aplicar migraciones.
4) Crea el usuario admin en Supabase Auth y sincronízalo en `app_users` (ver instrucciones del chat).
5) Despliega en Render (Build: `pip install -r requirements.txt && npm install && npm run build`; Start: `gunicorn "app:create_app()" --bind 0.0.0.0:$PORT`).
