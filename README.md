# Estacion Inteligente - Sistema de Seguridad Automatizado

![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=for-the-badge&logo=html5&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-ES6-F7DF1E?style=for-the-badge&logo=javascript&logoColor=000)
![Tailwind CSS](https://img.shields.io/badge/Tailwind_CSS-CDN-38B2AC?style=for-the-badge&logo=tailwindcss&logoColor=white)
![Firebase](https://img.shields.io/badge/Firebase-Hosting%20%7C%20Auth%20%7C%20Realtime%20DB-FFCA28?style=for-the-badge&logo=firebase&logoColor=000)
![ESP8266](https://img.shields.io/badge/ESP8266-NodeMCU-000000?style=for-the-badge&logo=espressif&logoColor=white)

Proyecto de monitoreo y respuesta a eventos para hogar, desarrollado para la materia Automatizacion de Procesos (UNET).

La plataforma combina un nodo ESP8266 con sensores fisicos y un panel web en Firebase para:

- Monitorear condiciones del entorno en tiempo real.
- Evaluar riesgo segun reglas de combinacion de sensores.
- Registrar historial de alertas.
- Notificar al usuario por correo electronico.

## Tabla de contenido

1. [Caracteristicas principales](#caracteristicas-principales)
2. [Arquitectura general](#arquitectura-general)
3. [Hardware utilizado](#hardware-utilizado)
4. [Stack tecnologico](#stack-tecnologico)
5. [Logica de diagnostico](#logica-de-diagnostico)
6. [Estructura del proyecto](#estructura-del-proyecto)
7. [Requisitos previos](#requisitos-previos)
8. [Instalacion y ejecucion local](#instalacion-y-ejecucion-local)
9. [Despliegue en Firebase](#despliegue-en-firebase)
10. [Modelo de datos (Realtime Database)](#modelo-de-datos-realtime-database)
11. [Seguridad y buenas practicas](#seguridad-y-buenas-practicas)
12. [Roadmap sugerido](#roadmap-sugerido)
13. [Creditos](#creditos)

## Caracteristicas principales

- Monitoreo en tiempo real de sensores de movimiento, presencia en puerta y luz ambiente.
- Panel web con diagnostico de anomalias y visualizacion del estado actual.
- Historial de alertas persistido en Firebase Realtime Database.
- Alertas por correo mediante EmailJS cuando se detecta una condicion de riesgo.
- Sistema de autenticacion con Firebase Auth:
	- Registro con correo y contrasena.
	- Inicio de sesion con correo y contrasena.
	- Inicio de sesion con Google.
- Control de evaluacion (activar/desactivar analisis) desde el panel principal.

## Arquitectura general

```text
Sensores + ESP8266
				|
				v
Firebase Realtime Database (sensor/*)
				|
				v
Panel Web (index.html)
	- lee estados en tiempo real
	- ejecuta reglas de deteccion
	- guarda alertas en /alertas
	- envia email con EmailJS
				|
				v
Firebase Authentication
	- login/register
	- proteccion de acceso al panel
```

## Hardware utilizado

- ESP8266MOD (NodeMCU) con conectividad WiFi.
- Sensor PIR (movimiento).
- Sensor de luz (LDR/fotorresistencia).
- Sensor ultrasonico HC-SR04 (presencia/visitante).

## Stack tecnologico

- Frontend: HTML5 + JavaScript (ES Modules).
- Estilos: Tailwind CSS via CDN.
- Backend/BaaS: Firebase
	- Authentication
	- Realtime Database
	- Hosting
	- Cloud Functions (estructura preparada en `functions/`)
- Notificaciones: EmailJS (envio de correos).

## Logica de diagnostico

La evaluacion actual reacciona a estas combinaciones:

- Movimiento detectado + Visitante presente + Oscuro -> Intrusion nocturna.
- Movimiento detectado + Visitante presente + Luz -> Acceso no autorizado en horario diurno.
- Sin movimiento + Visitante presente -> Movimiento sospechoso cerca de puerta.
- Movimiento detectado + Sin visitante -> Intruso con las puertas cerradas.
- Sin movimiento + Luz -> Luces encendidas innecesariamente.
- Movimiento detectado + Fuera de rango -> Sensor ultrasonico bloqueado.

Si no se cumple ninguna combinacion, el panel reporta: "No se detectaron anomalias".

## Estructura del proyecto

```text
.
|-- firebase.json
|-- index.html
|-- login.html
|-- register.html
|-- style.css
|-- gmail.png
`-- functions/
		|-- index.js
		`-- package.json
```

## Requisitos previos

- Node.js LTS (recomendado >= 20).
- npm.
- Firebase CLI.
- Un proyecto Firebase con:
	- Authentication habilitado (Email/Password y Google).
	- Realtime Database en modo adecuado para desarrollo.

## Instalacion y ejecucion local

1. Clonar el repositorio.

```bash
git clone <URL_DE_TU_REPOSITORIO>
cd "Estacion Inteligente"
```

2. Instalar Firebase CLI (si no lo tienes).

```bash
npm install -g firebase-tools
```

3. Instalar dependencias de Cloud Functions.

```bash
cd functions
npm install
cd ..
```

4. Iniciar sesion en Firebase.

```bash
firebase login
```

5. Vincular el proyecto Firebase (si aplica).

```bash
firebase use --add
```

6. Ejecutar localmente.

```bash
firebase serve
```

Opcional con emuladores:

```bash
firebase emulators:start
```

Por defecto, Hosting se sirve en `http://localhost:5000`.

## Despliegue en Firebase

Despliegue completo:

```bash
firebase deploy
```

Solo Hosting:

```bash
firebase deploy --only hosting
```

Solo Functions:

```bash
cd functions
npm run deploy
```

## Modelo de datos (Realtime Database)

Rutas usadas por el panel:

```json
{
	"sensor": {
		"movimiento": "Movimiento detectado | Sin movimiento",
		"visitante": "Visitante presente | Sin visitante | Fuera de rango",
		"estadoLuz": "Luz | Oscuro"
	},
	"alertas": {
		"-Nx...": {
			"usuario": "usuario@correo.com",
			"motivo": "⚠️ Intrusion nocturna",
			"fecha": "27/3/2026, 10:42:18"
		}
	}
}
```

## Seguridad y buenas practicas

Este prototipo funciona para contexto academico, pero si lo publicas o lo evolucionas, considera lo siguiente:

- No exponer claves o identificadores sensibles en frontend publico.
- Mover configuraciones criticas a variables de entorno y/o backend.
- Restringir reglas de Firebase Realtime Database y Auth por roles.
- Evitar que cualquiera pueda escribir en `/alertas` sin autenticacion.
- Aplicar cuotas, rate limiting y validacion de payloads.
- Rotar llaves/credenciales si fueron expuestas en repositorios publicos.

## Roadmap sugerido

- Migrar logica de deteccion a Cloud Functions para centralizar reglas.
- Incorporar notificaciones push (FCM) ademas de correo.
- Agregar dashboard historico con filtros por usuario, fecha y severidad.
- Definir niveles de riesgo (bajo, medio, alto) con priorizacion.
- Integrar telemetria y pruebas automatizadas para escenarios criticos.

## Creditos

Desarrollado por:

- Juan Paredes
- Jose Bravo
- Angel Vivas

Universidad Nacional Experimental del Tachira (UNET)  
Automatizacion de Procesos 2025 - Apertura del Salon Japones

