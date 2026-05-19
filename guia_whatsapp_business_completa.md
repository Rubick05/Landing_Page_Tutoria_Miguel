# 📱 Guía Completa: WhatsApp Business para TutorMiguel

Cubre todo: configuración, automatización, recepción de encargos, generación de guías y manejo de citas.

---

## PARTE 1 — Configurar WhatsApp Business

### 1.1 Instalar y configurar

1. Descarga **WhatsApp Business** desde Play Store o App Store.
2. Registra tu número `+591 79381414`.
3. Ve a **Configuración → Perfil de empresa** y completa:

| Campo | Qué poner |
|---|---|
| Nombre | TutorMiguel |
| Categoría | Educación |
| Descripción | Clases particulares y guías de estudio. Primaria, Secundaria, Preuniversitario y Universidad. |
| Horario | Lunes-Jueves 18-21h / Viernes 11-18h |
| Sitio web | https://tutormiguel.vercel.app |

### 1.2 Mensaje de Bienvenida

Ajustes → Herramientas para la empresa → **Mensaje de bienvenida**:

```
¡Hola! 👋 Soy *Miguel*, tutor universitario.

Gracias por escribirme. ¿En qué te puedo ayudar?

1️⃣ Clase individual (universidad)
2️⃣ Clase grupal (universidad)
3️⃣ Guía o resumen en PDF
4️⃣ Refuerzo escolar (primaria/secundaria)
5️⃣ Preparación para examen de ingreso

Respóndeme con el _número_ de tu opción y te ayudo en seguida 😊
```

### 1.3 Mensaje de Ausencia

Ajustes → Herramientas para la empresa → **Mensaje de ausencia** (activa fuera de horario):

```
¡Hola! 🌙 En este momento no estoy disponible.

Mi horario de atención es:
📅 Lun: 19:00-22:00
📅 Mar-Jue: 18:00-21:00
📅 Vie: 11:00-18:00

Te responderé lo antes posible. Puedes contarme qué necesitas y lo reviso cuando me conecte. ✅
```

### 1.4 Respuestas Rápidas (atajos)

Ajustes → Herramientas para empresa → **Respuestas rápidas**:

| Atajo | Mensaje |
|---|---|
| `/precio` | Los precios son: Individual presencial 40 Bs/hr, virtual 35 Bs/hr. Grupal (2-5 personas) presencial 25 Bs/persona, virtual 20 Bs/persona. Guías PDF: precio según tema. |
| `/horario` | Mis horarios: Lun 19-22h (virtual), Mar-Jue 18-21h, Vie 11-14h presencial y 15-18h virtual. |
| `/confirmar` | ✅ ¡Perfecto! Tu clase queda confirmada para el [DÍA] a las [HORA]. ¡Nos vemos! |
| `/rechazar` | Lo siento, ese horario ya está ocupado. ¿Puedes el [ALTERNATIVA]? |
| `/guia` | Para pedir una guía, indícame: 1) Tema o materia 2) Nivel (primaria/secundaria/uni) 3) Profundidad (resumen básico o completo). Te digo el precio en seguida. |

---

## PARTE 2 — Flujo Completo de Reserva de Clases

### Flujo ideal cuando llega un cliente:

```
Cliente: "Hola, quiero una clase de Física"
   ↓
Tú (o bot): "¡Excelente! ¿Es para uni, secundaria o primaria?"
   ↓
Cliente: "Secundaria, para examen la próxima semana"
   ↓
Tú: "Perfecto. Te propongo el Martes a las 19:00 o el Jueves a las 18:00. ¿Cuál te va mejor?"
   ↓
Cliente: "El martes"
   ↓
Tú (respuesta rápida /confirmar): "✅ ¡Perfecto! Tu clase queda confirmada para el Martes a las 19:00h. ¡Nos vemos!"
```

### Datos que debes registrar por cada reserva:
- Nombre del estudiante
- Tema / materia
- Nivel (primaria / secundaria / preuniversitario / uni)
- Modalidad (presencial / virtual)
- Fecha y hora
- Precio acordado

---

## PARTE 3 — Recibir Encargos de Guías (Flujo + Datos)

### 3.1 Flujo de encargo de guía

Cuando alguien pide una guía:

