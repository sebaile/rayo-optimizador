# Catálogo de optimizaciones de Rayo

Generado automáticamente desde el código (`Rayo.exe --catalog`). 86 optimizaciones de sistema y 49 apps de debloat.

Leyenda: ⭐ incluida en el perfil Recomendado · 🎯 incluida en el perfil Competitivo · 🔁 requiere reinicio · 🚪 requiere cerrar sesión · ⚡ mayor consumo · 🛡️ reduce seguridad.

Anticheat: 🟢 No toca el juego (solo Windows, drivers, servicios o apps) · 🟡 Ajusta el proceso del juego, Sensible con anticheats (arranque o PCI, según el informe de evidencia) o Edita archivos del juego. Dice qué toca cada ajuste; no es una garantía frente a ningún anticheat.

## FPS y Latencia

### Activar Modo Juego ⭐
`fps.gamemode` · Riesgo: **Seguro** · Reversible · Anticheat: 🟢 No toca el juego

Asegura que el Modo Juego de Windows esté activo: evita que Windows Update instale drivers o avise de reinicios mientras juegas. En Windows 11 ya viene activo y no garantiza más FPS.

- `HKCU\Software\Microsoft\GameBar\AutoGameModeEnabled = 1 [DWORD]`
- `HKCU\Software\Microsoft\GameBar\AllowAutoGameMode = 1 [DWORD]`

### Desactivar grabación de Xbox Game Bar ⭐
`fps.gamedvr` · Riesgo: **Seguro** · Reversible · Anticheat: 🟢 No toca el juego

Apaga la captura y la grabación en segundo plano de Game Bar, que consumen GPU, VRAM y disco. Puedes seguir grabando con NVIDIA App (ShadowPlay).

- `HKCU\System\GameConfigStore\GameDVR_Enabled = 0 [DWORD]`
- `HKCU\Software\Microsoft\Windows\CurrentVersion\GameDVR\AppCaptureEnabled = 0 [DWORD]`
- `HKCU\Software\Microsoft\Windows\CurrentVersion\GameDVR\HistoricalCaptureEnabled = 0 [DWORD]`
- `HKLM\SOFTWARE\Policies\Microsoft\Windows\GameDVR\AllowGameDVR = 0 [DWORD]`

### Programación de GPU acelerada por hardware ⭐ 🔁
`fps.hags` · Riesgo: **Seguro** · Reversible · Anticheat: 🟢 No toca el juego

Tu GPU gestiona su propia cola de trabajo y memoria de video (HAGS). En juegos normales el efecto es pequeño o nulo (las pruebas independientes no muestran una mejora clara), pero hace falta para DLSS Frame Generation.

- `HKLM\SYSTEM\CurrentControlSet\Control\GraphicsDrivers\HwSchMode = 2 [DWORD]`

### Optimizaciones para juegos en ventana ⭐
`fps.windowed` · Riesgo: **Seguro** · Reversible · Anticheat: 🟢 No toca el juego

Usa el modelo de presentación «flip» en juegos DirectX 10/11 en ventana o sin bordes: menos latencia, sin tearing y compatible con G-SYNC.

- `HKCU\Software\Microsoft\DirectX\UserGpuPreferences\DirectXUserGlobalSettings → SwapEffectUpgradeEnable=1;`

### Desactivar optimizaciones de pantalla completa 🎯
`fps.fso` · Riesgo: **Moderado** · Reversible · Anticheat: 🟢 No toca el juego

Fuerza la pantalla completa exclusiva real en juegos DX9/DX11 que la soportan. Puede bajar el input lag en títulos antiguos; en juegos modernos la diferencia es mínima.

- `HKCU\System\GameConfigStore\GameDVR_FSEBehaviorMode = 2 [DWORD]`
- `HKCU\System\GameConfigStore\GameDVR_HonorUserFSEBehaviorMode = 1 [DWORD]`
- `HKCU\System\GameConfigStore\GameDVR_FSEBehavior = 2 [DWORD]`
- `HKCU\System\GameConfigStore\GameDVR_DXGIHonorFSEWindowsCompatible = 1 [DWORD]`

### Resolución de temporizador global 🎯 🔁 ⚡
`fps.timerres` · Riesgo: **Moderado** · Reversible · Anticheat: 🟢 No toca el juego

La resolución de 0,5–1 ms que pide un juego se aplica a todo el sistema, como en el Windows 10 original. Mejora la consistencia del frametime en algunos títulos.

- `HKLM\SYSTEM\CurrentControlSet\Control\Session Manager\kernel\GlobalTimerResolutionRequests = 1 [DWORD]`

### Desactivar el tick dinámico del kernel 🔁 ⚡
`fps.dyntick` · Riesgo: **Avanzado** · Reversible · Anticheat: 🟡 Sensible con anticheats

Mantiene el temporizador del sistema a ritmo constante en lugar de pausarlo en reposo. Puede reducir microtirones a cambio de un poco más de consumo.

> ⚠️ Modifica la configuración de arranque (bcdedit). Si BitLocker está activo, Rayo lo suspende durante un reinicio para que no te pida la clave de recuperación.

- `Arranque (bcdedit /set {current}): disabledynamictick = yes`

## GPU

### Desactivar telemetría de NVIDIA 
`gpu.telemetry` · Riesgo: **Seguro** · Reversible · Anticheat: 🟢 No toca el juego

Bloquea el envío de datos de uso del driver y del Panel de control de NVIDIA. Es un ajuste de privacidad: no mejora los FPS. Los drivers nuevos ya no instalan el servicio NvTelemetryContainer; si no existe, Rayo solo ajusta las claves.

- `HKLM\SOFTWARE\NVIDIA Corporation\NvControlPanel2\Client\OptInOrOutPreference = 0 [DWORD]`
- `HKLM\SOFTWARE\NVIDIA Corporation\Global\FTS\EnableRID44231 = 0 [DWORD]`
- `HKLM\SOFTWARE\NVIDIA Corporation\Global\FTS\EnableRID64640 = 0 [DWORD]`
- `HKLM\SOFTWARE\NVIDIA Corporation\Global\FTS\EnableRID66610 = 0 [DWORD]`
- `Servicio NvTelemetryContainer → Deshabilitado`

### G-SYNC / VRR en juegos en ventana ⭐
`gpu.vrr` · Riesgo: **Seguro** · Reversible · Anticheat: 🟢 No toca el juego

Permite que la frecuencia variable de tu monitor funcione también en juegos sin bordes que no la soportan de forma nativa. Sin tearing y sin el retraso de V-Sync.

- `HKCU\Software\Microsoft\DirectX\UserGpuPreferences\DirectXUserGlobalSettings → VRROptimizeEnable=1;`

### Interrupciones MSI para la GPU 🔁
`gpu.msi` · Riesgo: **Avanzado** · Reversible · Anticheat: 🟡 Sensible con anticheats

Activa Message Signaled Interrupts en tu GPU: interrupciones directas a la CPU, menor latencia de DPC y menos tirones. Muchos drivers ya lo usan; esto lo asegura.

> ⚠️ Modifica la configuración del dispositivo PCI de la GPU. Es seguro en GPUs NVIDIA modernas, pero si notaras pantallas negras al arrancar, revierte este cambio.

- `HKLM\SYSTEM\CurrentControlSet\Enum\PCI\<tu GPU>\Device Parameters\Interrupt Management\MessageSignaledInterruptProperties\MSISupported = 1 [DWORD]`

### NVIDIA: modo de baja latencia ⭐
`gpu.nvlatency` · Riesgo: **Seguro** · Reversible · Anticheat: 🟢 No toca el juego

Activa el «Modo de baja latencia» (On) del Panel de NVIDIA: la CPU prepara un solo cuadro por adelantado. Solo actúa en juegos DirectX 9 a 11 y solo ayuda si la GPU va al límite; en juegos con NVIDIA Reflex, activa Reflex dentro del juego, que manda sobre este ajuste.

- `Panel de control de NVIDIA (perfil global): Modo de baja latencia (cuadros previamente renderizados) → Activado (1)`

### NVIDIA: preferir rendimiento máximo 🎯 ⚡
`gpu.nvpower` · Riesgo: **Seguro** · Reversible · Anticheat: 🟢 No toca el juego

Tu GPU mantiene sus relojes altos mientras juegas en vez de bajarlos entre escenas. Evita bajones de FPS momentáneos; solo afecta a juegos y apps 3D.

- `Panel de control de NVIDIA (perfil global): Modo de control de energía → Preferir rendimiento máximo`

### NVIDIA: filtrado de texturas en alto rendimiento 🎯
`gpu.nvtexture` · Riesgo: **Seguro** · Reversible · Anticheat: 🟢 No toca el juego

Usa el filtrado de texturas más rápido del driver. Gana algunos FPS a cambio de una diferencia de calidad casi imperceptible.

