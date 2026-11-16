# Iberic Phone — ESX Legacy & QBCore

Iberic Phone is a FiveM smartphone resource compatible with **QBCore** and **ESX Legacy**.  
UI available in **Spanish** and **English** (toggle inside the phone).

---

## Español / Spanish

### Abrir el teléfono
- Tecla por defecto: **F1** (cambiable en Ajustes de FiveM → Key Bindings → FiveM).
- Comandos: `/ibericphone` o `/phone`.
- Configurable en `config.lua` con `Config.OpenKey`.

### Movimiento mientras se usa
La NUI usa `SetNuiFocusKeepInput(true)`. Puedes caminar, correr y mirar alrededor. Ataque, apuntar y rueda de armas se bloquean mientras el teléfono está abierto.

### Frameworks soportados
- QBCore (`qb-core`)
- ESX Legacy (`es_extended`)
- `Config.Framework = 'auto'` detecta automáticamente. Puedes forzar `'qb'` o `'esx'`.
- Requiere `oxmysql` y el framework arrancados **antes** de `iberic-phone`.

### Instalación
1. Copia la carpeta `iberic-phone` a tus resources.
2. Importa `sql/install.sql` en tu base de datos.
3. En `server.cfg`:
   ```cfg
   ensure oxmysql
   ensure qb-core   # o ensure es_extended
   ensure iberic-phone
   ```
4. Configura `Config.Framework`, `Config.PhoneItem` y `Config.RequireItem` en `config.lua`.
5. Asegúrate de que el ítem del teléfono exista en tu inventario.
6. (Opcional) Configura webhook de capturas y el recurso de voz (`Config.VoiceResource`).

### Aplicaciones incluidas
**Pantalla de inicio / dock:**
- Teléfono, Mensajes, Contactos, App Store, Cámara
- Ajustes, Socials, Garaje, Licencias, Galería, Calendario
- Business, Crypto, Banco, Lemon List (market)
- **Calculadora** (rediseñada)
- **Services** (servicios de emergencia / jobs on-duty)

**Dock (barra inferior):**
- Teléfono, Mensajes, Contactos, App Store, Cámara
- **Linterna** (botón en el dock: enciende/apaga luz real del personaje)

**App Store (instalables):**
- Banco, Crypto, Market, Business, Services

### Services (aplicación de servicios)
Al pulsar **Call** en un servicio (Police, Sheriff, Ambulance…):
1. El servidor busca jugadores **online** con el `job` configurado y **de servicio** (`onduty`).
2. Elige uno al azar que no esté en otra llamada / modo avión / bloqueado.
3. Inicia una **llamada real** del teléfono hacia el número de ese jugador.

Configuración en `config.lua`:
```lua
Config.ServiceContacts = {
    police    = { label = 'Police',    job = 'police',    description = 'Police assistance' },
    sheriff   = { label = 'Sheriff',   job = 'sheriff',   description = 'Sheriff department' },
    ambulance = { label = 'Ambulance', job = 'ambulance', description = 'Medical emergency' },
}
```
- `job` debe coincidir exactamente con el nombre del job del framework.
- Si no hay unidades de servicio → mensaje "No hay unidades de servicio en este momento".

### Calculadora
Aplicación nativa de calculadora básica (suma, resta, multiplicación, división, %, ±). Siempre visible en la parrilla de apps.

### Unificación ESX / QBCore
El recurso incluye capas de compatibilidad (`client/framework.lua` y `server/framework.lua`) que envuelven ESX para que el resto del código use una API estilo QBCore.  
No hace falta duplicar lógica: con `Config.Framework = 'auto'` funciona en ambos.

**Notas ESX:**
- Identificador: `xPlayer.identifier`.
- Número de teléfono: se intenta leer de charinfo / `users.phone`.
- Duty: usa `Config.ESXSetJobDuty` si tu servidor tiene sistema de duty.
- Garaje: por defecto `owned_vehicles`. Puedes sobrescribir con `Config.ESXGarageQuery`.

**Notas QBCore:**
- Usa datos de jugador, dinero, jobs e ítems de QBCore.
- Garaje: schema típico `player_vehicles`.

### Idiomas
Toggle ES / EN dentro del teléfono (botón en la esquina). La preferencia se guarda en el navegador NUI.

### Solución de problemas
- Revisa consola cliente y servidor buscando `[iberic-phone]`.
- Confirma que el SQL se importó y que `oxmysql` arranca antes.
- Verifica que `Config.Framework` coincida con el framework en uso.
- Para llamadas: el recurso de voz (`pma-voice` por defecto) debe estar iniciado y exportar `setPlayerCall`.

