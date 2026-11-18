# SuperApp Dolores (super_dolores)

Proyecto del *Manual TDD SuperApp Dolores*: mensajería, monedero, comercio local y
transporte en un solo ecosistema. El código de `lib/`, `test/` y `pubspec.yaml` es el del
manual, tal cual. No usa paquetes externos y no necesita permisos especiales.

## 1. Preparar (una sola vez)

El zip no trae las carpetas de plataforma (android, ios, web…) para que pese poco.
Dentro de la carpeta `super_dolores`:

```
flutter create . --project-name super_dolores
flutter pub get
```

> ⚠️ `flutter create .` agrega **test/widget_test.dart**, que busca `MyApp` (no existe en
> este proyecto) y hace fallar `flutter test`. **Bórralo.**

## 2. Ejecutar

```
flutter test
flutter run
```

## 3. Rondas TDD y capturas

Ver **GUIA_RONDAS.md**: qué línea comentar en cada ronda para que la prueba falle (ROJO),
cómo regresarla a VERDE y qué capturar.