- `Panel de control de NVIDIA (perfil global): Filtrado de texturas - Calidad → Alto rendimiento`

### NVIDIA: caché de sombreadores amplia 🎯
`gpu.nvshader` · Riesgo: **Seguro** · Reversible · Anticheat: 🟢 No toca el juego

Activa la caché de sombreadores del driver con al menos 10 GB. Los juegos no recompilan sombreadores en cada partida: menos tirones al cargar mapas.

- `Panel de control de NVIDIA (perfil global): Caché del sombreador → Activada`
- `Panel de control de NVIDIA (perfil global): Tamaño máximo de la caché del sombreador → 10 GB (o más si ya era mayor)`

### Desactivar ULPS en Radeon 🔁 ⚡
`gpu.amdulps` · Riesgo: **Moderado** · Reversible · Anticheat: 🟢 No toca el juego

ULPS apaga partes de tu GPU en reposo para ahorrar energía; al volver a usarla puede causar tirones o pantallas negras. Desactivarlo mantiene la GPU lista.

- `HKLM\SYSTEM\CurrentControlSet\Control\Class\{4d36e968-...}\<tu Radeon>\EnableUlps = 0 [DWORD]`

### Impedir que Windows Update cambie tus drivers 
`gpu.wudrivers` · Riesgo: **Moderado** · Reversible · Anticheat: 🟢 No toca el juego

Evita que Windows Update reemplace el driver de NVIDIA por uno genérico o más antiguo. Actualiza la GPU desde NVIDIA App. Afecta a todos los drivers del PC.

- `HKLM\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate\ExcludeWUDriversInQualityUpdate = 1 [DWORD]`

### Desactivar Multiplane Overlay (MPO) 🔁
`gpu.mpo` · Riesgo: **Avanzado** · Reversible · Anticheat: 🟢 No toca el juego

Soluciona parpadeos, pantallazos negros al cambiar de ventana y tirones con varios monitores o con monitores de distinta frecuencia, sobre todo en NVIDIA.

> ⚠️ Úsalo solo si tienes esos problemas: en PCs sin fallos no mejora los FPS y en algunos puede subir un poco la latencia del escritorio.

- `HKLM\SOFTWARE\Microsoft\Windows\Dwm\OverlayTestMode = 5 [DWORD]`

## CPU y Energía

### Plan de energía Máximo rendimiento ⭐ ⚡
`pwr.ultimate` · Riesgo: **Seguro** · Reversible · Anticheat: 🟢 No toca el juego

Crea y activa el plan oculto «Máximo rendimiento» de Windows: tu CPU sube de frecuencia sin demoras y sin aparcar núcleos. Ideal para PC de escritorio.

- `Plan de energía: duplicar 'Máximo rendimiento' (Ultimate Performance) como 'Rayo Máximo Rendimiento' y activarlo`

### Modo de energía «Mejor rendimiento» (portátil) ⭐
`pwr.overlay` · Riesgo: **Seguro** · Reversible · Anticheat: 🟢 No toca el juego

En portátiles, el plan Máximo rendimiento gasta la batería. Este es el modo recomendado por Windows para jugar con el cargador conectado: más rendimiento sin cambiar de plan.

- `Configuración → Sistema → Energía → Modo de energía: Mejor rendimiento`

### Desactivar aparcamiento de núcleos 
`pwr.coreparking` · Riesgo: **Moderado** · Reversible · Anticheat: 🟢 No toca el juego

Mantiene todos los núcleos siempre despiertos para evitar la latencia de reactivarlos. El beneficio es pequeño o nulo en la mayoría de equipos y sube el consumo y la temperatura. No se ofrece en portátiles ni en Ryzen X3D de dos chiplets (7900X3D, 7950X3D, 9900X3D, 9950X3D).

> ⚠️ En Ryzen X3D de dos chiplets desarma la lógica que manda el juego al chiplet con V-Cache y puede bajar los FPS. En portátiles gasta batería.

- `Plan de energía activo: Mínimo de núcleos sin aparcar → 100 %`

### Desactivar Power Throttling 🎯 ⚡
`pwr.throttling` · Riesgo: **Seguro** · Reversible · Anticheat: 🟢 No toca el juego

Impide que Windows limite la potencia de procesos en segundo plano (Discord, OBS, overlays) con un ahorro de energía pensado para portátiles.

- `HKLM\SYSTEM\CurrentControlSet\Control\Power\PowerThrottling\PowerThrottlingOff = 1 [DWORD]`

### Desactivar suspensión selectiva de USB ⭐
`pwr.usb` · Riesgo: **Seguro** · Reversible · Anticheat: 🟢 No toca el juego

Evita que Windows apague puertos USB inactivos: previene microcortes del mouse, teclado o headset.

- `Plan de energía activo: Suspensión selectiva de USB → Deshabilitada`

### Desactivar ahorro de energía PCI Express ⭐ ⚡
`pwr.aspm` · Riesgo: **Seguro** · Reversible · Anticheat: 🟢 No toca el juego

Mantiene activo el enlace PCIe de tu GPU y tus SSD (ASPM apagado), eliminando la latencia de salir de estados de bajo consumo.

- `Plan de energía activo: Administración de energía del estado de vínculo → Desactivada`

### No apagar los discos duros 🎯
`pwr.diskidle` · Riesgo: **Seguro** · Reversible · Anticheat: 🟢 No toca el juego

Evita que los discos mecánicos se detengan por inactividad y tarden segundos en volver a girar cuando un juego carga datos.

- `Plan de energía activo: Apagar el disco duro tras → Nunca`

### Desactivar hibernación e Inicio rápido 
`pwr.hibernate` · Riesgo: **Moderado** · Reversible · Anticheat: 🟢 No toca el juego

Elimina hiberfil.sys (≈40 % de tu RAM ocupando el disco C:) y hace que «Apagar» reinicie el kernel por completo, evitando errores de drivers acumulados.

> ⚠️ La opción Hibernar desaparece del menú de apagado.

- `powercfg /hibernate off (borra hiberfil.sys y desactiva Inicio rápido)`

## Registro

### Prioridad multimedia máxima para juegos 🎯
`reg.mmcss` · Riesgo: **Seguro** · Reversible · Anticheat: 🟡 Ajusta el proceso del juego

Configura el programador multimedia (MMCSS) para dar prioridad alta de CPU, GPU y disco a las tareas de juego. Solo afecta a programas que se registran en MMCSS; muchos juegos no lo hacen, así que el efecto suele ser nulo.

- `HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Multimedia\SystemProfile\Tasks\Games\GPU Priority = 8 [DWORD]`
- `HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Multimedia\SystemProfile\Tasks\Games\Priority = 6 [DWORD]`
- `HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Multimedia\SystemProfile\Tasks\Games\Scheduling Category = "High" [REG_SZ]`
- `HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Multimedia\SystemProfile\Tasks\Games\SFIO Priority = "High" [REG_SZ]`

### Reducir la reserva de CPU multimedia 🎯
`reg.sysresp` · Riesgo: **Seguro** · Reversible · Anticheat: 🟡 Ajusta el proceso del juego

Reduce del 20 % al 10 % la CPU que el programador multimedia reserva para tareas de baja prioridad (SystemResponsiveness). Solo afecta a programas que usan ese programador; el efecto en juegos suele ser pequeño.

- `HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Multimedia\SystemProfile\SystemResponsiveness = 10 (0xA) [DWORD]`

### Priorizar el programa en primer plano 
`reg.priosep` · Riesgo: **Seguro** · Reversible · Anticheat: 🟡 Ajusta el proceso del juego

Fija Win32PrioritySeparation en 0x26 (intervalos cortos y variables, impulso 3:1 para la ventana activa). Es casi igual al comportamiento por defecto de Windows en cliente, así que en la mayoría de equipos no cambia nada.

- `HKLM\SYSTEM\CurrentControlSet\Control\PriorityControl\Win32PrioritySeparation = 38 (0x26) [DWORD]`

### Desactivar Fault Tolerant Heap 🎯
`reg.fth` · Riesgo: **Moderado** · Reversible · Anticheat: 🟢 No toca el juego

Windows aplica parches de compatibilidad lentos a los programas que se cierran inesperadamente. Así un juego que crasheó una vez no queda «penalizado».

- `HKLM\SOFTWARE\Microsoft\FTH\Enabled = 0 [DWORD]`

### Menús instantáneos ⭐ 🚪
`reg.menudelay` · Riesgo: **Seguro** · Reversible · Anticheat: 🟢 No toca el juego

Elimina el retardo de 400 ms al abrir los submenús de Windows.

- `HKCU\Control Panel\Desktop\MenuShowDelay = "0" [REG_SZ]`

### Sin retraso para las apps de inicio ⭐
`reg.startupdelay` · Riesgo: **Seguro** · Reversible · Anticheat: 🟢 No toca el juego

