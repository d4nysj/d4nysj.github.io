---
title: "Tu Android TV barato es un espía de fábrica"
date: 2026-11-03 08:01:53 +0200
categories: ["Ciberseguridad", "Tecnología"]
tags: ["android", "malware", "botnet"]
image:
  path: /assets/img/posts/2026-11-03-tu-android-tv-barato-es-un-espia-de-fabrica.jpg
  alt: "Tu Android TV barato es un espía de fábrica"
---

Te compras un Android TV por treinta euros en una web china pensando que has triunfado y lo que acabas de meter en el salón es un caballo de Troya con puerto HDMI. No es una exageración ni el típico miedo de manual de seguridad. Google acaba de confirmar que ha tenido que desmantelar una red de más de un millón de estos dispositivos que venían infectados directamente desde la fábrica. El bicho se llama XCPE y lo que hacía era básicamente usar tu conexión a internet para montar una red de proxies sin que tú te enterases de nada.

Lo que más me flipa de esta historia es que no hablamos de que el usuario haya pinchado en un enlace raro o haya instalado una app pirata para ver el fútbol. El malware estaba integrado en el firmware del aparato. Es decir, el fabricante decidió que su modelo de negocio no era solo venderte el cacharro, sino también alquilar tu dirección IP a ciberdelincuentes. Cuando encendías la tele, tu decodificador se conectaba a un servidor de comando y control y se ponía al servicio de quien pagara por usar una conexión doméstica limpia para ocultar sus rastros.

A nivel técnico la jugada es redonda. Los atacantes buscan IPs residenciales porque son las que menos sospechas levantan. Si un servidor recibe mil peticiones desde un centro de datos, saltan las alarmas. Pero si las recibe de mil casas distintas en Cuenca, Albacete o Buenos Aires, parece tráfico legítimo. Google ha tenido que intervenir de forma agresiva para neutralizar esta botnet y, de paso, ha cortado lazos de forma definitiva con el fabricante implicado. Lo cuentan con todo el detalle en BleepingComputer, y la verdad es que pone los pelos de punta ver la escala del despliegue.

A mí lo que más me mosquea de todo esto es la falsa sensación de seguridad que tenemos con este tipo de hardware. Pensamos que un televisor o un decodificador no son ordenadores, pero lo son. Y tienen acceso a toda tu red local. Si ese bicho está escuchando en tu red, quién te dice que no está intentando entrar en tu NAS o ver qué archivos compartes en el grupo del hogar. La gente se gasta un dineral en un iPhone por la privacidad y luego mete un espía de veinte pavos conectado al router sin pestañear.

Google lleva tiempo intentando limpiar el ecosistema de Android TV, exigiendo certificaciones y Play Protect, pero el mercado de los dispositivos genéricos es la jungla. Estos fabricantes cambian de nombre cada dos meses, venden un millón de unidades y desaparecen. Por mucho que Google banee a uno, mañana saldrá otro con una carcasa diferente y el mismo malware en las tripas. Al final, el mayor agujero de seguridad sigue siendo nuestro empeño por buscar chollos imposibles donde no los hay.

Lo peor de todo es que el usuario medio no tiene forma de saber que su tele está trabajando para una mafia rusa o china mientras él ve una serie. No notas que el sistema vaya lento, no ves ventanas emergentes, simplemente tu factura de la luz sube un céntimo y tu conexión va un pelín más pesada. Yo lo tengo claro: si el hardware es sospechosamente barato, el producto eres tú, tu ancho de banda y la tranquilidad de tu red doméstica. No compensa ahorrarse cincuenta euros para acabar siendo el cómplice involuntario de un botnet global.

---
*Fuente: [BleepingComputer](https://www.bleepingcomputer.com/news/security/google-disrupts-million-device-xcpe-android-malware-botnet-bans-tv-box-maker/)*
