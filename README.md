# EFI-Hackintosh-Dell7472

EFI de OpenCore para Dell Inspiron 14 7472 con Intel Core i7-8550U, Intel UHD
Graphics 620, NVIDIA MX150 desactivada, Intel Wireless 7265 (`8086:095a`) e
Intel Bluetooth USB (`8087:0a2a`).

## Estado actual

- OpenCore `1.0.7` release, con recursos y drivers actualizados.
- AppleALC `1.9.7`.
- `itlwm 2.3.0`, `IntelBluetoothFirmware 2.5.0` e `IntelBTPatcher 2.5.0`
	para la Wireless 7265 y su Bluetooth.
- Se retiraron los kexts Broadcom, que no corresponden al hardware detectado.
- El arranque ya no usa `-v`, `debug=0x100` ni `keepsyms=1`; se conserva
	`-vi2c-force-polling` para el touchpad VoodooI2C.
- La NVIDIA MX150 se mantiene desactivada mediante ACPI; la UHD 620 es la GPU
	que debe proporcionar aceleración.

## BIOS recomendado

Aplicar antes de arrancar macOS: UEFI only, Secure Boot desactivado, SATA en
AHCI, Fast Boot desactivado, VT-d desactivado si no se puede desactivar CFG
Lock, CFG Lock desactivado si el firmware lo permite y DVMT Pre-Allocated en
64 MB o más. No activar CSM/Legacy.

## Tahoe: limitaciones importantes

macOS Tahoe no está soportado oficialmente en este modelo de Mac no original.
La EFI puede arrancar el instalador, pero no garantiza aceleración Metal en la
UHD 620, Wi-Fi Intel o Bluetooth hasta probar la build concreta de Tahoe y
aplicar los root patches de OpenCore Legacy Patcher que correspondan. La MX150
no tiene soporte gráfico funcional en macOS y debe permanecer desactivada.

`itlwm` no proporciona la interfaz Wi-Fi nativa de macOS: se necesita HeliPort
para seleccionar redes. El Bluetooth Intel depende de que el dispositivo USB
`8087:0a2a` permanezca visible; si desaparece tras suspensión, hay que revisar
el mapeo USB y no cambiar a kexts Broadcom.

## USB tethering Android

HoRNDIS no se añade a esta EFI. No es un driver de arranque de OpenCore, sino
un kext antiguo que se instala dentro de macOS en
`/Library/Extensions/HoRNDIS.kext`. La última release oficial es `9.2` y su
código fuente no recibe cambios desde 2018; no existe garantía de que cargue en
Tahoe y forzarlo dentro de `EFI/OC/Kexts` puede causar un kernel panic.

Después de arrancar macOS, descarga el instalador oficial desde
[HoRNDIS 9.2](https://github.com/jwise/HoRNDIS/releases/tag/rel9.2), instala el
`.pkg`, reinicia y activa **USB tethering** en el teléfono Android. Si Tahoe
rechaza el kext, utiliza Wi-Fi tethering o un adaptador Ethernet USB compatible;
no copies manualmente HoRNDIS a la EFI.

## Primer arranque

1. Haz una copia de la EFI que actualmente funciona y prueba esta versión
	 desde una memoria USB.
2. En OpenCore selecciona `Reset NVRAM` una vez después de sustituir la EFI.
3. Instala macOS y valida primero teclado, touchpad, audio, batería, Wi-Fi,
	 Bluetooth, suspensión y aceleración gráfica.
4. Solo después instala root patches y actualizaciones mayores de Tahoe.

La configuración fue validada con `ocvalidate` de OpenCore `1.0.7`. Los
seriales de `PlatformInfo` son de ejemplo: no deben compartirse públicamente y
deben ser únicos si se usan servicios de Apple.