Windows espera unos segundos antes de abrir las apps de inicio. Esto elimina esa espera artificial.

- `HKCU\Software\Microsoft\Windows\CurrentVersion\Explorer\Serialize\StartupDelayInMSec = 0 [DWORD]`
- `HKCU\Software\Microsoft\Windows\CurrentVersion\Explorer\Serialize\WaitForIdleState = 0 [DWORD]`

### Apagado más rápido ⭐
`reg.waitkill` · Riesgo: **Seguro** · Reversible · Anticheat: 🟢 No toca el juego

Reduce de 5 a 2 segundos el tiempo que Windows espera a que los servicios se cierren al apagar o reiniciar.

- `HKLM\SYSTEM\CurrentControlSet\Control\WaitToKillServiceTimeout = "2000" [REG_SZ]`

### Reducir animaciones de Windows 🎯 🚪
`reg.animations` · Riesgo: **Seguro** · Reversible · Anticheat: 🟢 No toca el juego

Desactiva las animaciones al minimizar y maximizar, las de la barra de tareas y Aero Peek. Windows se siente más ágil.

- `HKCU\Control Panel\Desktop\WindowMetrics\MinAnimate = "0" [REG_SZ]`
- `HKCU\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced\TaskbarAnimations = 0 [DWORD]`
- `HKCU\Software\Microsoft\Windows\DWM\EnableAeroPeek = 0 [DWORD]`

### Desactivar efectos de transparencia 🎯
`reg.transparency` · Riesgo: **Seguro** · Reversible · Anticheat: 🟢 No toca el juego

Quita el desenfoque translúcido de la barra de tareas, el menú Inicio y las ventanas. Ahorra un poco de GPU y memoria de video.

- `HKCU\Software\Microsoft\Windows\CurrentVersion\Themes\Personalize\EnableTransparency = 0 [DWORD]`

### Desactivar detección automática de carpetas ⭐ 🚪
`reg.folderdiscovery` · Riesgo: **Seguro** · Reversible · Anticheat: 🟢 No toca el juego

El Explorador deja de revisar el contenido de cada carpeta para adivinar si es de fotos, música o documentos. Las carpetas grandes (como las de tus juegos) se abren al instante.

- `HKCU\Software\Classes\Local Settings\Software\Microsoft\Windows\Shell\Bags\AllFolders\Shell\FolderType = "NotSpecified" [REG_SZ]`

### Efectos visuales: mejor rendimiento 🎯 🚪
`reg.visualfx` · Riesgo: **Seguro** · Reversible · Anticheat: 🟢 No toca el juego

Como «Ajustar para obtener el mejor rendimiento» de Windows, pero conservando las fuentes suavizadas y las miniaturas: sin arrastre de ventanas con contenido, sombras ni selección translúcida.

- `HKCU\Software\Microsoft\Windows\CurrentVersion\Explorer\VisualEffects\VisualFXSetting = 3 [DWORD]`
- `HKCU\Control Panel\Desktop\DragFullWindows = "0" [REG_SZ]`
- `HKCU\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced\ListviewAlphaSelect = 0 [DWORD]`
- `HKCU\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced\ListviewShadow = 0 [DWORD]`

## Red y Ping

### Desactivar limitación de red multimedia 
`net.throttle` · Riesgo: **Seguro** · Reversible · Anticheat: 🟡 Ajusta el proceso del juego

Desactiva el límite de procesamiento de paquetes que el programador multimedia aplica mientras suena audio o video (NetworkThrottlingIndex). Microsoft no documenta este valor y no hay pruebas de que mejore el ping en juegos.

- `HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Multimedia\SystemProfile\NetworkThrottlingIndex = 4294967295 (0xFFFFFFFF) [DWORD]`

### Desactivar algoritmo de Nagle 🎯
`net.nagle` · Riesgo: **Seguro** · Reversible · Anticheat: 🟢 No toca el juego

Envía los paquetes TCP al instante en lugar de agruparlos. Solo ayuda en juegos y servicios que usan TCP (MMO, LoL); los shooters con UDP no cambian. Microsoft solo documenta TcpAckFrequency; TCPNoDelay viene de guías de terceros.

- `Registro: TcpAckFrequency=1, TCPNoDelay=1, TcpDelAckTicks=0 en cada interfaz de red activa (HKLM\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters\Interfaces\{GUID})`

### Desactivar actualizaciones P2P ⭐
`net.delivery` · Riesgo: **Seguro** · Reversible · Anticheat: 🟢 No toca el juego

Impide que tu PC suba actualizaciones de Windows a otros equipos por Internet (Optimización de distribución), liberando ancho de banda de subida.

- `HKLM\SOFTWARE\Policies\Microsoft\Windows\DeliveryOptimization\DODownloadMode = 0 [DWORD]`

### Desactivar Ethernet de bajo consumo ⭐ ⚡
`net.eee` · Riesgo: **Seguro** · Reversible · Anticheat: 🟢 No toca el juego

Apaga Green Ethernet, EEE, Gigabit Lite y el modo de ahorro del adaptador, que pueden causar picos de ping y microcortes. La red se reinicia unos segundos al aplicar.

- `Adaptador de red: Green Ethernet, EEE, Advanced EEE, Gigabit Lite y Power Saving Mode → Desactivado`

### Desactivar moderación de interrupciones ⚡
`net.intmod` · Riesgo: **Avanzado** · Reversible · Anticheat: 🟢 No toca el juego

El adaptador avisa a la CPU por cada paquete en lugar de agruparlos: la latencia de red más baja posible a cambio de algo más de uso de CPU.

> ⚠️ Aumenta las interrupciones de CPU. Con descargas grandes (Steam) el uso de CPU puede subir.

- `Adaptador de red: Moderación de interrupciones → Desactivada`

### DNS rápido de Cloudflare (1.1.1.1) 
`net.dns` · Riesgo: **Moderado** · Reversible · Anticheat: 🟢 No toca el juego

Usa los DNS 1.1.1.1 y 1.0.0.1, que suelen responder antes que los del proveedor. Acelera la conexión a servidores y webs; no cambia el ping dentro de la partida. Tus consultas DNS pasan a Cloudflare.

- `DNS IPv4 de los adaptadores activos → 1.1.1.1, 1.0.0.1 (se guardan los DNS actuales)`

### Desactivar Teredo y preferir IPv4 🔁
`net.teredo` · Riesgo: **Avanzado** · Reversible · Anticheat: 🟢 No toca el juego

Apaga los túneles IPv6 (Teredo, 6to4 e ISATAP) y hace que Windows prefiera IPv4. Algunas redes resuelven antes por IPv4 y los túneles agregan latencia.

> ⚠️ Algunos juegos de Xbox / Game Pass (Halo, Forza, Sea of Thieves) usan Teredo para el chat de grupo y el multijugador: si dejan de conectar, revierte este ajuste.

- `HKLM\SYSTEM\CurrentControlSet\Services\Tcpip6\Parameters\DisabledComponents = 33 (0x21) [DWORD]`

## Mouse y Teclado

### Desactivar aceleración del mouse ⭐
`input.mouseaccel` · Riesgo: **Seguro** · Reversible · Anticheat: 🟢 No toca el juego

Apaga «Mejorar precisión del puntero»: el cursor se mueve 1:1 con tu mano sin importar la velocidad. Imprescindible para apuntar con precisión.

- `Aplicar al instante (SystemParametersInfo SPI_SETMOUSE)`
- `HKCU\Control Panel\Mouse\MouseSpeed = "0" [REG_SZ]`
- `HKCU\Control Panel\Mouse\MouseThreshold1 = "0" [REG_SZ]`
- `HKCU\Control Panel\Mouse\MouseThreshold2 = "0" [REG_SZ]`

### Desactivar atajo de Teclas especiales ⭐ 🚪
`input.stickykeys` · Riesgo: **Seguro** · Reversible · Anticheat: 🟢 No toca el juego

Evita que pulsar Shift cinco veces (o mantenerlo) abra la ventana de Teclas especiales y te saque del juego.

- `HKCU\Control Panel\Accessibility\StickyKeys\Flags = "506" [REG_SZ]`
- `HKCU\Control Panel\Accessibility\Keyboard Response\Flags = "122" [REG_SZ]`
- `HKCU\Control Panel\Accessibility\ToggleKeys\Flags = "58" [REG_SZ]`

### Repetición de teclas más rápida 🎯
`input.keyrepeat` · Riesgo: **Seguro** · Reversible · Anticheat: 🟢 No toca el juego

Reduce al mínimo el retardo antes de que una tecla mantenida empiece a repetirse y usa la velocidad máxima de repetición.

- `Aplicar al instante (SystemParametersInfo)`
- `HKCU\Control Panel\Keyboard\KeyboardDelay = "0" [REG_SZ]`
- `HKCU\Control Panel\Keyboard\KeyboardSpeed = "31" [REG_SZ]`

