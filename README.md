# WhatsApp Webhook -> Excel / Google Sheets

Este repositorio contiene un webhook Flask que recibe mensajes entrantes (esperando campos `Body` y `From`) y escribe un código detectado en:

- **Excel local** (`openpyxl`) cuando `USE_SHEETS=false` (por defecto).
- **Google Sheets** cuando `USE_SHEETS=true` y variables `SHEET_KEY` y `CREDS_FILE` configuradas.

## Archivos incluidos
- `app.py` : servidor Flask principal.
- `requirements.txt`
- `Dockerfile`
- `Procfile` : para Render/Heroku.
- `.replit` : para ejecutar en Replit.

## Variables de entorno recomendadas
- `USE_SHEETS` = "true" o "false" (por defecto false)
- Si `USE_SHEETS=true`:
  - `CREDS_FILE` = ruta al JSON de la cuenta de servicio (subirlo al proyecto).
  - `SHEET_KEY` = id de la hoja de Google Sheets (parte de la URL).
- Si `USE_SHEETS=false`:
  - `EXCEL_PATH` = ruta del archivo Excel (default `whatsapp_codes.xlsx`)
- `PORT` = puerto (opcional)

## Despliegue rápido (Replit)
1. Crear nuevo Repl -> Python.
2. Subir todos los archivos y, si usas Google Sheets, subir `service_account.json`.
3. En Secrets/Environment variables definir `USE_SHEETS`, `SHEET_KEY` (y `CREDS_FILE` si aplica).
4. Run -> Replit te dará una URL pública. Copia `https://<tu-repl>.repl.co/webhook` en la configuración del webhook de tu proveedor (Twilio Sandbox o similar).

## Pruebas con ngrok (local)
1. Instala dependencias: `pip install -r requirements.txt`
2. Ejecuta local: `python app.py`
3. Abre túnel: `ngrok http 5000`
4. Copia la URL de ngrok y añade `/webhook`. Pégala como webhook entrante en Twilio Sandbox.

## Twilio Sandbox (pruebas)
1. Entra a tu cuenta Twilio -> Programmable SMS -> WhatsApp Sandbox.
2. Configura "When a message comes in" con tu URL `https://.../webhook`.
3. Envía mensajes al número sandbox para probar.

## Seguridad y producción
- En producción usa HTTPS y proveedores estables.
- Para alta concurrencia usa una base de datos y procesos workers.

## Notas
- No subas credenciales privadas a repositorios públicos.
- Ajusta la función `extract_code()` según el formato exacto de tus códigos.
