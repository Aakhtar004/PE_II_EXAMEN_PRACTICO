# Examen Práctica Unidad II - PETI
Autor: Ahmed Hasan Akhtar Oviedo  
Fecha: 22/10/2025

Repositorio: https://github.com/Aakhtar004/PE_II_EXAMEN_PRACTICO.git

## Modificaciones y correcciones
Se corrigió la conectividad entre frontend y backend ajustando `frontend/.env` para usar `VITE_API_URL=http://127.0.0.1:8000` en lugar de `localhost`. En Windows, `localhost` puede resolver a IPv6 (`::1`) mientras Docker expone `127.0.0.1` (IPv4), provocando `ECONNABORTED`. Se reinició el contenedor del frontend para aplicar cambios, se añadió un `console.log` del `baseURL` en Axios y se verificó en los logs del backend la recepción de `POST /api/v1/auth/login`.

Se implementó y corrigió la exportación a PDF del resumen ejecutivo: se creó la utilidad `exportPdf` con `jsPDF` y `html2canvas`, se limitó la exportación únicamente al apartado de “Resumen Ejecutivo” usando `summaryRef`, y se mejoró la compatibilidad con imágenes externas estableciendo `allowTaint: false` y `useCORS: true` en `html2canvas`, además de `crossOrigin="anonymous"` en el logo. Se instalaron las dependencias `jspdf` y `html2canvas` en el frontend.

## Mejoras
Se añadió una utilidad de exportación a PDF multipágina (formato A4) con márgenes configurables y escala para mayor legibilidad. Se reforzó el diagnóstico de red con el log del `baseURL` y se documentaron las URLs y orígenes CORS a utilizar durante el desarrollo para prevenir errores de conectividad.

## Rutas corregidas
- Frontend (preview dev): `http://127.0.0.1:3000/`
- Backend API Base: `http://127.0.0.1:8000`
- Resumen ejecutivo: `GET /api/v1/plans/{planId}/executive-summary`
- Login (form-data): `POST /api/v1/auth/login`
- CORS (backend): incluir `http://127.0.0.1:3000` y `http://localhost:3000`

## Funcionalidades implementadas
- Exportar a PDF del apartado “Resumen Ejecutivo” (solo la tarjeta superior) desde `ResumenPage`.
- Utilidad `frontend/src/utils/exportPdf.js` (A4 multipágina, márgenes configurables, `jsPDF` + `html2canvas`).
- Botón “Exportar PDF” y `summaryRef` en `frontend/src/pages/ResumenPage.jsx` para limitar la exportación a la sección correcta.
- Registro de `baseURL` de Axios en `frontend/src/services/api.js` para depuración.
- Hook `useExecutiveSummary(planId)` disponible para integrar texto del resumen del backend (`/api/v1/plans/{planId}/executive-summary`).





## Próximas correcciones

Se implementará la corrección del apartado de notificaciones para asegurar la entrega confiable y consistente de avisos en la aplicación. Incluye alineación de eventos entre backend y UI, estados de lectura (leído/no leído), persistencia, y mejoras de rendimiento para evitar duplicados o pérdidas de mensajes. También se documentarán los endpoints y flujos involucrados para facilitar pruebas y soporte.

Se añadirá la opción de compartir el plan en formato solo texto, generando una versión sin imágenes ni estilos pensada para copiar y pegar o enviar por correo/mensajería. Este modo incluirá sanitización básica y estructura con títulos y listas para mantener legibilidad, y quedará accesible desde la página de resumen con un control dedicado.