## Memoria y Disco

### Mantener el kernel en la RAM 🎯 🔁
`mem.pagingexec` · Riesgo: **Seguro** · Reversible · Anticheat: 🟢 No toca el juego

Con tu RAM de memoria, Windows no necesita mover los controladores ni el núcleo del sistema al archivo de paginación (DisablePagingExecutive).

- `HKLM\SYSTEM\CurrentControlSet\Control\Session Manager\Memory Management\DisablePagingExecutive = 1 [DWORD]`

### Agrupar procesos de servicio (svchost) 🎯 🔁
`mem.svchost` · Riesgo: **Moderado** · Reversible · Anticheat: 🟢 No toca el juego

Con tu RAM, Windows separa cada servicio en su propio svchost.exe (80+ procesos). Agruparlos reduce la cantidad de procesos, pero no garantiza que se use menos memoria, y un servicio que falla puede arrastrar a otros del mismo proceso.

- `HKLM\SYSTEM\CurrentControlSet\Control\SvcHostSplitThresholdInKB = 3670016 (0x380000) [DWORD]`

### Desactivar compresión de memoria 🎯 🔁
`mem.compression` · Riesgo: **Moderado** · Reversible · Anticheat: 🟢 No toca el juego

Windows deja de gastar CPU comprimiendo y descomprimiendo páginas de RAM en segundo plano. No hay mediciones que muestren más FPS, así que es opcional.

> ⚠️ Depende del servicio SysMain: si lo desactivas, la compresión también queda desactivada.

- `Disable-MMAgent -MemoryCompression`

### Asegurar TRIM en los SSD ⭐
`mem.trim` · Riesgo: **Seguro** · Reversible · Anticheat: 🟢 No toca el juego

Confirma que Windows envía comandos TRIM a tus SSD para mantener su velocidad de escritura con el paso del tiempo.

- `HKLM\SYSTEM\CurrentControlSet\Control\FileSystem\DisableDeleteNotification = 0 [DWORD]`

### Desactivar marca de último acceso (NTFS) 🎯
`mem.lastaccess` · Riesgo: **Seguro** · Reversible · Anticheat: 🟢 No toca el juego

NTFS deja de escribir la fecha de último acceso cada vez que se lee un archivo: menos escrituras al cargar juegos con miles de archivos. La ganancia es mínima en la mayoría de equipos.

- `HKLM\SYSTEM\CurrentControlSet\Control\FileSystem\NtfsDisableLastAccessUpdate = 2147483649 (0x80000001) [DWORD]`

### Desactivar nombres cortos 8.3 🎯
`mem.8dot3` · Riesgo: **Seguro** · Reversible · Anticheat: 🟢 No toca el juego

Deja de crear nombres estilo MS-DOS (PROGRA~1) para cada archivo nuevo, lo que acelera la creación de archivos en carpetas grandes.

- `HKLM\SYSTEM\CurrentControlSet\Control\FileSystem\NtfsDisable8dot3NameCreation = 1 [DWORD]`

### Desactivar apps de la Store en segundo plano 🎯
`reg.bgapps` · Riesgo: **Moderado** · Reversible · Anticheat: 🟢 No toca el juego

Las apps de Microsoft Store (Tiempo, Noticias, Vincular teléfono…) dejan de ejecutarse cuando no las tienes abiertas: menos RAM, CPU y red mientras juegas.

> ⚠️ Esas apps no actualizan sus avisos en segundo plano (por ejemplo, notificaciones de Vincular teléfono o del Correo de la Store).

- `HKCU\Software\Microsoft\Windows\CurrentVersion\BackgroundAccessApplications\GlobalUserDisabled = 1 [DWORD]`
- `HKLM\SOFTWARE\Policies\Microsoft\Windows\AppPrivacy\LetAppsRunInBackground = 2 [DWORD]`

## Calidad de vida

### Mostrar extensiones de archivo ⭐
`qol.ext` · Riesgo: **Seguro** · Reversible · Anticheat: 🟢 No toca el juego

Muestra .exe, .zip, .pdf, etc. Más claro y más seguro: detectas archivos maliciosos disfrazados como «foto.jpg.exe».

- `HKCU\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced\HideFileExt = 0 [DWORD]`

### Menú contextual clásico 
`qol.contextmenu` · Riesgo: **Seguro** · Reversible · Anticheat: 🟢 No toca el juego

Recupera el menú completo del clic derecho, sin el paso extra «Mostrar más opciones». Se ve tras reiniciar el Explorador (Limpieza → Reiniciar Explorador).

- `Crear clave HKCU\Software\Classes\CLSID\{86ca1aa0-34aa-4e8b-a509-50c905bae2a2}\InprocServer32 con valor predeterminado vacío`

### «Finalizar tarea» en la barra de tareas ⭐
`qol.endtask` · Riesgo: **Seguro** · Reversible · Anticheat: 🟢 No toca el juego

Añade Finalizar tarea al clic derecho de la barra de tareas: cierra un juego congelado sin abrir el Administrador de tareas.

- `HKCU\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced\TaskbarDeveloperSettings\TaskbarEndTask = 1 [DWORD]`

### Abrir el Explorador en «Este equipo» 
`qol.thispc` · Riesgo: **Seguro** · Reversible · Anticheat: 🟢 No toca el juego

El Explorador abre directamente tus unidades (C:, D:, E:) en lugar de Inicio / Acceso rápido.

- `HKCU\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced\LaunchTo = 1 [DWORD]`

### Mostrar información detallada al iniciar 
`qol.verbose` · Riesgo: **Seguro** · Reversible · Anticheat: 🟢 No toca el juego

Muestra qué está haciendo Windows al arrancar y apagar. No lo acelera, pero ayuda a descubrir qué lo hace lento.

- `HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System\VerboseStatus = 1 [DWORD]`

## Aceleración en apps

### Desactivar aceleración por hardware en Discord 
`accel.discord` · Riesgo: **Seguro** · Reversible · Anticheat: 🟢 No toca el juego

Desactiva «Aceleración por hardware» en Discord (también PTB y Canary). Libera la GPU (y su memoria) para el juego mientras la app queda abierta de fondo; la app usa un poco más de CPU. Útil sobre todo con GPU integrada o de gama de entrada.

> ⚠️ Cierra Discord por completo (también desde la bandeja) antes de aplicar.

- `Discord: enableHardwareAcceleration = false (settings.json)`
- `Discord PTB: enableHardwareAcceleration = false (settings.json)`
- `Discord Canary: enableHardwareAcceleration = false (settings.json)`

### Desactivar aceleración por hardware en Chrome 
`accel.chrome` · Riesgo: **Seguro** · Reversible · Anticheat: 🟢 No toca el juego

Desactiva la aceleración por hardware de Google Chrome (política del navegador; Chrome dirá «Administrado por tu organización»). Libera la GPU (y su memoria) para el juego mientras la app queda abierta de fondo; la app usa un poco más de CPU. Útil sobre todo con GPU integrada o de gama de entrada.

- `HKLM\SOFTWARE\Policies\Google\Chrome\HardwareAccelerationModeEnabled = 0 [DWORD]`

### Desactivar aceleración por hardware en Edge 
`accel.edge` · Riesgo: **Seguro** · Reversible · Anticheat: 🟢 No toca el juego

Desactiva la aceleración por hardware de Microsoft Edge (política del navegador; Edge dirá «Administrado por tu organización»). Libera la GPU (y su memoria) para el juego mientras la app queda abierta de fondo; la app usa un poco más de CPU. Útil sobre todo con GPU integrada o de gama de entrada.

- `HKLM\SOFTWARE\Policies\Microsoft\Edge\HardwareAccelerationModeEnabled = 0 [DWORD]`

### Desactivar aceleración por hardware en Brave 
`accel.brave` · Riesgo: **Seguro** · Reversible · Anticheat: 🟢 No toca el juego

Desactiva la aceleración por hardware del navegador Brave (política del navegador). Libera la GPU (y su memoria) para el juego mientras la app queda abierta de fondo; la app usa un poco más de CPU. Útil sobre todo con GPU integrada o de gama de entrada.

- `HKLM\SOFTWARE\Policies\BraveSoftware\Brave\HardwareAccelerationModeEnabled = 0 [DWORD]`

### Desactivar aceleración por hardware en Firefox 
`accel.firefox` · Riesgo: **Seguro** · Reversible · Anticheat: 🟢 No toca el juego

Desactiva la aceleración por hardware de Firefox (política del navegador). Libera la GPU (y su memoria) para el juego mientras la app queda abierta de fondo; la app usa un poco más de CPU. Útil sobre todo con GPU integrada o de gama de entrada.

- `HKLM\SOFTWARE\Policies\Mozilla\Firefox\HardwareAcceleration = 0 [DWORD]`

