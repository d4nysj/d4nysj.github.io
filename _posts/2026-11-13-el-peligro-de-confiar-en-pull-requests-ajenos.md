---
title: "El peligro de confiar en pull requests ajenos"
date: 2026-11-13 08:00:24 +0200
categories: ["Ciberseguridad", "GitHub"]
tags: ["github", "malware", "seguridad", "cloud"]
image:
  path: /assets/img/posts/2026-11-13-el-peligro-de-confiar-en-pull-requests-ajenos.jpg
  alt: "El peligro de confiar en pull requests ajenos"
---

Han vuelto a liarla en GitHub. Si hace nada hablábamos de la limpieza habitual de repositorios, ahora toca hablar de wfx-loot, una campaña que básicamente ha secuestrado casi 5.000 cuentas de desarrolladores para hacer el trabajo sucio. Según cuentan en BleepingComputer, el método es tan sencillo como efectivo: toman el control de tu cuenta y, ni cortos ni perezosos, empiezan a abrir pull requests en más de 60.000 repositorios ajenos.

Lo que intentan colar es malware, obviamente. No es el típico script cutre, sino una herramienta diseñada para robar credenciales de servicios en la nube. Lo que más me mosquea de todo esto no es que el malware sea sofisticado, sino la escala. Cuando tienes a 5.000 cuentas legítimas actuando como caballos de Troya, el volumen de ruido que generan es suficiente para que cualquier automatización de seguridad se vuelva loca o, peor aún, que un desarrollador con prisa haga click en el botón de merge sin mirar lo que está pasando.

Al final, es la misma historia de siempre pero con más presupuesto. Los atacantes se han dado cuenta de que es mucho más fácil comprometer la identidad de alguien que ya tiene permisos de confianza en un proyecto, que intentar atacar la infraestructura desde fuera. Si abres un PR desde una cuenta que ya ha contribuido al código, pasas mucho más desapercibido que si intentas colar un ejecutable raro desde una cuenta recién creada. 

Me hace pensar en cuánta gente sigue sin usar una llave de seguridad física o, al menos, un segundo factor de autenticación decente. GitHub ha estado insistiendo mucho con el tema de las llaves U2F y no es por puro capricho corporativo. Si tienes una cuenta con acceso a repositorios privados o a claves de despliegue de AWS o Azure y todavía te fías del SMS o de una contraseña que reutilizas en tres sitios, tienes todas las papeletas para ser el próximo eslabón de una cadena como esta. Es increíble que en pleno 2024 sigamos confiando nuestra seguridad digital a sistemas que se inventaron cuando internet era poco más que un chat de texto.

A mí, sinceramente, lo que más me toca la moral es que ahora nos va a tocar revisar con lupa cada pull request que nos llegue de desconocidos, como si no tuviéramos ya suficiente carga de trabajo manteniendo el código limpio. Esto solo confirma que la confianza es el activo más caro de mantener en el desarrollo de software y que, en cuanto alguien encuentra un agujero por donde colarse, no se lo va a pensar dos veces. Ya no basta con escribir buen código, ahora hay que convertirse en el policía de tu propio repositorio. O eso, o empezamos a exigir un poco más de rigor en la gestión de credenciales antes de que nos cierren el chiringuito.

---
*Fuente: [BleepingComputer](https://www.bleepingcomputer.com)*