```
Cliente: "Necesito una guía de Cálculo Diferencial"
   ↓
Tú /guia: "Para pedirla, necesito saber:
  1) Tema exacto (ej: Derivadas, Integrales)
  2) Para qué nivel (uni - ingeniería, exactas, etc.)
  3) ¿Resumen básico o guía completa con ejercicios?"
   ↓
Cliente responde → tú dices el precio → cliente acepta
   ↓
Tú: "¡Listo! Te la entrego en [PLAZO]. El pago es: [QR/transferencia]"
```

### 3.2 Datos que extraer automáticamente

Para **registrar automáticamente** los pedidos, tienes dos opciones:

---

## PARTE 4 — Registro Automático en Google Sheets

> [!TIP]
> Esta es la opción más accesible: sin servidores, gratis, visible desde el celular.

### Cómo funciona:
1. Cuando confirmes un pedido, abres un **Google Form** y lo llenas en segundos.
2. Las respuestas se guardan solas en un **Google Sheet**.
3. Puedes ver todos tus pedidos y citas organizadas desde el celular.

### Crear el formulario:

Ve a [forms.google.com](https://forms.google.com) y crea un formulario con:

| Pregunta | Tipo |
|---|---|
| Nombre del cliente | Texto corto |
| Tipo de servicio | Opción múltiple (Clase Individual / Clase Grupal / Guía PDF) |
| Nivel | Opción múltiple (Primaria / Secundaria / Preuniversitario / Universidad) |
| Tema / Materia | Texto corto |
| Modalidad | Opción múltiple (Presencial / Virtual) |
| Fecha y hora | Fecha + Hora |
| Precio acordado (Bs) | Número |
| Estado | Opción múltiple (Pendiente / Confirmado / Completado / Cancelado) |
| Notas adicionales | Párrafo |

### Acceso rápido desde el celular:
- Guarda el link del formulario como favorito en Chrome.
- Cada vez que confirmas algo → abres el form → llenas en 30 segundos.
- El Google Sheet se actualiza automáticamente.

### Ver el Sheet como agenda:
- Puedes usar la hoja como filtro por fecha, tipo, estado.
- Compártela contigo mismo en Drive para acceso offline.

---

## PARTE 5 — Bot Automático con Node.js + Baileys

> [!IMPORTANT]
> Esta es la opción más poderosa: el bot hace todo solo. Necesitas conocimientos básicos de Node.js.

### Flujo automatizado completo:

```javascript
const { makeWASocket, useMultiFileAuthState } = require('@whiskeysockets/baileys');
const { GoogleSpreadsheet } = require('google-spreadsheet');
const { JWT } = require('google-auth-library');

const PHONE = "59179381414"; // Tu número (para recibir notificaciones)
const SHEET_ID = "TU_GOOGLE_SHEET_ID";

// Estado de conversaciones activas
const conversaciones = {};

async function startBot() {
    const { state, saveCreds } = await useMultiFileAuthState('./auth');
    const sock = makeWASocket({ auth: state });
    sock.ev.on('creds.update', saveCreds);

    sock.ev.on('messages.upsert', async ({ messages }) => {
        const msg = messages[0];
        if (!msg.message || msg.key.fromMe) return;

        const from = msg.key.remoteJid;
        const texto = (msg.message.conversation ||
                       msg.message.extendedTextMessage?.text || '').trim();
        const estado = conversaciones[from]?.paso || 'inicio';

        // ============ PASO 1: BIENVENIDA ============
        if (estado === 'inicio') {
            await sock.sendMessage(from, {
                text: `¡Hola! 👋 Soy *TutorMiguel*.\n\n¿Qué necesitas?\n\n1️⃣ Clase individual\n2️⃣ Clase grupal\n3️⃣ Guía o resumen en PDF\n4️⃣ Preparación examen de ingreso\n\nResponde con el número de tu opción:`
            });
            conversaciones[from] = { paso: 'tipo' };

        // ============ PASO 2: TIPO DE SERVICIO ============
        } else if (estado === 'tipo') {
            const tipos = { '1': 'Clase Individual', '2': 'Clase Grupal', '3': 'Guía PDF', '4': 'Preparación Ingreso' };
            const tipo = tipos[texto];
            if (!tipo) {
                await sock.sendMessage(from, { text: 'Por favor responde con 1, 2, 3 o 4.' });
                return;
            }
            conversaciones[from] = { ...conversaciones[from], tipo, paso: 'nivel' };
            await sock.sendMessage(from, {
                text: `Anotado: *${tipo}*\n\n¿De qué nivel?\n\n1️⃣ Primaria\n2️⃣ Secundaria\n3️⃣ Preuniversitario\n4️⃣ Universidad`
            });

        // ============ PASO 3: NIVEL ============
        } else if (estado === 'nivel') {
            const niveles = { '1': 'Primaria', '2': 'Secundaria', '3': 'Preuniversitario', '4': 'Universidad' };
            const nivel = niveles[texto];
            if (!nivel) {
                await sock.sendMessage(from, { text: 'Responde con 1, 2, 3 o 4.' });
                return;
            }
            conversaciones[from] = { ...conversaciones[from], nivel, paso: 'tema' };
            await sock.sendMessage(from, { text: `¿Cuál es el tema o materia específica?` });

        // ============ PASO 4: TEMA ============
        } else if (estado === 'tema') {
            conversaciones[from] = { ...conversaciones[from], tema: texto, paso: 'modalidad' };
            await sock.sendMessage(from, {
                text: `¿Qué modalidad prefieres?\n\n1️⃣ Presencial\n2️⃣ Virtual`
            });

        // ============ PASO 5: MODALIDAD ============
        } else if (estado === 'modalidad') {
            const modalidades = { '1': 'Presencial', '2': 'Virtual' };
            const modalidad = modalidades[texto];
            if (!modalidad) {
                await sock.sendMessage(from, { text: 'Responde 1 o 2.' });
                return;
            }
            conversaciones[from] = { ...conversaciones[from], modalidad, paso: 'nombre' };
            await sock.sendMessage(from, { text: '¿Cuál es tu nombre?' });

        // ============ PASO 6: NOMBRE ============
        } else if (estado === 'nombre') {
            const datos = { ...conversaciones[from], nombre: texto };
            conversaciones[from] = { ...datos, paso: 'confirmado' };

            // Guardar en Google Sheets
            await guardarEnSheet(datos, from);

            // Notificar a MIGUEL
            await sock.sendMessage(`${PHONE}@s.whatsapp.net`, {
                text: `🔔 *NUEVA SOLICITUD*\n\n👤 ${datos.nombre}\n📚 ${datos.tipo}\n🎓 ${datos.nivel}\n📖 ${datos.tema}\n📍 ${datos.modalidad}\n📱 ${from.replace('@s.whatsapp.net','')}\n\nResponde *ACEPTAR* o *RECHAZAR* seguido del número:\nEj: ACEPTAR ${from.replace('@s.whatsapp.net','')}`
            });

            // Confirmación al cliente
            await sock.sendMessage(from, {
                text: `¡Gracias, ${datos.nombre}! 🙌\n\nTu solicitud fue recibida:\n• Servicio: ${datos.tipo}\n• Nivel: ${datos.nivel}\n• Tema: ${datos.tema}\n• Modalidad: ${datos.modalidad}\n\nMiguel revisará disponibilidad y te confirmará pronto. ⏳`
            });

        // ============ PASO 7: MIGUEL ACEPTA/RECHAZA ============
        // Este bloque se activa cuando MIGUEL escribe desde su propio número
        } else if (from === `${PHONE}@s.whatsapp.net` && texto.startsWith('ACEPTAR ')) {
            const clientNum = texto.replace('ACEPTAR ', '').trim() + '@s.whatsapp.net';
            await sock.sendMessage(clientNum, {
                text: `✅ ¡Buenas noticias! Tu solicitud fue *confirmada* por Miguel.\n\nTe contactará pronto para coordinar el horario exacto. 📅`
            });
            await sock.sendMessage(from, { text: `✅ Confirmación enviada al cliente.` });

        } else if (from === `${PHONE}@s.whatsapp.net` && texto.startsWith('RECHAZAR ')) {
            const clientNum = texto.replace('RECHAZAR ', '').trim() + '@s.whatsapp.net';
            await sock.sendMessage(clientNum, {
                text: `Lo siento 😔, en este momento no tengo disponibilidad para ese horario.\n\n¿Quieres intentar con otra fecha o modalidad? Escríbeme nuevamente y lo vemos. 🤝`
            });
            await sock.sendMessage(from, { text: `❌ Rechazo enviado al cliente.` });
        }
    });
}

// Función para guardar en Google Sheets
async function guardarEnSheet(datos, from) {
    const serviceAccountAuth = new JWT({
        email: process.env.GOOGLE_CLIENT_EMAIL,
        key: process.env.GOOGLE_PRIVATE_KEY.replace(/\\n/g, '\n'),
        scopes: ['https://www.googleapis.com/auth/spreadsheets'],
    });
    const doc = new GoogleSpreadsheet(SHEET_ID, serviceAccountAuth);
    await doc.loadInfo();
    const sheet = doc.sheetsByIndex[0];
    await sheet.addRow({
        Fecha: new Date().toLocaleString('es-BO'),
        Nombre: datos.nombre,
        Teléfono: from.replace('@s.whatsapp.net', ''),
        Tipo: datos.tipo,
        Nivel: datos.nivel,
        Tema: datos.tema,
        Modalidad: datos.modalidad,
        Estado: 'Pendiente'
    });
}

startBot();
```

### Instalar dependencias:
```bash
npm init -y
npm install @whiskeysockets/baileys google-spreadsheet google-auth-library
node bot.js
# Escanea el QR con tu WhatsApp
```

---

## PARTE 6 — Generar Guías Automáticamente con IA

> [!NOTE]
> Una vez que tienes los datos del pedido (tema, nivel, tipo), puedes usar IA para generar el borrador de la guía.

### Opción A — ChatGPT (manual pero rápido)

Cuando llegue un pedido, ve a [chat.openai.com](https://chat.openai.com) y usa este prompt:

```
Crea una guía de estudio completa sobre [TEMA] para nivel [NIVEL].
Incluye:
- Resumen teórico claro (sin relleno)
- Fórmulas clave o conceptos principales
- 5 ejercicios resueltos paso a paso
- 5 ejercicios de práctica
Formato: organizado, con títulos y subtítulos. Estilo directo y claro.
```

Copia el resultado a Google Docs → lo formateas → exportas como PDF.

### Opción B — API de OpenAI (automatizado)

```javascript
const OpenAI = require('openai');
const openai = new OpenAI({ apiKey: process.env.OPENAI_API_KEY });

async function generarGuia(tema, nivel) {
    const completion = await openai.chat.completions.create({
        model: 'gpt-4o-mini',
        messages: [{
            role: 'user',
            content: `Crea una guía de estudio sobre ${tema} para nivel ${nivel}. 
                      Incluye teoría clara, fórmulas clave, 5 ejercicios resueltos y 5 de práctica.`
        }]
    });
    return completion.choices[0].message.content;
}
```

Puedes integrar esto al bot: cuando se confirma un pedido de guía, se genera automáticamente el borrador.

---

## PARTE 7 — Resumen Visual del Sistema Completo

```
CLIENTE escribe en WhatsApp
        ↓
   BOT lo atiende (Baileys)
        ↓
  Recopila datos (tipo, nivel, tema, modalidad, nombre)
        ↓
  Guarda en Google Sheets 📊
        ↓
  Notifica a MIGUEL en WhatsApp 🔔
        ↓
  MIGUEL escribe: ACEPTAR/RECHAZAR + número
        ↓
  BOT envía respuesta automática al cliente ✅/❌
        ↓
  Si es guía → ChatGPT genera borrador 📄
        ↓
  Miguel revisa y envía el PDF al cliente
```

---

## Herramientas y costos

| Herramienta | Costo | Para qué |
|---|---|---|
| WhatsApp Business App | Gratis | Mensajes de bienvenida / respuestas rápidas |
| Baileys (Node.js) | Gratis | Bot completo de reservas |
| Google Sheets | Gratis | Registro de pedidos |
| ChatGPT free | Gratis | Generar borradores de guías |
| OpenAI API | ~$0.01 por guía | Generación automática |
| Vercel | Gratis | Hosting de la landing page |