---

## English

### Opening the phone
- Default key: **F1** (change in FiveM Settings → Key Bindings → FiveM).
- Commands: `/ibericphone` or `/phone`.
- Default key is set with `Config.OpenKey` in `config.lua`.

### Movement while using it
NUI uses `SetNuiFocusKeepInput(true)`. Walking, running and looking remain available. Attack, aim and weapon wheel are blocked while the phone is open.

### Supported frameworks
- QBCore (`qb-core`)
- ESX Legacy (`es_extended`)
- `Config.Framework = 'auto'` auto-detects. Force with `'qb'` or `'esx'`.
- Start `oxmysql` and the framework **before** `iberic-phone`.

### Installation
1. Copy the `iberic-phone` folder into your resources.
2. Import `sql/install.sql` into your database.
3. In `server.cfg`:
   ```cfg
   ensure oxmysql
   ensure qb-core   # or ensure es_extended
   ensure iberic-phone
   ```
4. Set `Config.Framework`, `Config.PhoneItem` and `Config.RequireItem` in `config.lua`.
5. Make sure the phone item exists in your inventory system.
6. (Optional) Configure screenshot webhook and voice resource (`Config.VoiceResource`).

### Included applications
**Home / dock:**
- Phone, Messages, Contacts, App Store, Camera
- Settings, Socials, Garage, Licenses, Gallery, Calendar
- Business, Crypto, Bank, Lemon List (market)
- **Calculator** (redesigned)
- **Services** (emergency / on-duty job calls)

**Dock (bottom bar):**
- Phone, Messages, Contacts, App Store, Camera
- **Torch** (dock button: real player spotlight light on/off)

**App Store (installable):**
- Bank, Crypto, Market, Business, Services

### Services app
When you press **Call** on a service (Police, Sheriff, Ambulance…):
1. The server looks for **online** players who have the configured `job` and are **on duty**.
2. Picks one at random who is not already in a call / airplane mode / blocked.
3. Starts a **real phone call** to that player’s phone number.

Configuration in `config.lua`:
```lua
Config.ServiceContacts = {
    police    = { label = 'Police',    job = 'police',    description = 'Police assistance' },
    sheriff   = { label = 'Sheriff',   job = 'sheriff',   description = 'Sheriff department' },
    ambulance = { label = 'Ambulance', job = 'ambulance', description = 'Medical emergency' },
}
```
- `job` must match the framework job name exactly.
- If nobody is on duty → “No units on duty right now”.

### Calculator
Native basic calculator (add, subtract, multiply, divide, %, ±). Always present on the home grid.

### Removed apps in this version
The following “new” apps were removed for a cleaner phone:
- Clock
- Tasks
- Weather
- Expenses
- Converter
- Stopwatch
- Dice
- Cookie Clicker

### ESX / QBCore unification
Compatibility layers (`client/framework.lua` and `server/framework.lua`) wrap ESX so the rest of the code can use a QBCore-style API.  
No duplicated logic is required; `Config.Framework = 'auto'` works on both frameworks.

**ESX notes:**
- Identifier: `xPlayer.identifier`.
- Phone number: tries charinfo / `users.phone`.
- Duty: set `Config.ESXSetJobDuty` if your server has a duty system.
- Garage: defaults to `owned_vehicles`. Override with `Config.ESXGarageQuery`.

**QBCore notes:**
- Uses QBCore player data, money, jobs and usable items.
- Garage: typical `player_vehicles` schema.

### Languages
In-phone ES / EN toggle (corner button). Preference is stored in the NUI browser.

### Troubleshooting
- Check client and server console for `[iberic-phone]` errors.
- Confirm the SQL file was imported and `oxmysql` starts first.
- Verify `Config.Framework` matches the running framework.
- For calls: the voice resource (`pma-voice` by default) must be started and export `setPlayerCall`.

---

## How to add a new app (developers)

1. Register in `const APPS = { ... }` in `html/app.js`.
2. Add the id to `GRID` / `GRID_UNFOLD` (or `DOCK`).
3. Implement `R.myapp = () => \`...\``.
4. Handle actions with `ACT.xxx` and the existing `data-act` system.
5. Add English ↔ Spanish strings in the translations object at the bottom of `app.js`.
6. For persistent data, add a `CB('myapp:load', ...)` in `server/main.lua` and call it via `api('myapp:load', payload)`.

Never trust citizen IDs, phone numbers, amounts or jobs coming from the NUI — always derive identity from the server-side player object.