### Desactivar aceleración por hardware en Opera / Opera GX 
`accel.opera` · Riesgo: **Seguro** · Reversible · Anticheat: 🟢 No toca el juego

Desactiva «Usar aceleración por hardware» en Opera y Opera GX. Libera la GPU (y su memoria) para el juego mientras la app queda abierta de fondo; la app usa un poco más de CPU. Útil sobre todo con GPU integrada o de gama de entrada.

> ⚠️ Cierra Opera antes de aplicar.

- `Opera GX: hardware_acceleration_mode.enabled = false (Local State)`
- `Opera: hardware_acceleration_mode.enabled = false (Local State)`

### Desactivar aceleración por hardware en Spotify 
`accel.spotify` · Riesgo: **Seguro** · Reversible · Anticheat: 🟢 No toca el juego

Desactiva «Aceleración por hardware» en Spotify (versión de escritorio y de Microsoft Store). Libera la GPU (y su memoria) para el juego mientras la app queda abierta de fondo; la app usa un poco más de CPU. Útil sobre todo con GPU integrada o de gama de entrada.

> ⚠️ Cierra Spotify antes de aplicar.

- `Spotify: ui.hardware_acceleration=false (prefs)`
- `Spotify: ui.hardware_acceleration=false (prefs)`

## Privacidad

### Bloquear apps de fabricantes al conectar dispositivos 
`priv.devicesoftware` · Riesgo: **Seguro** · Reversible · Anticheat: 🟢 No toca el juego

Al conectar un mouse, teclado o auriculares, Windows deja de descargar sola la app del fabricante (Razer, Logitech, etc.) y su publicidad. Los drivers siguen instalándose normalmente.

- `HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Device Metadata\PreventDeviceMetadataFromNetwork = 1 [DWORD]`
- `HKLM\SOFTWARE\Policies\Microsoft\Windows\Device Metadata\PreventDeviceMetadataFromNetwork = 1 [DWORD]`

### Reducir telemetría de Windows ⭐
`priv.telemetry` · Riesgo: **Seguro** · Reversible · Anticheat: 🟢 No toca el juego

Lleva los datos de diagnóstico al mínimo, desactiva el servicio DiagTrack y las encuestas de opinión. Es un ajuste de privacidad: su efecto en el rendimiento no está medido. En Windows Home y Pro, Microsoft trata el nivel 0 como el mínimo («Necesario»).

> ⚠️ Si usas el programa Windows Insider, no la actives: Insider necesita los datos de diagnóstico opcionales para seguir recibiendo compilaciones.

- `HKLM\SOFTWARE\Policies\Microsoft\Windows\DataCollection\AllowTelemetry = 0 [DWORD]`
- `HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\DataCollection\AllowTelemetry = 0 [DWORD]`
- `HKCU\Software\Microsoft\Siuf\Rules\NumberOfSIUFInPeriod = 0 [DWORD]`
- `Servicio DiagTrack → Deshabilitado`
- `Servicio dmwappushservice → Deshabilitado`

### Desactivar tareas programadas de telemetría ⭐
`priv.tasks` · Riesgo: **Seguro** · Reversible · Anticheat: 🟢 No toca el juego

Apaga tareas de telemetría como el Evaluador de compatibilidad de Microsoft, que a veces usa CPU y disco en segundo plano. Es de privacidad: no hay mediciones de FPS.

- `Tarea programada \Microsoft\Windows\Application Experience\Microsoft Compatibility Appraiser → Deshabilitada`
- `Tarea programada \Microsoft\Windows\Application Experience\ProgramDataUpdater → Deshabilitada`
- `Tarea programada \Microsoft\Windows\Customer Experience Improvement Program\Consolidator → Deshabilitada`
- `Tarea programada \Microsoft\Windows\Customer Experience Improvement Program\UsbCeip → Deshabilitada`
- `Tarea programada \Microsoft\Windows\Autochk\Proxy → Deshabilitada`
- `Tarea programada \Microsoft\Windows\DiskDiagnostic\Microsoft-Windows-DiskDiagnosticDataCollector → Deshabilitada`
- `Tarea programada \Microsoft\Windows\Feedback\Siuf\DmClient → Deshabilitada`
- `Tarea programada \Microsoft\Windows\Feedback\Siuf\DmClientOnScenarioDownload → Deshabilitada`
- `Tarea programada \Microsoft\Windows\Windows Error Reporting\QueueReporting → Deshabilitada`
- `Tarea programada \Microsoft\Windows\Maps\MapsUpdateTask → Deshabilitada`
- `Tarea programada \Microsoft\Windows\Maps\MapsToastTask → Deshabilitada`

### Desactivar ID de publicidad ⭐
`priv.adid` · Riesgo: **Seguro** · Reversible · Anticheat: 🟢 No toca el juego

Las apps dejan de usar un identificador para mostrarte anuncios personalizados, y Windows deja de usar tus diagnósticos para «experiencias personalizadas».

- `HKCU\Software\Microsoft\Windows\CurrentVersion\AdvertisingInfo\Enabled = 0 [DWORD]`
- `HKLM\SOFTWARE\Policies\Microsoft\Windows\AdvertisingInfo\DisabledByGroupPolicy = 1 [DWORD]`
- `HKCU\Software\Microsoft\Windows\CurrentVersion\Privacy\TailoredExperiencesWithDiagnosticDataEnabled = 0 [DWORD]`

### Desactivar historial de actividad ⭐
`priv.activity` · Riesgo: **Seguro** · Reversible · Anticheat: 🟢 No toca el juego

Windows deja de registrar y subir a la nube las apps y archivos que abres.

- `HKLM\SOFTWARE\Policies\Microsoft\Windows\System\EnableActivityFeed = 0 [DWORD]`
- `HKLM\SOFTWARE\Policies\Microsoft\Windows\System\PublishUserActivities = 0 [DWORD]`
- `HKLM\SOFTWARE\Policies\Microsoft\Windows\System\UploadUserActivities = 0 [DWORD]`

### Quitar anuncios y sugerencias de Windows ⭐
`priv.ads` · Riesgo: **Seguro** · Reversible · Anticheat: 🟢 No toca el juego

Elimina sugerencias del menú Inicio, anuncios en Configuración, consejos, la pantalla «Terminemos de configurar» y la instalación silenciosa de apps promocionadas.

- `HKCU\Software\Microsoft\Windows\CurrentVersion\ContentDeliveryManager\SubscribedContent-338388Enabled = 0 [DWORD]`
- `HKCU\Software\Microsoft\Windows\CurrentVersion\ContentDeliveryManager\SubscribedContent-338389Enabled = 0 [DWORD]`
- `HKCU\Software\Microsoft\Windows\CurrentVersion\ContentDeliveryManager\SubscribedContent-338393Enabled = 0 [DWORD]`
- `HKCU\Software\Microsoft\Windows\CurrentVersion\ContentDeliveryManager\SubscribedContent-353694Enabled = 0 [DWORD]`
- `HKCU\Software\Microsoft\Windows\CurrentVersion\ContentDeliveryManager\SubscribedContent-353696Enabled = 0 [DWORD]`
- `HKCU\Software\Microsoft\Windows\CurrentVersion\ContentDeliveryManager\SubscribedContent-310093Enabled = 0 [DWORD]`
- `HKCU\Software\Microsoft\Windows\CurrentVersion\ContentDeliveryManager\SystemPaneSuggestionsEnabled = 0 [DWORD]`
- `HKCU\Software\Microsoft\Windows\CurrentVersion\ContentDeliveryManager\SoftLandingEnabled = 0 [DWORD]`
- `HKCU\Software\Microsoft\Windows\CurrentVersion\ContentDeliveryManager\SilentInstalledAppsEnabled = 0 [DWORD]`
- `HKCU\Software\Microsoft\Windows\CurrentVersion\ContentDeliveryManager\OemPreInstalledAppsEnabled = 0 [DWORD]`
- `HKCU\Software\Microsoft\Windows\CurrentVersion\ContentDeliveryManager\PreInstalledAppsEnabled = 0 [DWORD]`
- `HKCU\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced\Start_IrisRecommendations = 0 [DWORD]`
- `HKCU\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced\ShowSyncProviderNotifications = 0 [DWORD]`
- `HKCU\Software\Microsoft\Windows\CurrentVersion\UserProfileEngagement\ScoobeSystemSettingEnabled = 0 [DWORD]`

### Quitar Bing del menú Inicio ⭐ 🚪
`priv.bing` · Riesgo: **Seguro** · Reversible · Anticheat: 🟢 No toca el juego

Las búsquedas del menú Inicio dejan de enviarse a Bing: resultados locales más rápidos y sin anuncios web.

- `HKCU\Software\Policies\Microsoft\Windows\Explorer\DisableSearchBoxSuggestions = 1 [DWORD]`
- `HKCU\Software\Microsoft\Windows\CurrentVersion\Search\BingSearchEnabled = 0 [DWORD]`
- `HKCU\Software\Microsoft\Windows\CurrentVersion\Search\CortanaConsent = 0 [DWORD]`

