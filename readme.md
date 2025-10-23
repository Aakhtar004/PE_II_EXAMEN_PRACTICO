# Examen Práctica Unidad II - PETI
Autor: Ahmed Hasan Akhtar Oviedo  
Fecha: 22/10/2025

Repositorio: https://github.com/Aakhtar004/PE_II_EXAMEN_PRACTICO.git

---

## Modificaciones y correcciones

### Conectividad frontend/backend
Se corrigió la conectividad entre frontend y backend ajustando `frontend/.env` para usar `VITE_API_URL=http://127.0.0.1:8000` en lugar de `localhost`. En Windows, `localhost` puede resolver a IPv6 (`::1`) mientras Docker expone `127.0.0.1` (IPv4), provocando `ECONNABORTED`. Se reinició el contenedor del frontend para aplicar cambios, se añadió un `console.log` del `baseURL` en Axios y se verificó en los logs del backend la recepción de `POST /api/v1/auth/login`.

### Exportación a PDF del Resumen Ejecutivo
Se implementó y corrigió la exportación a PDF del resumen ejecutivo: se creó la utilidad `exportPdf` con `jsPDF` y `html2canvas`, se limitó la exportación únicamente al apartado de “Resumen Ejecutivo” usando `summaryRef`, y se mejoró la compatibilidad con imágenes externas estableciendo `allowTaint: false` y `useCORS: true` en `html2canvas`, además de `crossOrigin="anonymous"` en el logo. Se instalaron las dependencias `jspdf` y `html2canvas` en el frontend.

### React Query: corrección “Missing queryFn”
Se alinearon los hooks del frontend con las funciones de `frontend/src/services/api.js` para evitar el error “Missing queryFn”. Se añadieron y verificaron funciones como:
- `authAPI.getProfile`
- `plansAPI.getAll`, `plansAPI.create`, `plansAPI.update`, `plansAPI.delete`
- `plansAPI.getNotifications`, `plansAPI.markNotificationRead`
- `plansAPI.getExecutiveSummary`

---

## Funcionalidades implementadas

### Compartir Plan: Invitaciones y Colaboradores
Se añadió una sección dedicada para compartir planes e invitar colaboradores desde el editor del plan.

- Nueva pestaña: “Compartir / Usuarios” en `PlanEditor` (`/plan/{planId}/usuarios`).
- UI: componente `UsersManager` con las acciones:
  - Invitar usuarios por email (válido y ya registrado en el sistema).
  - Estado de invitación `pending` hasta que el usuario acepte.
  - Listado de usuarios del plan con nombre, email, rol y estado.
  - Remover usuarios (si no son `owner`).
- Lógica de React Query: `usePlanUsers(planId)` con `useQuery` + `useMutation` y `invalidateQueries` para refrescar el listado tras invitar o remover.

### Notificaciones de Invitación
Los usuarios invitados reciben una notificación en su cuenta con opciones para aceptar o rechazar la invitación.

- Header (`Header.jsx`): muestra notificaciones mediante REST.
- Acciones:
  - Aceptar invitación → el usuario pasa a colaborador del plan.
  - Rechazar invitación → se descarta la invitación.
- Se mapean los campos del backend para tipo `plan_invitation`.

---

## Endpoints REST utilizados

- `GET /api/v1/plans/{plan_id}/users` → Listar usuarios del plan.
- `POST /api/v1/plans/{plan_id}/invite` → Invitar usuario por email.
- `DELETE /api/v1/plans/{plan_id}/users/{user_id}` → Remover usuario del plan.
- `GET /api/v1/plans/notifications` → Notificaciones del usuario.
- `POST /api/v1/plans/notifications/{notification_id}/read` → Marcar notificación como leída.
- `POST /api/v1/plans/{plan_id}/invitations/{invitation_id}/accept` → Aceptar invitación.
- `POST /api/v1/plans/{plan_id}/invitations/{invitation_id}/reject` → Rechazar invitación.
- `GET /api/v1/plans/{plan_id}/executive-summary` → Resumen Ejecutivo (para exportación a PDF).

---

## Ubicación de la funcionalidad en el frontend

- `frontend/src/pages/PlanEditor.jsx` → Añade pestaña “Compartir / Usuarios” y renderiza `UsersManager` con `planId`.
- `frontend/src/components/plan/UsersManager.jsx` → UI de invitación, listado y remoción de usuarios.
- `frontend/src/components/common/Header.jsx` → Notificaciones y acciones de aceptar/rechazar invitación.
- `frontend/src/services/api.js` → Funciones de REST usadas por hooks y componentes:
  - `plansAPI.getPlanUsers(planId)`
  - `plansAPI.inviteUser(planId, email)`
  - `plansAPI.removeUserFromPlan(planId, userId)`
  - `plansAPI.getNotifications()`
  - `plansAPI.markNotificationRead(notificationId)`
  - `plansAPI.acceptInvitation(planId, invitationId)`
  - `plansAPI.rejectInvitation(planId, invitationId)`

---

## Cómo usar la funcionalidad de Compartir

1. Abre un plan y entra en “Compartir / Usuarios”.
2. Ingresa el email de un usuario registrado y pulsa “Invitar”.
3. El usuario invitado verá la notificación en su cuenta:
   - Aceptar → se convierte en colaborador del plan.
   - Rechazar → se descarta la invitación.
4. En “Usuarios del Plan” podrás ver estado y rol, y remover usuarios (si no son `owner`).

---

## Verificación y pruebas

- Frontend dev server: `http://127.0.0.1:3000/` (o `http://localhost:3000/`).
- Backend API base: `http://127.0.0.1:8000`.
- Pruebas recomendadas:
  - Invitar un email válido de un usuario existente y confirmar estado `pending`.
  - Iniciar sesión como el usuario invitado y aceptar/rechazar la invitación.
  - Verificar actualización del listado de usuarios tras aceptar/rechazar.
  - Probar remover un colaborador y validar que deja de aparecer.

---

## Rutas relevantes
- Resumen ejecutivo: `GET /api/v1/plans/{planId}/executive-summary`
- Login (form-data): `POST /api/v1/auth/login`
- CORS (backend): incluir `http://127.0.0.1:3000` y `http://localhost:3000`

---

## Utilidad de Exportación a PDF
- Archivo: `frontend/src/utils/exportPdf.js` (A4 multipágina, márgenes configurables, `jsPDF` + `html2canvas`).
- Origen: botón “Exportar PDF” en `frontend/src/pages/ResumenPage.jsx`, renderizando únicamente el contenedor referenciado por `summaryRef`.

---

## Próximas mejoras
- Opción “compartir en texto” desde el Resumen: generar una versión limpia (sin imágenes ni estilos) para copiar/pegar o enviar por correo.
- Refinar roles de colaboradores (por ejemplo, `viewer` vs `editor`) según políticas de negocio.
- Añadir tests de UI para invitaciones y notificaciones.




