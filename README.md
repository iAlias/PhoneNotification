# Phone Notifications Logger

**Lo storico delle notifiche Android, dentro Home Assistant — con una Lovelace card per sfogliarle.**

Un custom component che ascolta un sensore di notifiche del telefono (app **Home Assistant
Companion**) e conserva tutto lo storico su disco, così sopravvive ai riavvii di Home Assistant.
La card inclusa ti fa scorrere le ultime notifiche direttamente in dashboard.

---

## Cosa fa

- Ascolta un `sensor` di notifiche Android (attributi `android.title` / `android.text`)
- Salva lo storico in un file di storage persistente (`.storage`)
- Espone un sensore con:
  - **stato** = ultima notifica ricevuta
  - **attributi** `history_count`, `last_notification`, `notifications`
- Fornisce una Lovelace card (`custom:phone-notifications-card`)

---

## Requisiti

- Home Assistant
- **Home Assistant Companion** su Android, con il sensore delle notifiche attivo
  (l'entità è del tipo `sensor.<telefono>_active_notification_count`)

---

## Installazione

### Con HACS (consigliato)

1. Aggiungi questo repository come **Integrazione** personalizzata in HACS
2. Installa e riavvia Home Assistant

### A mano

Copia la cartella `custom_components/phone_notifications_logger` in
`config/custom_components/`, poi riavvia.

---

## Configurazione

Aggiungi a `configuration.yaml`:

```yaml
sensor:
  - platform: phone_notifications_logger
    sensor_entity_id: sensor.samsung_s21_active_notification_count_2
    name: "Notifiche Telefono"
    max_history: 1000
```

| Opzione | Obbligatorio | Predefinito | Descrizione |
|---|---|---|---|
| `sensor_entity_id` | ✅ | — | Il sensore di notifiche del telefono da ascoltare |
| `name` | ❌ | `Phone Notifications` | Nome del sensore creato |
| `max_history` | ❌ | `1000` | Numero massimo di notifiche conservate |

Riavvia Home Assistant: viene creato `sensor.notifiche_telefono` (o il nome scelto).

---

## Card Lovelace

Dopo l'installazione via HACS, il file

```
/hacsfiles/phone_notifications_logger/phone_notifications_card.js
```

Aggiungi una card manuale:

```yaml
type: custom:phone-notifications-card
entity: sensor.notifiche_telefono
max_items: 10
title: "Storico Notifiche"
```

| Opzione | Descrizione |
|---|---|
| `entity` | Il sensore creato da questa integrazione |
| `max_items` | Quante notifiche mostrare |
| `title` | Titolo della card |

La card mostra le ultime `max_items` notifiche con **data/ora, titolo e testo**.

---

## Attributi del sensore

| Attributo | Cosa contiene |
|---|---|
| `history_count` | Numero di notifiche salvate |
| `last_notification` | L'ultima notifica come oggetto `{ timestamp, title, text }` |
| `notifications` | L'intero storico |

---

## Licenza

[MIT](LICENSE) © Antonino Di Stefano