### Desactivar Cortana ⭐
`priv.cortana` · Riesgo: **Seguro** · Reversible · Anticheat: 🟢 No toca el juego

Impide que Cortana se ejecute y consuma CPU y disco en segundo plano.

- `HKLM\SOFTWARE\Policies\Microsoft\Windows\Windows Search\AllowCortana = 0 [DWORD]`

### Ocultar Copilot y bloquear Recall ⭐
`priv.copilot` · Riesgo: **Seguro** · Reversible · Anticheat: 🟢 No toca el juego

Oculta el botón de Copilot y aplica las políticas de Windows para Copilot y Recall. La política antigua de Copilot está obsoleta y no desactiva la app nueva de Windows 11 24H2 (puedes quitarla desde la lista de Apps, «Copilot»). Recall solo existe en equipos Copilot+.

- `HKCU\Software\Policies\Microsoft\Windows\WindowsCopilot\TurnOffWindowsCopilot = 1 [DWORD]`
- `HKLM\SOFTWARE\Policies\Microsoft\Windows\WindowsCopilot\TurnOffWindowsCopilot = 1 [DWORD]`
- `HKCU\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced\ShowCopilotButton = 0 [DWORD]`
- `HKLM\SOFTWARE\Policies\Microsoft\Windows\WindowsAI\DisableAIDataAnalysis = 1 [DWORD]`
- `HKLM\SOFTWARE\Policies\Microsoft\Windows\WindowsAI\AllowRecallEnablement = 0 [DWORD]`
- `HKCU\Software\Policies\Microsoft\Windows\WindowsAI\DisableAIDataAnalysis = 1 [DWORD]`

### Desactivar Widgets (Noticias e intereses) ⭐
`priv.widgets` · Riesgo: **Seguro** · Reversible · Anticheat: 🟢 No toca el juego

El panel de Widgets mantiene procesos de Edge WebView2 en segundo plano consumiendo RAM y CPU. Esto lo apaga por completo.

- `HKLM\SOFTWARE\Policies\Microsoft\Dsh\AllowNewsAndInterests = 0 [DWORD]`

### Desactivar Informe de errores de Windows ⭐
`priv.wer` · Riesgo: **Seguro** · Reversible · Anticheat: 🟢 No toca el juego

Windows deja de generar volcados y enviar informes cada vez que un programa falla, lo que ahorra disco y CPU tras un crash.

- `HKLM\SOFTWARE\Microsoft\Windows\Windows Error Reporting\Disabled = 1 [DWORD]`
- `HKLM\SOFTWARE\Policies\Microsoft\Windows\Windows Error Reporting\Disabled = 1 [DWORD]`
- `Servicio WerSvc → Deshabilitado`

### Edge sin procesos en segundo plano ⭐
`priv.edge` · Riesgo: **Seguro** · Reversible · Anticheat: 🟢 No toca el juego

Desactiva el «Aumento de inicio» y la ejecución en segundo plano de Edge: libera 100–300 MB de RAM con el navegador cerrado. Edge dirá «Administrado por tu organización».

- `HKLM\SOFTWARE\Policies\Microsoft\Edge\StartupBoostEnabled = 0 [DWORD]`
- `HKLM\SOFTWARE\Policies\Microsoft\Edge\BackgroundModeEnabled = 0 [DWORD]`

### Chrome sin procesos en segundo plano 
`priv.chrome` · Riesgo: **Seguro** · Reversible · Anticheat: 🟢 No toca el juego

Impide que Google Chrome siga ejecutándose al cerrarlo y desactiva el envío de estadísticas. Chrome dirá «Administrado por tu organización».

- `HKLM\SOFTWARE\Policies\Google\Chrome\BackgroundModeEnabled = 0 [DWORD]`
- `HKLM\SOFTWARE\Policies\Google\Chrome\MetricsReportingEnabled = 0 [DWORD]`

### Desactivar telemetría de Firefox 
`priv.firefox` · Riesgo: **Seguro** · Reversible · Anticheat: 🟢 No toca el juego

Firefox deja de enviar datos de uso y rendimiento a Mozilla y desactiva su agente de navegador predeterminado.

- `HKLM\SOFTWARE\Policies\Mozilla\Firefox\DisableTelemetry = 1 [DWORD]`
- `HKLM\SOFTWARE\Policies\Mozilla\Firefox\DisableDefaultBrowserAgent = 1 [DWORD]`

### Desactivar personalización de escritura y voz 🎯
`priv.inking` · Riesgo: **Seguro** · Reversible · Anticheat: 🟢 No toca el juego

Windows deja de recopilar lo que escribes y dices para «mejorar» el reconocimiento, y se apaga el reconocimiento de voz en línea.

- `HKCU\Software\Microsoft\InputPersonalization\RestrictImplicitInkCollection = 1 [DWORD]`
- `HKCU\Software\Microsoft\InputPersonalization\RestrictImplicitTextCollection = 1 [DWORD]`
- `HKCU\Software\Microsoft\InputPersonalization\TrainedDataStore\HarvestContacts = 0 [DWORD]`
- `HKCU\Software\Microsoft\Personalization\Settings\AcceptedPrivacyPolicy = 0 [DWORD]`
- `HKCU\Software\Microsoft\Speech_OneCore\Settings\OnlineSpeechPrivacy\HasAccepted = 0 [DWORD]`

## Servicios

### Windows Update: modo Seguridad 
`svc.updates` · Riesgo: **Moderado** · Reversible · Anticheat: 🟢 No toca el juego

Los parches de seguridad llegan con 4 días de retraso (los defectuosos se retiran antes de tocar tu PC) y las actualizaciones de versión grandes, con 365 días. Si solo quieres evitar reinicios en mitad de una partida, Microsoft recomienda las «Horas activas» de Windows Update, que no retrasan nada.

> ⚠️ Sigues recibiendo los parches de seguridad. Funciona en Windows Pro, Enterprise y Education: Windows Home ignora estas políticas.

- `HKLM\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate\DeferFeatureUpdates = 1 [DWORD]`
- `HKLM\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate\DeferFeatureUpdatesPeriodInDays = 365 (0x16D) [DWORD]`
- `HKLM\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate\DeferQualityUpdates = 1 [DWORD]`
- `HKLM\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate\DeferQualityUpdatesPeriodInDays = 4 [DWORD]`

### Desactivar servicios innecesarios ⭐
`svc.misc` · Riesgo: **Seguro** · Reversible · Anticheat: 🟢 No toca el juego

Apaga servicios que casi nadie usa en un PC gamer: Fax, Mapas descargados, Modo demo de tienda, Registro remoto, Seguimiento de vínculos y uso compartido de Windows Media.

- `Servicio Fax → Deshabilitado`
- `Servicio MapsBroker → Deshabilitado`
- `Servicio RetailDemo → Deshabilitado`
- `Servicio RemoteRegistry → Deshabilitado`
- `Servicio TrkWks → Deshabilitado`
- `Servicio WMPNetworkSvc → Deshabilitado`

### Desactivar SysMain (Superfetch) 
`svc.sysmain` · Riesgo: **Moderado** · Reversible · Anticheat: 🟢 No toca el juego

Deja de precargar apps en la RAM según tu uso. En SSD aporta poco y puede provocar picos de uso del disco duro. Con tu RAM puedes probar ambas opciones.

> ⚠️ También desactiva la compresión de memoria (depende de este servicio).

- `Servicio SysMain → Deshabilitado`

### Desactivar indexación de búsqueda 
`svc.search` · Riesgo: **Moderado** · Reversible · Anticheat: 🟢 No toca el juego

Windows Search deja de indexar tus archivos en segundo plano (CPU y disco). Buscar archivos será más lento y la búsqueda del menú Inicio puede dejar de funcionar bien.

- `Servicio WSearch → Deshabilitado`

### Desactivar cola de impresión 
`svc.print` · Riesgo: **Moderado** · Reversible · Anticheat: 🟢 No toca el juego

Si no tienes impresora, el servicio Spooler no hace falta (y se cierra una vía de ataque conocida). Ojo: «Imprimir a PDF» también lo necesita.

- `Servicio Spooler → Deshabilitado`

### Desactivar servicios de Xbox 
`svc.xbox` · Riesgo: **Avanzado** · Reversible · Anticheat: 🟢 No toca el juego

Si no usas Game Pass, la app de Xbox ni juegos de Microsoft Store con cuenta Xbox, estos tres servicios (inicio de sesión, partidas en la nube y red de Xbox) no hacen falta. No toca XboxGipSvc: apagarlo puede dejar sin funcionar mandos y accesorios Xbox.

> ⚠️ Forza, Halo, Minecraft Bedrock y los juegos de Game Pass no podrán iniciar sesión hasta que lo reviertas.

