# SteamOS на Switch: состояние проекта и вся карта работ

**Обновление13 октября2026:** Steam UI854×480 теперь рисуется на GPU через
Mesa 24.2.8/Zink/T3 и NVIDIA. Пользователь подтвердил картинку и навигацию;
после примерно часа без замеченных багов попросил отключить диагностику.
Она отключена, новый результат ещё проверяет пользователь. CPU OnPaint/копии
остаются: это аппаратная растеризация, но не zero-copy.
[Путь, исправления и причины CPU-нагрузки](ZINK-STEAM-UI.md).
Текст ниже описывает исторический срез до12 октября.

Срез: **12 октября 2026 года**. Это история разработки и справочник для нового
разработчика. Документ не является готовым образом и не выдаёт разрешений на
запуск старых экспериментов.

## Что уже работает

Главное достижение — настоящий standalone Gamescope на NVIDIA Tegra X1, который
сам композитит кадры Vulkan, на GPU превращает их в Tegra GOB/block-linear layout
и выводит на LCD через legacy Tegra DC/FLIP4. Host Xorg и вложенный compositor
для этого вывода не нужны. Internal Xwayland нужен X11-клиентам и остаётся
частью Gamescope-сессии.

Steam UI автоматически поднимается после загрузки системы. Принятый путь
ориентации и масштаба показан пользователю: landscape-контент, portrait scanout
720×1280, компенсация разворота и зеркала. Позже UI оставили низкого разрешения
426×240, а игровые поверхности смогли иметь собственное разрешение вплоть до
проверенного 1280×720. Для FEZ/SMB пользователь подтвердил работоспособность;
оконный режим — известное достаточное условие для alpha.

Звук подключён через один пользовательский PipeWire/WirePlumber/Pulse endpoint.
Исправлен access-rule, мешавший экспорту/линковке sink. Есть touch→XTest и
volume-key helpers. Геймпад и гироскоп подтверждены пользователем. Сон/пробуждение
позже также подтверждены пользователем. Отдельная полноценная проверка всех
вариантов touch, будущих boot и всех Steam power/client-lifecycle действий не
следует из общего «кнопки работают».

Native Linux x86-64 игры **Super Meat Boy и FEZ** работают через Box64 и
SteamLinuxRuntime soldier/scout с native Wayland. FEZ запускается x86-64 helper'ом
внутри настоящей Steam launch chain; штатный выбор 32-bit executable на aarch64
был неправильным. Этот путь не является Proton и не является доказательством FEX.

## Что именно доказано по FEX

1. x64 выполнение под ARM-Wine/FEX: проверены файлы, потоки и синхронизация.
2. Реальный Steam context и SteamAPI/IPC: pipe/user/callbacks подтверждены.
3. Offscreen D3D11: приватный DXVK-Sarek исключает только реально отсутствующий
   Win32 WSI extension в opt-in режиме, запрещает WSI; GPU draw/completion и
   все 4096 пикселей readback прошли проверку на Tegra.
4. Оконная Vulkan-синтетика x64 под FEX дошла через winex11, FROG→Wayland,
   NVIDIA EGLStream, Gamescope и LCD. Сначала был подтверждён короткий показ,
   затем unit06 на 240 секунд: динамичная картинка, правильная ориентация и
   возврат Steam. Consumer generation была финализирована.

**Это ещё не означает, что Proton 11.0-1 FEX починен для обычной игры.**
Нужны квалифицированный реальный game process, тот же private runtime, безопасная
инициализация, графика игры, устойчивое окно и нормальный выход всей цепочки.
Завершение самой фикстуры и принудительная очистка shim/wineserver — разные вещи.

## Почему Box64 не решает всё автоматически

Box64 запускает x86-64 Wine и использует собственный слой native-wrapped библиотек.
FEX-вариант использует ARM-Wine, ARM64EC/WoW64 guest↔Unix bridge и другую
контейнерную closure. Общий участок — FROG→Wayland/EGLStream→Gamescope→LCD —
переиспользован. Но driver registration, Wine desktop, native loader anchor,
Vulkan extension mapping и процессный lifecycle различаются. Наличие картинки
в Box64 не исправляет эти границы FEX.

У Box64 тоже нет общего игрового PASS: HL2DM дошёл до меню с D3D9Ex/DXVK-Sarek,
но пользователь видел низкую производительность и неправильную ориентацию.
We Were Here дошла до графических стадий и кадров, однако были consumer/kernel
инциденты. Было бы неправильно использовать это как полностью устойчивый эталон.

## Текущий первый blocker

Последний trace17 R3 остановился на **startup Steam UI**, до SteamAction,
FEX-helper, ICD bootstrap и игры. Отказ:
`eglStreamConsumerReleaseKHR failed (0x321c)`.

В probe и accepted recovery есть одна временная цепочка: stream1280×800,
первый commit, Release0x321c, новый stream426×240, UI продолжает работать.
Это сильная связь со startup resize; точная failing generation, состояние до
Release и producer disconnect ещё не доказаны. Kernel GPU-fault в этом unit
не был зарегистрирован. Называть результат новым отказом FEX GPU нельзя.

M9L18 — PC-подготовка startup-only capture на уже собранном частном lifetime-fix
варианте. Новый аппаратный результат не опубликован. Старый STOP сохраняется.

## Чего нельзя переоткрывать без новых оснований

- proprietary NVIDIA GLX frontend внутри этого Xwayland;
- Zink как уже готовый GPU Steam UI — T1/T2 были синтетикой, T3 не инициализировался;
- DRI3/GBM как простую «недостающую библиотеку»;
- массовые CEF flags, affinity/governor и `--disable-gpu` ради 720p UI performance;
- llvmpipe как окончательное решение производительности;
- Venus/vtest OPAQUE helper как уже реализованный путь;
- screenshot через `gamescopectl` в исторической сборке;
- автоматическое объяснение каждого сетевого отказа GPU hang'ом;
- изображение/визуальный PASS только из presents, XWD или API rc0.

## Куда двигаться

Сначала адресно наблюдать startup-resize consumer ownership/state. Затем
квалифицировать scoped исправление, включая resize/unmap/destroy и настоящий
выход сессии. После этого возвращаться к private FEX target/bootstrap identity и
реальной игре. Game PASS остаётся за пользователем на физическом экране.

Публикация разделена на графический стек, LCD-компонент, сборку образа и этот
исследовательский репозиторий. Источники и рецепты открыты, закрытые Steam/NVIDIA
payloads не включены. Все опорные работы Valve, gman, Switchdeck/SildurFX/Ivy и
Switchroot/CTCaer явно отмечены в [NOTICE](../NOTICE.md).

Подробная хронология: [TIMELINE](TIMELINE.md); полная карта препятствий и
исправленных ошибочных выводов: [BLOCKERS](BLOCKERS.md).
