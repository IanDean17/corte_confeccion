# Activar reservas compartidas y correo automático

La página funciona sin configuración en **modo demostración**. Las reservas se guardan solo en el navegador de quien agenda, y el correo se muestra como vista previa.

Para que todos los clientes vean las mismas horas ocupadas y les llegue el correo de confirmación, completa estos dos pasos y pega las claves en el objeto `CFG` de `index.html`. Ambos servicios son gratuitos para este volumen.

## 1. Base de datos de reservas (Supabase)

1. Crea una cuenta en https://supabase.com y un proyecto nuevo.
2. Abre **SQL Editor** y ejecuta:

```sql
create table reservas (
  id bigint generated always as identity primary key,
  fecha date not null,
  hora time not null,
  nombre text not null,
  telefono text not null,
  correo text not null,
  servicio text not null,
  detalle text,
  creada timestamptz default now(),
  unique (fecha, hora)          -- impide que dos personas tomen la misma hora
);

alter table reservas enable row level security;

-- Los visitantes pueden crear reservas, pero no leer los datos de otros clientes
create policy "crear reservas" on reservas for insert to anon with check (true);

-- Vista pública que expone SOLO fecha y hora (para pintar el calendario)
create view ocupados with (security_invoker = false) as
  select fecha, hora from reservas;
grant select on ocupados to anon;
```

3. En **Project Settings → API**, copia:
   - la **Project URL**, que va en `supabaseUrl`;
   - la clave **anon public**, que va en `supabaseKey`.

Tu mamá puede ver todas las reservas, con nombre, teléfono y correo, en **Table Editor → reservas**.

## 2. Correo automático (EmailJS)

1. Crea una cuenta en https://www.emailjs.com.
2. En **Email Services**, conecta el Gmail del taller. El ID va en `emailjsService`.
3. En **Email Templates**, crea una plantilla:
   - **To Email:** `{{to_email}}`
   - **Reply To:** `{{reply_to}}`
   - **Asunto:** `Tu hora en Johanna Corte y Confección: {{fecha}} a las {{hora}}`
   - **Cuerpo:**
     ```
     Hola {{to_name}},
     Tu hora quedó confirmada:
     • Servicio: {{servicio}}
     • Fecha: {{fecha}}
     • Hora: {{hora}}
     • Detalle: {{detalle}}
     Si necesitas cambiarla, responde este correo. ¡Te esperamos!
     ```
   - En la pestaña **Auto-Reply** o **CC**, agrega el correo del taller para que tu mamá también reciba un aviso de cada reserva.
   - El ID de la plantilla va en `emailjsTemplate`.
4. En **Account → Public Key**, copia la clave. Va en `emailjsPublicKey`.

## 3. Pegar las claves

En `index.html`, busca la línea `const CFG={...}` y complétala. Por ejemplo:

```js
const CFG={supabaseUrl:'https://xxxx.supabase.co',supabaseKey:'eyJ...',emailjsService:'service_xxx',emailjsTemplate:'template_xxx',emailjsPublicKey:'xxxx',ownerEmail:'correo@deltaller.cl'};
```

La página reconoce las claves automáticamente y deja de mostrar el aviso de "modo demostración".