- `Servicio XblAuthManager → Deshabilitado`
- `Servicio XblGameSave → Deshabilitado`
- `Servicio XboxNetApiSvc → Deshabilitado`
- `Servicio XboxGipSvc: no se toca (si Rayo lo apagó antes, se restaura al revertir)`

## Apps

### Microsoft Noticias 
`app.microsoft.bingnews` · Riesgo: **Seguro** · No reversible automáticamente · Anticheat: 🟢 No toca el juego

Noticias de MSN que se actualizan en segundo plano.

- `Desinstalar la app Microsoft.BingNews (usuario actual y copia aprovisionada)`

### Búsqueda de Bing 
`app.microsoft.bingsearch` · Riesgo: **Seguro** · No reversible automáticamente · Anticheat: 🟢 No toca el juego

Integración de búsqueda web de Bing.

- `Desinstalar la app Microsoft.BingSearch (usuario actual y copia aprovisionada)`

### El Tiempo (MSN) 
`app.microsoft.bingweather` · Riesgo: **Seguro** · No reversible automáticamente · Anticheat: 🟢 No toca el juego

Pronóstico del tiempo de MSN.

- `Desinstalar la app Microsoft.BingWeather (usuario actual y copia aprovisionada)`

### Obtener ayuda 
`app.microsoft.gethelp` · Riesgo: **Seguro** · No reversible automáticamente · Anticheat: 🟢 No toca el juego

Asistente de soporte de Microsoft.

- `Desinstalar la app Microsoft.GetHelp (usuario actual y copia aprovisionada)`

### Sugerencias 
`app.microsoft.getstarted` · Riesgo: **Seguro** · No reversible automáticamente · Anticheat: 🟢 No toca el juego

Consejos y tutoriales de Windows.

- `Desinstalar la app Microsoft.Getstarted (usuario actual y copia aprovisionada)`

### Solitaire Collection 
`app.microsoft.microsoftsolitairecollection` · Riesgo: **Seguro** · No reversible automáticamente · Anticheat: 🟢 No toca el juego

Juegos de cartas con anuncios.

- `Desinstalar la app Microsoft.MicrosoftSolitaireCollection (usuario actual y copia aprovisionada)`

### Microsoft 365 (Office) 
`app.microsoft.microsoftofficehub` · Riesgo: **Seguro** · No reversible automáticamente · Anticheat: 🟢 No toca el juego

Lanzador de Office que promociona la suscripción.

- `Desinstalar la app Microsoft.MicrosoftOfficeHub (usuario actual y copia aprovisionada)`

### Contactos 
`app.microsoft.people` · Riesgo: **Seguro** · No reversible automáticamente · Anticheat: 🟢 No toca el juego

App de contactos heredada.

- `Desinstalar la app Microsoft.People (usuario actual y copia aprovisionada)`

### Power Automate 
`app.microsoft.powerautomatedesktop` · Riesgo: **Seguro** · No reversible automáticamente · Anticheat: 🟢 No toca el juego

Automatización empresarial.

- `Desinstalar la app Microsoft.PowerAutomateDesktop (usuario actual y copia aprovisionada)`

### Microsoft To Do 
`app.microsoft.todos` · Riesgo: **Seguro** · No reversible automáticamente · Anticheat: 🟢 No toca el juego

Listas de tareas.

- `Desinstalar la app Microsoft.Todos (usuario actual y copia aprovisionada)`

### Centro de opiniones 
`app.microsoft.windowsfeedbackhub` · Riesgo: **Seguro** · No reversible automáticamente · Anticheat: 🟢 No toca el juego

Envío de comentarios a Microsoft (útil si eres Windows Insider).

- `Desinstalar la app Microsoft.WindowsFeedbackHub (usuario actual y copia aprovisionada)`

### Mapas 
`app.microsoft.windowsmaps` · Riesgo: **Seguro** · No reversible automáticamente · Anticheat: 🟢 No toca el juego

Mapas sin conexión de Windows.

- `Desinstalar la app Microsoft.WindowsMaps (usuario actual y copia aprovisionada)`

### Películas y TV 
`app.microsoft.zunevideo` · Riesgo: **Seguro** · No reversible automáticamente · Anticheat: 🟢 No toca el juego

Reproductor de video heredado.

- `Desinstalar la app Microsoft.ZuneVideo (usuario actual y copia aprovisionada)`

### Reproductor multimedia 
`app.microsoft.zunemusic` · Riesgo: **Moderado** · No reversible automáticamente · Anticheat: 🟢 No toca el juego

Reproductor de música y video predeterminado.

> ⚠️ Es el reproductor predeterminado: necesitarás otro (p. ej. VLC).

- `Desinstalar la app Microsoft.ZuneMusic (usuario actual y copia aprovisionada)`

### Clipchamp 
`app.clipchamp.clipchamp` · Riesgo: **Seguro** · No reversible automáticamente · Anticheat: 🟢 No toca el juego

Editor de video en línea.

- `Desinstalar la app Clipchamp.Clipchamp (usuario actual y copia aprovisionada)`

### Cortana (app) 
`app.microsoft.549981c3f5f10` · Riesgo: **Seguro** · No reversible automáticamente · Anticheat: 🟢 No toca el juego

Asistente de voz descontinuado.

- `Desinstalar la app Microsoft.549981C3F5F10 (usuario actual y copia aprovisionada)`

### Asistencia rápida 
`app.microsoftcorporationii.quickassist` · Riesgo: **Seguro** · No reversible automáticamente · Anticheat: 🟢 No toca el juego

Soporte remoto. Ojo: muy usado en estafas telefónicas.

- `Desinstalar la app MicrosoftCorporationII.QuickAssist (usuario actual y copia aprovisionada)`

### Vínculo móvil 
`app.microsoft.yourphone` · Riesgo: **Seguro** · No reversible automáticamente · Anticheat: 🟢 No toca el juego

Conecta tu celular; se ejecuta en segundo plano.

- `Desinstalar la app Microsoft.YourPhone (usuario actual y copia aprovisionada)`

### Outlook (nuevo) 
`app.microsoft.outlookforwindows` · Riesgo: **Seguro** · No reversible automáticamente · Anticheat: 🟢 No toca el juego

Cliente de correo web.

- `Desinstalar la app Microsoft.OutlookForWindows (usuario actual y copia aprovisionada)`

### Correo y Calendario 
`app.microsoft.windowscommunicationsapps` · Riesgo: **Seguro** · No reversible automáticamente · Anticheat: 🟢 No toca el juego

Apps de correo heredadas.

- `Desinstalar la app microsoft.windowscommunicationsapps (usuario actual y copia aprovisionada)`

### Microsoft Teams 
`app.msteams` · Riesgo: **Seguro** · No reversible automáticamente · Anticheat: 🟢 No toca el juego

Chat de Teams que inicia con Windows.

- `Desinstalar la app MSTeams (usuario actual y copia aprovisionada)`

### Teams (chat personal) 
`app.microsoftteams` · Riesgo: **Seguro** · No reversible automáticamente · Anticheat: 🟢 No toca el juego

Versión de Teams integrada en Windows 11.

- `Desinstalar la app MicrosoftTeams (usuario actual y copia aprovisionada)`

### Dev Home 
`app.microsoft.windows.devhome` · Riesgo: **Seguro** · No reversible automáticamente · Anticheat: 🟢 No toca el juego

Panel para desarrolladores (descontinuado).

- `Desinstalar la app Microsoft.Windows.DevHome (usuario actual y copia aprovisionada)`

### Copilot 
`app.microsoft.copilot` · Riesgo: **Seguro** · No reversible automáticamente · Anticheat: 🟢 No toca el juego

Asistente de IA de Microsoft.

- `Desinstalar la app Microsoft.Copilot (usuario actual y copia aprovisionada)`

### Reloj 
`app.microsoft.windowsalarms` · Riesgo: **Seguro** · No reversible automáticamente · Anticheat: 🟢 No toca el juego

Alarmas, temporizador y cronómetro.

- `Desinstalar la app Microsoft.WindowsAlarms (usuario actual y copia aprovisionada)`

### Grabadora de sonido 
`app.microsoft.windowssoundrecorder` · Riesgo: **Seguro** · No reversible automáticamente · Anticheat: 🟢 No toca el juego

Grabación de audio simple.

- `Desinstalar la app Microsoft.WindowsSoundRecorder (usuario actual y copia aprovisionada)`

### Portal de realidad mixta 
`app.microsoft.mixedreality.portal` · Riesgo: **Seguro** · No reversible automáticamente · Anticheat: 🟢 No toca el juego

Para cascos Windows Mixed Reality.

- `Desinstalar la app Microsoft.MixedReality.Portal (usuario actual y copia aprovisionada)`

### Skype 
`app.microsoft.skypeapp` · Riesgo: **Seguro** · No reversible automáticamente · Anticheat: 🟢 No toca el juego

