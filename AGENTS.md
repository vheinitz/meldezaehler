# Meldezähler – Arbeitsregeln für Agenten

> Angelegt 02.10.2026 auf Wunsch („ja, leg die AGENTS.md im Repo an“); Herleitung
> und Belege: docs/HINWEISE_FUER_PI_2026-10-02.md.

## Projekt
- Firmware: UhrMeldezaehler/ (Arduino, ESP32-S3, Waveshare AMOLED 2.06), Oberfläche LVGL 9.3.
- Schichten: UhrMeldezaehler_core.h (Logik, kein Zeichnen) · ui_ctl.h (Bedien-Logik)
  · ui*.h (Masken, nur LVGL) · hal_display.h (esp_lcd + DMA). Diese Grenzen einhalten.
- Navigation: globale Variable `screen`; uiSync() lädt die Maske. Masken ändern Zustand
  nur über ctl*-/Kernfunktionen.
- Alles, was LVGL oder I2C anfasst, läuft in loop(). BLE-Callbacks reihen nur ein.
  CNN läuft im Task cnnWorker (Kern 0).

## Vor jeder Änderung
1. Den beschriebenen Fehler im beschriebenen Zustand messen (Sensor AN, BLE AN).
   Log: [perf] max loop / max ui / CNN avg (seriell, 115200).
2. Ursache belegen (Messwert oder Codezeile), erst dann ändern.
3. Bei größeren Änderungen: Plan mit Dateien, Prüfschritten, Risiken – vom Nutzer freigeben lassen.

## Prüfen
- Oberfläche: tools/ui_sim/build.sh muss „ALLES OK“ melden; geänderte Masken als PNG ansehen.
- Firmware: ./build.sh firmware (-O2). Keine Warnungen aus eigenem Code.
- Auf der Uhr: SCREEN n für Maskenwechsel, Messwerte vorher/nachher ins LOGBUCH.
- Ruhezustand: keine Maske darf mehr als ~15 % Fläche pro Sekunde neu zeichnen.

## Gestaltung
- Nur Bausteine/Farben/Schriften aus ui_theme.h; keine Pixel-Einzelanfertigungen.
- Echte Umlaute (Schriften: tools/fonts.sh), Bestätigung vor jedem Löschen.

## Dokumentation
- LOGBUCH.md: datiert, nur was im Commit steht. Keine neuen Doku-Dateien ohne Auftrag.
- Fachbegriffe korrekt (serielle Konsole, Fork ≠ Branch).
