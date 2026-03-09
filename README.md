# AI-Automation---Generador-de-Alertas-Bitcoin

Este proyecto consiste en un workflow de automatización desarrollado en **n8n** que monitorea el precio de Bitcoin en tiempo real y gestiona alertas preventivas ante volatilidades del mercado.

## 📋 Descripción del Caso
El objetivo es automatizar la vigilancia de activos cripto. El sistema consulta una API externa cada hora, registra el historial de precios en una base de datos interna y notifica vía email únicamente cuando se detecta una caída crítica (definida como >5% en 24hs).

## 🛠️ Estructura del Workflow
El flujo se compone de los siguientes nodos:

1.  **Schedule Trigger**: Disparador automático configurado para ejecutarse cada hora.
2.  **HTTP Request (CoinGecko API)**: Obtiene los datos de mercado de Bitcoin (precio y variación 24h) sin necesidad de autenticación.
3.  **Edit Fields (Set)**: Mapea y normaliza los datos. Se utiliza una expresión para ajustar la fecha a la zona horaria `America/Montevideo`.
4.  **Data Table (Insert Row)**: Registra cada consulta en una tabla interna de n8n para mantener persistencia de datos.
5.  **IF Node**: Evalúa la condición lógica: `usd_24h_change <= -5`.
6.  **Gmail Node**: Envía una alerta por correo electrónico con el asunto y mensaje dinámico detallando la magnitud de la caída.

## ⚙️ Configuración y Buenas Prácticas
* **Gestión de Errores**: Los nodos críticos poseen políticas de reintento (Max Tries: 5) para manejar fallos temporales de red o API.
* **Seguridad**: No se incluyen credenciales en el archivo JSON. Se utiliza el gestor nativo de credenciales de n8n para la conexión con Gmail (OAuth2).
* **Idempotencia**: El flujo está diseñado para ejecutarse de forma independiente en cada ciclo sin generar conflictos de datos.

## 📸 Evidencias
*(Sugerencia: Inserta aquí las capturas solicitadas en las consignas)*
* **Workflow Completo**: `evidencias/captura_workflow.png`
* **Notificación Recibida**: `evidencias/captura_email.png`