Servicio descontinuado por Microsoft.

- `Desinstalar la app Microsoft.SkypeApp (usuario actual y copia aprovisionada)`

### Visor 3D 
`app.microsoft.microsoft3dviewer` · Riesgo: **Seguro** · No reversible automáticamente · Anticheat: 🟢 No toca el juego

Visor de modelos 3D.

- `Desinstalar la app Microsoft.Microsoft3DViewer (usuario actual y copia aprovisionada)`

### Xbox: voz a texto 
`app.microsoft.xboxspeechtotextoverlay` · Riesgo: **Seguro** · No reversible automáticamente · Anticheat: 🟢 No toca el juego

Superposición de accesibilidad de Game Bar.

- `Desinstalar la app Microsoft.XboxSpeechToTextOverlay (usuario actual y copia aprovisionada)`

### Xbox Game Bar 
`app.microsoft.xboxgamingoverlay` · Riesgo: **Avanzado** · No reversible automáticamente · Anticheat: 🟢 No toca el juego

Superposición Win+G.

> ⚠️ Win+G dejará de funcionar y algunos juegos no podrán grabar clips con Windows.

- `Desinstalar la app Microsoft.XboxGamingOverlay (usuario actual y copia aprovisionada)`

### OneNote (Windows 10) 
`app.microsoft.office.onenote` · Riesgo: **Seguro** · No reversible automáticamente · Anticheat: 🟢 No toca el juego

Versión antigua de OneNote que viene preinstalada.

- `Desinstalar la app Microsoft.Office.OneNote (usuario actual y copia aprovisionada)`

### Microsoft Whiteboard 
`app.microsoft.whiteboard` · Riesgo: **Seguro** · No reversible automáticamente · Anticheat: 🟢 No toca el juego

Pizarra colaborativa de Microsoft 365.

- `Desinstalar la app Microsoft.Whiteboard (usuario actual y copia aprovisionada)`

### Sway 
`app.microsoft.office.sway` · Riesgo: **Seguro** · No reversible automáticamente · Anticheat: 🟢 No toca el juego

Presentaciones en línea de Office.

- `Desinstalar la app Microsoft.Office.Sway (usuario actual y copia aprovisionada)`

### Planes móviles 
`app.microsoft.oneconnect` · Riesgo: **Seguro** · No reversible automáticamente · Anticheat: 🟢 No toca el juego

Contratación de planes de datos móviles.

- `Desinstalar la app Microsoft.OneConnect (usuario actual y copia aprovisionada)`

### Mensajería 
`app.microsoft.messaging` · Riesgo: **Seguro** · No reversible automáticamente · Anticheat: 🟢 No toca el juego

App de mensajes heredada de Windows 10.

- `Desinstalar la app Microsoft.Messaging (usuario actual y copia aprovisionada)`

### Print 3D 
`app.microsoft.print3d` · Riesgo: **Seguro** · No reversible automáticamente · Anticheat: 🟢 No toca el juego

Preparación de modelos para impresoras 3D.

- `Desinstalar la app Microsoft.Print3D (usuario actual y copia aprovisionada)`

### 3D Builder 
`app.microsoft.3dbuilder` · Riesgo: **Seguro** · No reversible automáticamente · Anticheat: 🟢 No toca el juego

Editor de modelos 3D heredado.

- `Desinstalar la app Microsoft.3DBuilder (usuario actual y copia aprovisionada)`

### Microsoft Wallet 
`app.microsoft.wallet` · Riesgo: **Seguro** · No reversible automáticamente · Anticheat: 🟢 No toca el juego

Cartera de pagos descontinuada.

- `Desinstalar la app Microsoft.Wallet (usuario actual y copia aprovisionada)`

### Test de velocidad de red 
`app.microsoft.networkspeedtest` · Riesgo: **Seguro** · No reversible automáticamente · Anticheat: 🟢 No toca el juego

Medidor de velocidad heredado.

- `Desinstalar la app Microsoft.NetworkSpeedTest (usuario actual y copia aprovisionada)`

### Finanzas (MSN) 
`app.microsoft.bingfinance` · Riesgo: **Seguro** · No reversible automáticamente · Anticheat: 🟢 No toca el juego

Cotizaciones y noticias financieras de MSN.

- `Desinstalar la app Microsoft.BingFinance (usuario actual y copia aprovisionada)`

### Deportes (MSN) 
`app.microsoft.bingsports` · Riesgo: **Seguro** · No reversible automáticamente · Anticheat: 🟢 No toca el juego

Resultados deportivos de MSN.

- `Desinstalar la app Microsoft.BingSports (usuario actual y copia aprovisionada)`

### Traductor (Bing) 
`app.microsoft.bingtranslator` · Riesgo: **Seguro** · No reversible automáticamente · Anticheat: 🟢 No toca el juego

Traductor de Bing.

- `Desinstalar la app Microsoft.BingTranslator (usuario actual y copia aprovisionada)`

### Xbox Console Companion 
`app.microsoft.xboxapp` · Riesgo: **Seguro** · No reversible automáticamente · Anticheat: 🟢 No toca el juego

App antigua para conectar con la consola Xbox.

- `Desinstalar la app Microsoft.XboxApp (usuario actual y copia aprovisionada)`

### Microsoft Family 
`app.microsoftcorporationii.microsoftfamily` · Riesgo: **Seguro** · No reversible automáticamente · Anticheat: 🟢 No toca el juego

Control parental de Microsoft.

- `Desinstalar la app MicrosoftCorporationII.MicrosoftFamily (usuario actual y copia aprovisionada)`

### Microsoft Journal 
`app.microsoft.microsoftjournal` · Riesgo: **Seguro** · No reversible automáticamente · Anticheat: 🟢 No toca el juego

App de notas a mano alzada.

- `Desinstalar la app Microsoft.MicrosoftJournal (usuario actual y copia aprovisionada)`

### App de Xbox 
`app.microsoft.gamingapp` · Riesgo: **Avanzado** · No reversible automáticamente · Anticheat: 🟢 No toca el juego

Necesaria para Game Pass y juegos de PC Game Pass.

> ⚠️ Si usas Game Pass no la quites.

- `Desinstalar la app Microsoft.GamingApp (usuario actual y copia aprovisionada)`

### Apps promocionadas de terceros 
`app.promos` · Riesgo: **Seguro** · No reversible automáticamente · Anticheat: 🟢 No toca el juego

Candy Crush, TikTok, Disney+, Facebook, Instagram, Prime Video y otros accesos que Windows instala sin pedir permiso.

- `Desinstalar la app *CandyCrush* (usuario actual y copia aprovisionada)`
- `Desinstalar la app *BubbleWitch* (usuario actual y copia aprovisionada)`
- `Desinstalar la app *Disney* (usuario actual y copia aprovisionada)`
- `Desinstalar la app *TikTok* (usuario actual y copia aprovisionada)`
- `Desinstalar la app *Facebook* (usuario actual y copia aprovisionada)`
- `Desinstalar la app *Instagram* (usuario actual y copia aprovisionada)`
- `Desinstalar la app *Twitter* (usuario actual y copia aprovisionada)`
- `Desinstalar la app *LinkedIn* (usuario actual y copia aprovisionada)`
- `Desinstalar la app AmazonVideo.PrimeVideo (usuario actual y copia aprovisionada)`
- `Desinstalar la app *Hulu* (usuario actual y copia aprovisionada)`
- `Desinstalar la app *Duolingo* (usuario actual y copia aprovisionada)`
- `Desinstalar la app *PicsArt* (usuario actual y copia aprovisionada)`
- `Desinstalar la app *Asphalt* (usuario actual y copia aprovisionada)`
- `Desinstalar la app *MarchofEmpires* (usuario actual y copia aprovisionada)`
- `Desinstalar la app *HiddenCity* (usuario actual y copia aprovisionada)`
- `Desinstalar la app *FarmVille* (usuario actual y copia aprovisionada)`
- `Desinstalar la app *RoyalRevolt* (usuario actual y copia aprovisionada)`
- `Desinstalar la app *Flipboard* (usuario actual y copia aprovisionada)`
- `Desinstalar la app *Wunderlist* (usuario actual y copia aprovisionada)`
- `Desinstalar la app *Sketchable* (usuario actual y copia aprovisionada)`
- `Desinstalar la app *AdobeExpress* (usuario actual y copia aprovisionada)`

### OneDrive 
`app.onedrive` · Riesgo: **Avanzado** · No reversible automáticamente · Anticheat: 🟢 No toca el juego

Sincronización en la nube que se inicia con Windows y sube archivos en segundo plano.

> ⚠️ Antes de quitarlo, asegúrate de que tus archivos estén descargados en el PC (los que solo están en la nube dejarán de verse hasta reinstalarlo).

- `Desinstalar OneDrive con su desinstalador oficial`
