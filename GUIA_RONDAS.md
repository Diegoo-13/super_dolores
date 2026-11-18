# Guía de rondas TDD – SuperApp Dolores (ROJO → VERDE + capturas)

El proyecto viene COMPLETO, así que todo pasa en verde. Para mostrar el ciclo TDD
en cada ronda:

1. **ROJO:** comenta (`//`) la línea indicada, corre la prueba y verás que FALLA. 📸 Captura.
2. **VERDE:** quita el `//`, corre la misma prueba y verás que PASA. 📸 Captura.

> En VS Code: pon el cursor en la línea y presiona **Ctrl + /** (comenta y descomenta).
> Corre SOLO la prueba de la ronda con el comando que viene (`--plain-name` filtra por grupo).

**Qué debe salir en cada captura**
- 📸 ROJO: el código con la línea comentada + la terminal con `Expected: ... Actual: ...`
  y el mensaje **"Some tests failed."**
- 📸 VERDE: el código sin `//` + la terminal con **"All tests passed!"**

> ⚠️ Los números de línea son los del zip tal como te lo entregué. Si moviste algo, búscalo con Ctrl + F.

---

## Ronda 0 – Las cuatro ideas de diseño
No tiene código ni pruebas. 📸 Opcional: el mapa del proyecto o la sección del manual.

## Ronda 1 – Preparar el proyecto
Sin pruebas. 📸 `flutter pub get` sin errores y el árbol de carpetas `lib/dominio`,
`lib/presentacion`, `test`.

---

## Ronda 2 – Dinero, fechas y errores
```
flutter test test/dominio_test.dart --plain-name "dinero y fechas"
```
Esta función ocupa una sola línea, así que aquí no se comenta: se **modifica**.

| Paso | Qué hacer |
|---|---|
| 🔴 ROJO | En `lib/dominio/dinero.dart`, **línea 12**, borra el `+ 50` → queda `(centavos * pct) ~/ 100;` |
| 🟢 VERDE | Vuelve a poner `+ 50` (Ctrl + Z) |

**Qué falla:** `porcentaje redondea al más cercano` → `Expected: <500>  Actual: <499>`
(sin el +50 se trunca en vez de redondear).

---

## Ronda 3 – Monedero (libro de movimientos)
```
flutter test test/dominio_test.dart --plain-name "Monedero"
```
| Paso | Qué hacer |
|---|---|
| 🔴 ROJO | En `lib/dominio/monedero.dart`, **línea 89** (dentro de `pagar`), comenta: `//    if (previo != null) return previo;` |
| 🟢 VERDE | Quita el `//` |

**Qué falla:** `IDEMPOTENCIA: repetir la misma clave no cobra dos veces` → cobra el taxi
dos veces (`Expected: <7000>  Actual: <4000>`).

---

## Ronda 4 – Bus de eventos
```
flutter test test/dominio_test.dart --plain-name "Bus de eventos"
```
| Paso | Qué hacer |
|---|---|
| 🔴 ROJO | En `lib/dominio/eventos.dart`, **línea 92**, comenta: `//        errores.add(err);` |
| 🟢 VERDE | Quita el `//` |

**Qué falla:** `un suscriptor que falla no impide que los demás reciban el evento`
→ el error ya no queda registrado (`Expected: <1>  Actual: <0>`).

---

## Ronda 5 – Mensajería con dinero en el chat
```
flutter test test/dominio_test.dart --plain-name "Mensajería"
```
| Paso | Qué hacer |
|---|---|
| 🔴 ROJO | En `lib/dominio/mensajeria.dart`, **línea 54**, comenta: `//    if (de == para \|\| para == sistema) throw ...` |
| 🟢 VERDE | Quita el `//` |

**Qué falla:** `valida mensajes vacíos, enormes, a uno mismo y a "sistema"`
(ahora deja mandarse mensajes a uno mismo y hacerse pasar por "sistema").

---

## Ronda 6 – Comercio local con comisión
```
flutter test test/servicios_test.dart --plain-name "Comercio"
```
| Paso | Qué hacer |
|---|---|
| 🔴 ROJO | En `lib/dominio/comercio.dart`, **línea 76**, comenta: `//    if (previo != null) return previo;` |
| 🟢 VERDE | Quita el `//` |

**Qué falla:** `IDEMPOTENCIA: un doble toque no compra dos veces` → se crea otro pedido
y se descuenta inventario dos veces (`Expected: <49>  Actual: <48>`).

---

## Ronda 7 – Transporte (Haversine, tarifa y máquina de estados)
```
flutter test test/servicios_test.dart --plain-name "Transporte"
```
| Paso | Qué hacer |
|---|---|
| 🔴 ROJO | En `lib/dominio/transporte.dart`, **línea 70**, comenta: `//    if (!_transiciones[estado]!.contains(nuevo)) throw ...` |
| 🟢 VERDE | Quita el `//` |

**Qué falla:** `transiciones ilegales lanzan error y no cambian nada` y
`viaje completo` (ya se puede iniciar un viaje que nadie aceptó).

---

## Ronda 8 – Mini-apps, permisos y kill switch
```
flutter test test/dominio_test.dart --plain-name "Mini-apps"
```
Haz las dos (es la parte de seguridad, vale la pena mostrarla):

| Paso | Qué hacer |
|---|---|
| 🔴 ROJO (permisos) | En `lib/dominio/contexto.dart`, **línea 24**, comenta: `//    if (!app.permisos.contains(p)) throw ...` |
| 🔴 ROJO (kill switch) | En `lib/dominio/contexto.dart`, **línea 23**, comenta: `//    _registro.exigeActiva(app.id);` |
| 🟢 VERDE | Quita los `//` |

**Qué falla:**
- Permisos: `una mini-app de tercero (solo identidad) no toca dinero ni chats` y
  `el chat puede enviar dinero pero NO leer el saldo` (el horóscopo podría leer tu saldo).
- Kill switch: `kill switch: apagar una mini-app la oculta y bloquea sus llamadas`.

---

## Ronda 9 – Lealtad (motor de retención)
```
flutter test test/retencion_test.dart --plain-name "Lealtad"
```
| Paso | Qué hacer |
|---|---|
| 🔴 ROJO | En `lib/dominio/lealtad.dart`, **línea 57**, comenta: `//    if (!_procesados.add(e.id)) return;` |
| 🟢 VERDE | Quita el `//` |

**Qué falla:** `un evento repetido NO premia dos veces (idempotencia por id)`
→ regala puntos dobles.

---

## Ronda 10 – Analítica (DAU/WAU/MAU, cohortes, retención)
```
flutter test test/retencion_test.dart --plain-name "Analítica"
```
| Paso | Qué hacer |
|---|---|
| 🔴 ROJO | En `lib/dominio/analitica.dart`, **línea 44**, comenta: `//      if (r.usuarioId != usuario) continue;` |
| 🟢 VERDE | Quita el `//` |

**Qué falla:** `cohortes: quién llegó por primera vez cada día`, `retención D1 y D7`
y `semana 2...` (todos los usuarios "llegan" el día 1 porque se mezclan sus registros).

---

## Ronda 11 – Notificaciones éticas y sugerencias
```
flutter test test/retencion_test.dart --plain-name "Notificaciones"
```
| Paso | Qué hacer |
|---|---|
| 🔴 ROJO (silencio) | En `lib/dominio/notificaciones.dart`, **línea 40**, comenta: `//      if (enHorarioDeSilencio(n.fecha)) return false;` |
| 🔴 ROJO (tope) | En el mismo archivo, **línea 48**, comenta: `//      if (yaEnviadas >= maxPromocionalesPorDia) return false;` |
| 🟢 VERDE | Quita los `//` |

**Qué falla:** `horario de silencio de 10 pm a 8 am` y `tope de 2 promociones al día`
(y `el tope es POR persona`).

📸 Sugerencias (sin sabotaje, solo verde):
```
flutter test test/retencion_test.dart --plain-name "Sugerencias"
```

📸 Cierre de la ronda: `flutter test test/retencion_test.dart` completo en verde.

---

## Ronda 12 – ViewModels
Primero 📸 `flutter test test/servicios_test.dart` completo (incluye la prueba de
integración "un día de uso…").

### 12a. Monedero
```
flutter test test/vms_test.dart --plain-name "MonederoViewModel"
```
🔴 `lib/presentacion/vm/monedero_vm.dart`, **línea 32**, comenta: `//      alCambiar?.call();`
**Falla:** `enviar dinero funciona y avisa para refrescar el inicio` (el inicio no se entera).
🟢 Quita el `//`.

### 12b. Chat
```
flutter test test/vms_test.dart --plain-name "ChatViewModel"
```
🔴 `lib/presentacion/vm/chat_vm.dart`, **línea 20**, comenta: `//    _contexto.mensajes.marcarLeidos(otro);`
**Falla:** `abrir marca como leídos y enviar agrega el mensaje`.
🟢 Quita el `//`.

### 12c. Tienda
```
flutter test test/vms_test.dart --plain-name "TiendaViewModel"
```
🔴 `lib/presentacion/vm/tienda_vm.dart`, **línea 63** (dentro de `comprar`), comenta: `//      carrito.clear();`
**Falla:** `arma el carrito, calcula el total y compra` (el carrito no se vacía después de comprar).
🟢 Quita el `//`.

📸 Final: `flutter test test/vms_test.dart` completo en verde.

---

## Ronda 13 – Pantallas y la concha de la SuperApp
Sin pruebas nuevas. 📸 Capturas con `flutter run`: inicio (saldo, puntos, nivel,
sugerencia y mini-apps), chat enviando dinero, monedero, tienda comprando, viaje completo,
métricas y horóscopo (la mini-app de tercero sin permisos).

## Rondas 14 y 15 – Probar con personas / legal, seguridad, escalamiento
Son teoría. 📸 Opcional: tus respuestas a las preguntas de cierre.

---

## Cierre – evidencia final
```
flutter test
```
📸 **"All tests passed!"** con las 4 pruebas: `dominio_test`, `servicios_test`,
`retencion_test` y `vms_test`.

---

### Alternativa "rojo de verdad"
Si tu profe quiere que el rojo sea porque el código **no existe todavía**: selecciona TODO el
archivo de código (Ctrl + A), coméntalo (Ctrl + /), corre la prueba y falla con errores de
compilación → 📸. Descoméntalo → verde → 📸.
Ojo: casi todas las pruebas importan `plataforma.dart`, que usa todos los servicios; al
comentar un archivo completo fallan **todas** las pruebas de ese test, no solo las de la
ronda. Por eso es más claro usar la línea indicada.
