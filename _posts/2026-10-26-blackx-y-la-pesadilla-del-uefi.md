---
title: "BlackX y la pesadilla del UEFI"
date: 2026-10-26 08:00:45 +0200
categories: ["Ciberseguridad", "Hardware"]
tags: ["uefi", "malware", "bootkit", "seguridad", "kernel"]
image:
  path: /assets/img/posts/2026-10-26-blackx-y-la-pesadilla-del-uefi.jpg
  alt: "BlackX y la pesadilla del UEFI"
---

A estas alturas ya deberías saber que si un atacante consigue meterse en el arranque de tu equipo, básicamente tiene las llaves de tu casa, del coche y hasta de la caja fuerte. Lo de BlackX, el nuevo bootkit que anda circulando por ahí, es un recordatorio bastante amargo de que nuestra confianza en el firmware está, siendo generosos, cogida con pinzas.

La movida con BlackX es que no se anda con chiquitas. Se salta el Secure Boot aprovechando una vulnerabilidad que han bautizado como Baton Drop. Por si no estás al tanto, el Secure Boot se supone que es ese portero de discoteca que solo deja entrar código firmado y legítimo antes de que cargue el sistema operativo. Pues bien, BlackX ha encontrado la forma de colar a sus colegas sin invitación. Lo que hace es cargar drivers maliciosos directamente en el kernel, asegurándose una persistencia que ya quisieran muchos troyanos de andar por casa. Una vez que el malware está ahí metido antes que Windows o Linux, da igual el antivirus que tengas instalado; ya no puede ver nada porque el bicho está literalmente por debajo de sus narices.

Lo que más me mosquea de todo esto no es que encuentren vulnerabilidades, porque el software es complejo y siempre habrá fallos, sino que estas cosas se sigan usando para espionaje de alto nivel. Según contaban en BleepingComputer, el rastro apunta a campañas dirigidas. Esto significa que alguien se ha gastado un buen dinero o ha invertido mucho tiempo en desarrollar este exploit para apuntar a objetivos concretos. No es el típico ransomware de un chaval aburrido en su cuarto; es ciberespionaje puro y duro.

El problema real con este tipo de bootkits es la visibilidad. Como usuario, o incluso como administrador de sistemas, es casi imposible saber si tu máquina ha sido comprometida en esta etapa del proceso de arranque. Puedes pasar mil escaneos, reinstalar el sistema operativo o formatear el disco duro, pero si el bicho ha infectado la partición EFI o ha manipulado el firmware, ahí va a seguir esperándote. Es la definición de pesadilla técnica. La arquitectura de nuestros equipos, que cada vez es más cerrada y compleja con el tema del UEFI, nos ha vendido la moto de que es más segura, pero a la hora de la verdad, solo ha trasladado la superficie de ataque a un nivel donde somos prácticamente ciegos.

Al final, me queda la sensación de que seguimos construyendo castillos de naipes tecnológicos. Intentamos poner capas de seguridad sobre capas de software que ya de por sí son un queso gruyere, y nos sorprendemos cuando alguien encuentra una rendija por la base. Si te dedicas a gestionar infraestructura crítica, igual va siendo hora de que empieces a mirar qué narices está haciendo tu hardware antes de que el kernel tome el control, porque confiar en el Secure Boot ya sabemos que no es sinónimo de estar tranquilo.

---
*Fuente: [BleepingComputer](https://www.bleepingcomputer.com/)*
