---
title: Implementando CAPTCHA en tu sitio web (reCAPTCHA vs Turnstile)
date: 2025-04-22 08:00:00 +0500
categories: [node.js, Web Security, Web Development, backend]
tags: [TAG]
---

## Implementando CAPTCHA en tu sitio web: reCAPTCHA vs Turnstile

Toda interacción de una web pública que ofrece consulta o registro de datos corre el riesgo de ser visitada por bots, ahora con la aparición de la IA esta tendencia es mayor. Cualquier consulta o escritura en una aplicación Web es costo que afectará tu bolsillo. 
Así que distinguir entre una visita humana y un bot es un necesidad para muchas aplicaciones Web. 
En estos escenarios analizaremos dos alternativas conocidas: Google ReCAPTCHA y CloudFlare Turnstile.


## Google reCAPTCHA: El más conocido

Google ofrece varias versiones de su sistema reCAPTCHA: Entre estas se encuentra la opción reCAPTCHA essential que es la que usaremos en el ejemplo. 
Las otras opciones representan propuestas más integrales y con características adicionales. 
[costos de Google reCAPTHA](https://cloud.google.com/recaptcha/docs/compare-tiers?hl=es-419)

**Implementación básica:**
1. Regístrate en la [consola de reCAPTCHA](https://www.google.com/recaptcha/admin/create)
2. Obtén tus claves (pública del sitio y secreta)
3. Agrega el script en el `<head>` en tu HTML o Web:
   ```html
   <script src="https://www.google.com/recaptcha/api.js" async defer></script>
   ```
4. Coloca el widget donde desees que aparezca:
   ```html
   <div class="g-recaptcha" data-sitekey="tu_clave_del_sitio"></div>
   ```
5. Verifica la respuesta en tu servidor usando la clave secreta

reCAPTCHA v3 funciona en segundo plano sin interrumpir a los usuarios, asignando una puntuación de riesgo que puedes utilizar para tomar decisiones. 
La versión v2 es la más conocida y la que muestra la casilla de 'No soy un robot'.

## Cloudflare Turnstile (smart CAPTCHA): Una alternativa prometedora

Turnstile se presenta como una alternativa más centrada en la privacidad, en este caso, también ofrece una opción empresarial que permite cologar los widgets en ilimitados números de paginas.

la solución general es gratuita, siendo un factor importante a considerar para todo website que requiera una protección de seguridad ante bots, y si es que no tienes planes de comprometerte con un costo a futuro como es el caso de google ReCAPTCHA:

**Implementación básica:**
1. Regístrate en el [panel de Cloudflare](https://dash.cloudflare.com/)
2. Crea un sitio y obtén tus claves privadas y publicas
3. Agrega el script en tu HTML:
   ```html
   <script src="https://challenges.cloudflare.com/turnstile/v0/api.js" async defer></script>
   ```
4. Inserta el widget:
   ```html
   <div class="cf-turnstile" data-sitekey="tu_clave_del_sitio"></div>
   ```
5. Verifica la respuesta en tu servidor usando la clave secreta

Ambas soluciones son similares en la implementación FrontEnd.

## Comparativa de características

| Característica         | Google reCAPTCHA                          | Cloudflare Turnstile                               |
| ---------------------- | ----------------------------------------- | -------------------------------------------------- |
| Experiencia de usuario | Puede ser intrusivo (v2) o invisible (v3) | Generalmente menos intrusivo                       |
| Privacidad             | Recopila datos para Google                | Enfoque en privacidad, menos recopilación de datos |
| Accesibilidad          | Problemas conocidos                       | Mejor rendimiento general                          |
| Integración            | Ampliamente soportado                     | Soporte creciente                                  |
| Implementación Front   | Sencillo de incluir                       | Sencillo de incluir                                |
| Implementación Back    | La validación de google es x-www-form     | La validación de cloudflare es json                |

## Estructura de costos

**Google reCAPTCHA:**
- Gratuito versión reCAPTCHA Essentials hasta 10,000 evaluaciones/mes (La web no lo indica pero es posible que luego de eso, ya no funcione)
- reCAPTCHA Standar: Gratis hasta 10,000 evaluaciones/mes, $8 hasta 100,000 solicitudes. (No lo indica pero se entiende que se incrementa por cada 100mil. )
- Versión Enterprise: Hasta 10,000 evaluaciones gratis*, $8 por hasta 100,000 evaluaciones mensuales y, luego, $1 por cada 1,000 evaluaciones

**Cloudflare Turnstile:**
- Completamente gratuito, sin límites de uso, Cloudflare ofrece el plan Enterprise con habilitación de Widgets sin límites, es decir se puede agregar sin limites  el widget en distintos formularios o secciones web.

## Costos proyectados

Para un sitio con **100,000 verificaciones mensuales**:
- Google reCAPTCHA: $0 (dentro del límite gratuito)
- Cloudflare Turnstile: $0

Para un sitio con **2 millones de verificaciones mensuales**:
- Google reCAPTCHA Enterprise: ~$2000/mes (incluye el descuento de las primeras 10,000 gratuitas)
- Cloudflare Turnstile: $0

Para un sitio con **10 millones de verificaciones mensuales**:
- Google reCAPTCHA Enterprise: ~$10,000/mes (por los 9.9 millones adicionales)
- Cloudflare Turnstile: $0

## Consideraciones finales

**Elige Google reCAPTCHA si:**
- Ya utilizas muchos servicios de Google, es decir si los productos Google forman parte de la estrategia es parte de una adquisición mayor de servicios Google.
- Necesitas la confiabilidad de una solución probada con experiencia.
- Te interesa la integración con Analytics y otras herramientas de Google, que pueden dar una visión mayor de los beneficios de una integración de diversos productos Google, tales como Google Workspace, entre otros.

**Elige Cloudflare Turnstile si:**
- La privacidad de los usuarios es prioritaria
- Buscas minimizar costos a largo plazo, es decir tu inversión se centra en otros costos operativos.
- Prefieres una solución menos intrusiva y simple de implementar.

La decisión final dependerá de tus necesidades específicas, pero Turnstile ofrece una propuesta de valor convincente, especialmente para sitios con alto tráfico donde los costos de reCAPTCHA podrían escalar significativamente.

Para la validación backend, he publicado dos ejemplos que pueden ayudarte en la confirmación de respuesta del widget, básicamente ambas soluciones son similares: se trata de validar la respuesta generada por el widget que se envía al backend de Google o Turnstile, la diferencia está en el header de la llamada a los respectivos backend, esa diferencia es importante para evitar errores en la validación. 

[backend en Express de Gogle Recaptcha](https://github.com/jorge-alvarado-revata/ex-captcha-validate)

[backend en Express de Cloudflare Turnstile](https://github.com/jorge-alvarado-revata/ex-captcha-turnstile-validate)

¿Qué solución CAPTCHA estás utilizando actualmente? ¿Has considerado integrar estas alternativas?
