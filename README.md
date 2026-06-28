# Plan de Respuesta a Incidentes: Acceso No Autorizado en Red Corporativa

> **Propósito:** Plan secuencial de respuesta ante la detección de un acceso no autorizado en una red corporativa. Cubre acciones inmediatas (primeras horas) y estrategias de mitigación a largo plazo, siguiendo el marco de referencia de NIST SP 800-61 (Preparación → Detección y análisis → Contención, erradicación y recuperación → Actividad post-incidente).

---

## 📌 Tabla de Contenidos

1. [Tipos de incidente cubiertos](#tipos-de-incidente-cubiertos)
2. [Fases del plan de respuesta](#fases-del-plan-de-respuesta)
3. [Fase 1 — Detección y triage inicial (0-30 min)](#fase-1--detección-y-triage-inicial-0-30-min)
4. [Fase 2 — Contención inmediata (30 min - 4 h)](#fase-2--contención-inmediata-30-min---4-h)
5. [Fase 3 — Erradicación (4-48 h)](#fase-3--erradicación-4-48-h)
6. [Fase 4 — Recuperación (días 2-7)](#fase-4--recuperación-días-2-7)
7. [Fase 5 — Actividad post-incidente](#fase-5--actividad-post-incidente)
8. [Mitigación a largo plazo](#mitigación-a-largo-plazo)
9. [Roles y responsabilidades](#roles-y-responsabilidades)
10. [Checklist de respuesta rápida](#checklist-de-respuesta-rápida)

---

![Plan de respuesta a incidentes: acceso no autorizado en red corporativa]<img width="2800" height="3980" alt="infografia-ir-plan" src="https://github.com/user-attachments/assets/a1e99010-7ae0-40ed-91c8-e36ba6aec5b4" />


## Tipos de incidente cubiertos

Este plan aplica al acceso no autorizado en sus variantes más comunes en entorno corporativo:

| Tipo | Descripción |
|---|---|
| **Credenciales comprometidas** | Uso de usuario/contraseña válidos obtenidos por phishing, fuerza bruta, o reutilización de contraseñas filtradas |
| **Escalamiento de privilegios** | Una cuenta de bajo privilegio obtiene permisos administrativos de forma indebida |
| **Movimiento lateral** | El atacante, ya dentro de la red, se desplaza entre sistemas usando credenciales o vulnerabilidades internas |
| **Acceso remoto no autorizado** | Uso indebido de VPN, RDP, SSH o servicios de acceso remoto expuestos |
| **Cuenta de servicio/API comprometida** | Tokens, claves de API o cuentas de servicio usadas fuera de su contexto esperado |
| **Insider no autorizado** | Empleado o contratista accediendo a sistemas o datos fuera de su alcance autorizado |

El mismo esqueleto de fases aplica a los seis casos; lo que cambia es el detalle técnico de contención y erradicación, indicado en cada fase cuando corresponde.

---

## Fases del plan de respuesta

```
Detección y triage → Contención inmediata → Erradicación → Recuperación → Post-incidente
     (0-30 min)         (30 min - 4 h)        (4-48 h)      (días 2-7)      (semanas)
```

---

## Fase 1 — Detección y triage inicial (0-30 min)

**Objetivo: confirmar que el incidente es real y clasificar su severidad antes de actuar.**

1. **Validar la alerta.** Confirma si la detección viene de SIEM, EDR, IDS/IPS, reporte de usuario o un tercero. Descarta falsos positivos obvios (mantenimiento programado, prueba autorizada, cambio de IP legítimo).
2. **Activar el equipo de respuesta a incidentes (CSIRT/IRT).** Notificar según el plan de escalamiento predefinido — no esperar confirmación total para avisar a las personas correctas.
3. **Clasificar la severidad** según alcance (un endpoint vs. múltiples segmentos), tipo de dato en riesgo (operativo vs. datos personales/financieros), y privilegios comprometidos (usuario estándar vs. administrador de dominio).
4. **Iniciar el registro cronológico del incidente** (timeline) desde el primer minuto: qué se observó, cuándo, quién lo reportó y qué acciones se han tomado.
5. **Preservar evidencia volátil** antes de tocar nada: capturas de memoria, sesiones activas, conexiones de red abiertas, y logs en buffers que se sobrescriban pronto.

---

## Fase 2 — Contención inmediata (30 min - 4 h)

**Objetivo: detener el avance del atacante sin destruir evidencia ni alertarlo de forma que acelere el daño.**

1. **Aislar los sistemas afectados de la red** (segmentación o desconexión de red, no apagado) — apagar un equipo comprometido puede borrar evidencia en memoria que es clave para el análisis forense.
2. **Deshabilitar las cuentas comprometidas** en lugar de cambiarles la contraseña de inmediato si se sospecha que el atacante sigue activo y un cambio repentino podría alertarlo antes de contener todos sus puntos de acceso.
3. **Revocar sesiones activas y tokens** asociados a las cuentas o sistemas afectados (sesiones VPN, tokens OAuth, cookies de sesión, claves de API).
4. **Bloquear IPs y dominios de origen identificados** a nivel de firewall perimetral y WAF.
5. **Aumentar temporalmente el nivel de logging** en los sistemas relacionados para capturar más detalle del comportamiento del atacante mientras se decide el siguiente paso.
6. **Notificar internamente con discreción** — limitar quién sabe del incidente a las personas estrictamente necesarias mientras se contiene, para evitar fugas de información o pánico que compliquen la respuesta.
7. **Evaluar la necesidad de notificación regulatoria temprana.** Algunas jurisdicciones tienen plazos de notificación que comienzan a contar desde la detección, no desde la confirmación final.

---

## Fase 3 — Erradicación (4-48 h)

**Objetivo: eliminar la presencia del atacante y la causa raíz que permitió el acceso.**

1. **Realizar análisis forense del alcance real:** qué sistemas fueron tocados, qué datos se accedieron o exfiltraron, y cuál fue el vector de entrada inicial.
2. **Identificar y eliminar mecanismos de persistencia** que el atacante pudo haber dejado (cuentas nuevas, tareas programadas, servicios, reglas de reenvío de correo, claves SSH añadidas).
3. **Rotar todas las credenciales potencialmente expuestas**, no solo las de la cuenta inicialmente comprometida — incluye cuentas de servicio, certificados, claves de cifrado y secretos compartidos.
4. **Aplicar los parches o correcciones de configuración** que cierren la vulnerabilidad explotada (parche de software, corrección de configuración de acceso remoto, ajuste de reglas de firewall).
5. **Verificar la integridad de los sistemas afectados** comparando contra líneas base conocidas o backups limpios anteriores al incidente.
6. **Re-escanear toda la red** en busca de indicadores de compromiso (IoCs) relacionados, no solo en los sistemas ya identificados como afectados.

---

## Fase 4 — Recuperación (días 2-7)

**Objetivo: restaurar operaciones normales con confianza de que el atacante ya no tiene acceso.**

1. **Restaurar sistemas desde backups verificados como limpios**, priorizando los sistemas críticos para el negocio según el plan de continuidad operativa.
2. **Reincorporar sistemas a la red de forma gradual y monitoreada**, no todos a la vez — esto permite detectar reinfecciones rápidamente.
3. **Restablecer accesos de usuarios legítimos** con credenciales nuevas y, si es posible, autenticación multifactor reforzada durante el periodo de recuperación.
4. **Monitorear activamente durante al menos 2-4 semanas posteriores** los sistemas restaurados, buscando señales de reactivación del atacante o accesos residuales no detectados.
5. **Confirmar con las áreas de negocio** que la operación ha vuelto a la normalidad y documentar cualquier impacto operativo residual.

---

## Fase 5 — Actividad post-incidente

**Objetivo: aprender del incidente y reducir la probabilidad de recurrencia.**

1. **Realizar una reunión de lecciones aprendidas** (post-mortem) con todas las partes involucradas, sin enfoque punitivo — el objetivo es mejorar el proceso, no señalar culpables.
2. **Documentar el incidente completo:** línea de tiempo, vector de entrada, alcance, acciones tomadas, tiempo de respuesta en cada fase y costo estimado (operativo, reputacional, regulatorio).
3. **Actualizar el plan de respuesta a incidentes** con los hallazgos — qué funcionó, qué no, y qué le faltaba al plan original.
4. **Completar las notificaciones regulatorias o a terceros** pendientes, según la legislación aplicable y los contratos con clientes o socios.
5. **Comunicar internamente** los aprendizajes relevantes al resto de la organización (sin necesariamente exponer detalles sensibles del incidente).
6. **Cerrar formalmente el incidente** solo cuando todas las acciones de remediación estén verificadas y documentadas.

---

## Mitigación a largo plazo

Acciones estructurales que reducen la probabilidad y el impacto de futuros accesos no autorizados:

- **Implementar el principio de mínimo privilegio** de forma consistente en toda la organización, con revisiones periódicas de accesos (recertificación de cuentas).
- **Desplegar autenticación multifactor (MFA)** en todos los puntos de acceso críticos, especialmente VPN, accesos administrativos y servicios expuestos a internet.
- **Adoptar un modelo de segmentación de red** (zero trust o microsegmentación) que limite el movimiento lateral incluso si una cuenta se compromete.
- **Establecer monitoreo continuo (SIEM/SOC)** con reglas de detección específicas para patrones de acceso anómalo: horarios inusuales, geolocalización inconsistente, volúmenes de acceso atípicos.
- **Realizar pruebas de penetración y ejercicios de Red Team/Purple Team periódicos** para validar que los controles detectan y contienen escenarios realistas.
- **Mantener un inventario actualizado de activos y accesos** — no se puede proteger ni responder bien a lo que no se sabe que existe.
- **Capacitar continuamente al personal** en reconocimiento de phishing e ingeniería social, ya que las credenciales comprometidas siguen siendo el vector de entrada más común.
- **Formalizar y ensayar el plan de respuesta a incidentes** mediante simulacros (tabletop exercises) al menos una o dos veces al año.
- **Revisar y actualizar acuerdos de retención de logs** para asegurar que la evidencia forense esté disponible el tiempo suficiente cuando se necesite investigar.
- **Establecer contratos previos con proveedores de respuesta a incidentes (IR retainer)** para tener soporte especializado disponible sin negociar términos durante una crisis activa.

---

## Roles y responsabilidades

| Rol | Responsabilidad principal durante el incidente |
|---|---|
| **Líder de respuesta a incidentes (Incident Commander)** | Coordina todas las fases, toma decisiones finales, es el punto único de mando |
| **Equipo técnico de seguridad (CSIRT/SOC)** | Análisis forense, contención técnica, erradicación y recuperación |
| **TI / infraestructura** | Ejecuta aislamientos de red, restauraciones de sistemas y cambios de configuración |
| **Legal / cumplimiento** | Evalúa obligaciones regulatorias de notificación y gestiona aspectos contractuales |
| **Comunicaciones / RR.PP.** | Gestiona comunicación interna y externa, evita filtraciones no controladas |
| **Liderazgo ejecutivo** | Aprueba decisiones de alto impacto (pagar rescate, notificación pública, paro de operaciones) |
| **Recursos humanos** | Interviene en casos de insider no autorizado o cuando hay personal involucrado |

---

## Checklist de respuesta rápida

- [ ] Alerta validada y CSIRT activado
- [ ] Severidad clasificada y timeline iniciado
- [ ] Evidencia volátil preservada antes de cualquier cambio
- [ ] Sistemas afectados aislados de la red (no apagados)
- [ ] Cuentas comprometidas deshabilitadas y sesiones revocadas
- [ ] IPs/dominios maliciosos bloqueados en el perímetro
- [ ] Vector de entrada identificado y mecanismos de persistencia eliminados
- [ ] Todas las credenciales potencialmente expuestas rotadas
- [ ] Sistemas restaurados desde backups verificados como limpios
- [ ] Monitoreo reforzado activo durante el periodo de recuperación
- [ ] Notificaciones regulatorias y a terceros evaluadas y, si aplica, enviadas
- [ ] Reunión de lecciones aprendidas realizada y plan de IR actualizado

---

## ⚠️ Aviso

Este documento es un marco de referencia general a nivel de proceso y gestión, alineado con buenas prácticas como NIST SP 800-61. No sustituye un plan de respuesta a incidentes formal adaptado a la infraestructura, regulación y tolerancia al riesgo específicas de cada organización, ni asesoría legal sobre obligaciones de notificación. Se recomienda validar y ensayar este plan con el equipo de seguridad, legal y liderazgo antes de un incidente real.

---

*Última actualización: junio 2026*
