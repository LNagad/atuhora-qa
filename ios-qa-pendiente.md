# QA pendiente en iOS (1 a 1 con Android)

_Creado: 24 de noviembre de 2026_

Lista para repetir en un **iPhone físico** las mismas pruebas que pasaron en Android el 24 de noviembre de 2026 ([reporte de Android](https://lnagad.github.io/atuhora-qa/2026-11-24-android/)). Cada caso lleva el mismo número que en Android para comparar captura con captura.

Al terminar, el reporte va en `2026-MM-DD-ios/` con el mismo formato y se enlaza en el [índice](https://lnagad.github.io/atuhora-qa/).

---

## 0. Preparación

### Entorno

| Pieza | Android (hecho) | iOS (por hacer) |
|---|---|---|
| Dispositivo | Pixel 11 Pro XL, ADB inalámbrico | iPhone físico con iOS actual |
| App | Development build `com.atuhora.app` | Development build `com.atuhora.app` para iOS |
| Conexión a Metro, API y web | `adb reverse` de `8081`, `3000` y `3001` (todo en `localhost`) | iOS no tiene `adb reverse`: **usar la IP de la computadora en la red local** |
| Capturas | `adb shell screencap` | Botón lateral + subir volumen, o Xcode → Devices |

### Pasos

- [ ] **Compilar la development build para iOS.** Una de dos:
  - En una Mac con Xcode: `CI=1 pnpm expo prebuild --platform ios` y luego `pnpm expo run:ios --device`.
  - En la nube: registrar el iPhone con `eas device:create` y luego `eas build --profile development --platform ios` (necesita la cuenta de Apple Developer).
- [ ] Confirmar que la build incluye los módulos nativos: `react-native-svg`, `expo-clipboard`, `expo-image-picker`, `expo-image-manipulator`, `expo-file-system` y `expo-sharing`.
- [ ] Levantar el stack en la computadora: PostgreSQL 17, API (`pnpm dev:api`, puerto `3000`) y web (`pnpm dev:web`, puerto `3001`).
- [ ] Arrancar Metro con la IP local: `EXPO_PUBLIC_API_URL=http://<IP-LAN>:3000 pnpm expo start --dev-client`.
- [ ] La API tiene que devolver las URLs de `/files` y del link público con esa IP, no con `localhost`: si no, el iPhone no carga la portada ni el logo.
- [ ] Aceptar el permiso de **Red local** que pide iOS la primera vez. Si se rechazó: Ajustes → Privacidad → Red local.
- [ ] Si las peticiones `http://` fallan en silencio, revisar App Transport Security en la development build.
- [ ] Usar las mismas cuentas de prueba que en Android (dueño, barbero y recepción) y los mismos dos negocios (El Filo Barbería y El Filo Norte). **Las contraseñas no van en este repositorio público:** se comparten aparte.
- [ ] Poner el horario del día de la prueba hasta medianoche (negocio y personas) para tener horas libres en la tarde, igual que en Android.

---

## 1. Mi página: portada, logo y vista previa v4

- [ ] **1.1** Subir la foto de portada eligiéndola del carrete con el selector de fotos de iOS.
- [ ] **1.2** Subir el logo y recortarlo.
- [ ] **1.3** Confirmar que la subida funciona en iOS: en Android hizo falta `new File(uri)` de `expo-file-system` para `expo/fetch`. Probar también una foto **HEIC** y una foto grande del iPhone.
- [ ] **1.4** **Publicar cambios** y ver «¡Publicada!».
- [ ] **1.5** **Vista previa** v4: portada, logo, «Abierto ahora», texto legible sobre el acento y servicios.
- [ ] **1.6** Abrir el link en Safari del iPhone y ver los cambios publicados en la web pública.
- [ ] **1.7** **Compartir por WhatsApp** y **Copiar** (pegar en Notas para comprobar el link).

## 2. Agenda

- [ ] **2.1** Agenda vacía: «Abre a las 9:00 a. m. y cierra a medianoche».
- [ ] **2.2** Hoja **Nueva cita**: servicios, personas y horas que vienen de la API real.
- [ ] **2.3** Crear las 4 citas de Android: `Pedro QA` (Corte de cabello, Omar, 19:00), `Juan NoVino` (Corte y barba, Carlos, 19:30), `Maria Cancela` (Arreglo de barba, Luis, 20:00) y `Diego Barbero` (Corte de niño, Carlos, 20:30).
- [ ] **2.4** Choque y limpieza: con Carlos ocupado de 19:30 a 20:15 más 5 minutos, la primera hora que ofrece es **20:30**.
- [ ] **2.5** El teclado de iOS no tapa el campo «Cliente» ni el botón **Agendar cita** (en Android esto dio problemas).
- [ ] **2.6** Tarjeta en tinta: «Sigue en N min», precio, hora grande, **Cobrar** y **No vino**.
- [ ] **2.7** Marcar **No vino** en `Juan NoVino`: pide confirmación y queda con la insignia.
- [ ] **2.8** **Cancelar cita** en `Maria Cancela`: pide confirmación y queda «Cancelada».
- [ ] **2.9** Los tramos libres se recalculan al instante («Libre 55 min» desde las 19:35 y «Libre 2 h 55 min» desde las 21:05).
- [ ] **2.10** Selector de día («Hoy ⌄») y línea del día con la marca de ahora.

## 3. Caja y Reportes

- [ ] **3.1** Banner «2 citas por cobrar · Suman RD$ 900», sin contar «No vino» ni «Cancelada».
- [ ] **3.2** **Por cobrar** → cobrar `Pedro QA` (RD$ 500). Dejar `Diego Barbero` sin cobrar para el caso 7.3.
- [ ] **3.3** **+ Ingreso**: RD$ 350, «Venta de productos», concepto «Cera mate».
- [ ] **3.4** **− Gasto**: RD$ 200.
- [ ] **3.5** **Anular** el gasto con motivo «Prueba QA»: queda tachado con el motivo y el total vuelve a RD$ 850.
- [ ] **3.6** **Reportes**: balance, ticket promedio, gráfica, métodos y ganancias por profesional.
- [ ] **3.7** **Exportar CSV**: se abre la hoja de compartir de **iOS** con `caja-AAAA-MM-01-a-AAAA-MM-DD.csv`. Guardarlo en Archivos y abrirlo para confirmar el contenido.
- [ ] **3.8** **Cerrar caja del día**, al final, después del caso 7.3: con RD$ 1,250 contado sale «Cuadra», pide confirmación y queda «Caja cerrada · hoy · Cuadró».
- [ ] **3.9** Con la caja cerrada, la Agenda ya no muestra **Cobrar**.

## 4. Turnos sin cita

- [ ] **4.1** **+ Agregar** `Walkin Uno` con «Corte de cabello» y `Walkin Dos` sin servicio.
- [ ] **4.2** Fila de espera con número y tiempo estimado («Ya», «~10 min»).
- [ ] **4.3** **Llamar a Walkin Uno**: «Atendiendo ahora · turno 1».
- [ ] **4.4** **Terminó**: «1 cliente atendido hoy».
- [ ] **4.5** Llamar a `Walkin Dos` y marcar **Se fue**: la fila queda vacía y el contador sigue en 1.

## 5. Más: negocio

- [ ] **5.1** **Servicios y precios**: los 4 servicios.
- [ ] **5.2** **Equipo**: personas con su rol.
- [ ] **5.3** **Horario**: Martes cierra a **medianoche** (en una línea, con el «+» desactivado). Guardar y ver «Horario guardado».
- [ ] **5.4** **Ausencias** de `Luis P.`: agregar la de hoy y quitarla.
- [ ] **5.5** El gesto de deslizar desde el borde izquierdo para volver funciona en todas estas pantallas (solo iOS).

## 6. Cambiar de negocio

- [ ] **6.1** **Más → Cambiar**: la hoja muestra El Filo Barbería («Ahora») y El Filo Norte («Entrar»).
- [ ] **6.2** Entrar a **El Filo Norte**: «Encargado de El Filo Norte», sin pedir la contraseña.
- [ ] **6.3** Volver a **El Filo Barbería** como dueño.

## 7. Roles: barbero y recepción

### Barbero (Carlos M.)

- [ ] **7.1** Cuatro pestañas, **sin Caja**; Turnos visible.
- [ ] **7.2** La Agenda muestra solo sus citas (`Juan NoVino` y `Diego Barbero`, no `Pedro QA`).
- [ ] **7.3** Cobrar `Diego Barbero` (RD$ 400) desde la Agenda: queda «Cobrada».
- [ ] **7.4** **Mi página** de solo lectura, con su aviso.
- [ ] **7.5** **Más**: la fila Equipo no abre; **Módulos** sin «Pedir» y con «Para activar un módulo, pídeselo al dueño…».
- [ ] **7.6** **Turnos** funciona.

### Recepción (Ana)

- [ ] **7.7** Sin Caja; Turnos visible.
- [ ] **7.8** La Agenda muestra las citas de todos, con «Completada» en lugar de «Cobrada».
- [ ] **7.9** **Mi página**, **Equipo** y **Horario** de solo lectura, cada uno con su aviso.

### De vuelta como dueño

- [ ] **7.10** Entrar de nuevo como dueño: Caja muestra el cobro del barbero (efectivo esperado RD$ 1,250). Seguir con el caso 3.8.

## 8. Reserva de punta a punta: web → app

- [ ] **8.1** En Safari del iPhone (o en la computadora), reservar en la página pública: `Cliente Web QA`, Corte de cabello, quien esté libre, una hora de la noche de hoy.
- [ ] **8.2** Ver el boleto v4 con la hora grande, la línea del día, quién atiende, el precio y el código.
- [ ] **8.3** **Agregar a mi calendario** abre el archivo de calendario en iOS y lo agrega a Calendario.
- [ ] **8.4** En la app, la Agenda (al refrescar) muestra `Cliente Web QA` como siguiente cita.
- [ ] **8.5** Con la caja cerrada, esa tarjeta muestra solo **No vino**.

---

## 9. Solo en iOS

Cosas que Android no cubre y conviene revisar en el mismo pase:

- [ ] **9.1** Zonas seguras: nada queda bajo la Dynamic Island (o el notch) ni bajo la barra de inicio, sobre todo el menú flotante y los botones fijos abajo («Publicar cambios», «Agendar cita»).
- [ ] **9.2** Hojas de abajo: arrastrar para cerrar y tocar fuera para cerrar.
- [ ] **9.3** Teclado numérico en montos (Caja, cobrar, cerrar caja): hay forma de cerrarlo y llegar al botón.
- [ ] **9.4** Pedir permisos: el selector de fotos funciona con acceso limitado («Seleccionar fotos»).
- [ ] **9.5** Texto grande (Ajustes → Pantalla → Tamaño del texto): las filas de Agenda y Caja no se cortan.
- [ ] **9.6** Volver a la app desde segundo plano después de unos minutos: la sesión se renueva sola y no saca al login.
- [ ] **9.7** La tipografía Atkinson Hyperlegible Next carga en toda la app (el cero lleva raya).

---

## 10. Resultado

| # | Área | Android (24 nov 2026) | iOS |
|---|---|---|---|
| 1 | Mi página: portada, logo y vista previa | PASS | — |
| 2 | Agenda | PASS | — |
| 3 | Caja y Reportes (CSV y cierre) | PASS | — |
| 4 | Turnos sin cita | PASS | — |
| 5 | Más: negocio | PASS | — |
| 6 | Cambiar de negocio | PASS | — |
| 7 | Roles: barbero y recepción | PASS | — |
| 8 | Reserva web → app | PASS | — |
| 9 | Solo en iOS | No aplica | — |

Nota que viene de Android, para decidir antes o después de iOS: el encabezado «Tienes N citas» cuenta las que quedaron en «No vino», y la línea del día no.
