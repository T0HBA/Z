# shizoval

Discord: **солевой#4769**

## Установка

**1.** Установите [Tampermonkey](https://www.tampermonkey.net/)

**2.** Установите [скрипт](https://github.com/T0HBA/Z/raw/main/release/shizoval.user.js)

## Клавиши

`INSERT`, `NumPad 0`, `/` - Открыть меню

## Кастомизация чита

  В версии **0.64.3** добавлена возможность кастомизации чита, а именно был открыт полный доступ к апи чита и всем внутреннем функциям.
  
  **Глобальные переменные созданные для вас**
  
      Мне лень объяснять, что да как, поэтому за меня это сделает твоя консоль браузера
      utils
      gameObjects
      storeOpener
      config
      removeMines
      airBreak
      cameraHack
      menu
      cImGui
      ImGui
      other
      stick
      clicker
      sync
      consoleLog
      wallhack
      filters
      packetControl
      striker
      
  ![](https://github.com/T0HBA/Z/blob/main/img/exampleScript.jpg?raw=true)
 
  Пару примеров (эти скрипты вписывать в консоль или в конец скрипта): 
  
```js
/*  SHIZOVAL
*  Автор кода: sheezzmee
*  Название: Legit FPS hack
*  Описание: Удаление мин позиция, которых совпадает с другими
*  Активация: Digit0
*/
   
const checkMine = mine => {
    if (!mine.removeMine_0)
        return;

    const mines = gameObjects.world.triggers_0.triggers_0.array;
    
    for (const element of mines) {
        if (!element.removeMine_0 || element.id === mine.id)
            continue;
    
        if (element.position.distance_ry1qwf$(mine.position) <= 10)
            element.removeMine_0();
    }
}

document.addEventListener('keyup', (e) => {
    if (utils.isChatOpen() || e.code !== 'Digit0') 
        return;

    const mines = gameObjects.world?.triggers_0?.triggers_0?.array;

    if (!utils.isArrayValid(mines))
        return;

    for (const element of mines)
        checkMine(element);
})
```

```js
/*  SHIZOVAL
*  Автор кода: sheezzmee
*  Название: Box Teleport
*  Описание: Телепортирует танк на бонусные ящики
*  Активация: B (удержание)
*/

requestAnimationFrame(function boxTeleport() {
    requestAnimationFrame(boxTeleport);

    if (!utils.getKeyState('KeyB'))
        return;

    const world = gameObjects.world,
        tank = gameObjects.localTank,
        physics = tank?.['TankPhysicsComponent'];

    if (!world || !physics)
        return;

    const boxes = gameObjects.world?.triggers_0?.triggers_0?.array;

    if (!utils.isArrayValid(boxes))
        return;

    for (const box of boxes) {
        const object3d = box.bonus_0?.bonusMesh?.object3d;

        if (!object3d)
            continue;

        physics.body.state.position.init_ry1qwf$(object3d.aabb.center);
    }
})
```

```js
/*  SHIZOVAL
*  Автор кода: sheezzmee
*  Название: Flip simple tp
*  Описание: Переворачивает танк на спину (хз нах но просили сделать)
*  Активация: автоматическая
*/

airBreak.align = (body, direction) => {
    body.state.velocity.z = 0;
    body.state.angularVelocity.x = 0;
    body.state.angularVelocity.y = 0;
    body.state.orientation.x = 0;
    body.state.orientation.y = 0;

    if (direction === 'noob') {
        body.state.angularVelocity.z = 0;
        body.state.velocity.x = 0;
        body.state.velocity.y = 0;
        body.state.orientation.w = 0;
        body.state.orientation.z = 0;
        body.state.orientation.y = 1;

        return;
    }

    if (direction !== 0) {
        body.state.angularVelocity.z = 0;
        body.state.velocity.x = 0;
        body.state.velocity.y = 0;
        body.state.orientation.w = Math.sin(-(direction - Math.PI) / 2);
        body.state.orientation.z = Math.cos(-(direction - Math.PI) / 2);
    }
}
```

Если вы хотите, чтобы я выставил ваш скрипт сюда (авторство будет указано), то присылайте его мне в дс (там выше)

## Клонирование проекта

```bash
# Клонирование репозитория
git clone https://github.com/T0HBA/Z.git
# Перейти в папку с репозиторием
cd shizoval
# Установить зависимости
npm i
# Компиляция проекта
npx gulp
```

## Список изменений

* 0.64.3:

      - Вырезана функция: BoxTeleport, FlagTeleport, DeSync, Box WallHack, Anti-Crash, Gravity, No-Knockback
      
      - Улучшена функция AimBot
      
      - Добавлена функция Packet Control (только для кликера)
          * позволяет контролировать состояние сервера (если большой пинг, то скорость кликера уменьшается)
          * активация автоматическая
          
      - Добавлена функция PPS (Packets Per Second)
          * отображает кол-во пакетов отправляемых на сервер (включая пакеты отправляемые самой игрой)
          * активация автоматическая
      
      - Оптимизирован блок Sync 
      
      - Улучшен Rapid Update
          * пакеты не отправляются, если состояние танка не изменяется
          * пакеты не отправляются, если танк мертв или не заспавнен (входит в битву)

      - Отключена функция sendChassisControl (отправка пакетов передвижения)

      - Пофикшен фпс баг

      - Добавлена функция ConsoleLog
          * логирует сообщения, убийства, выходы, самоуничтожения в консоль
    ![](https://github.com/T0HBA/Z/blob/main/img/consoleLog.jpg?raw=true)
       
      - Улучшен кликер
          * пакеты не отправляются, если танк мертв или не заспавнен (пауза)
          * пакеты не отправляются, если сервер не отвечает
          * изменен цикл на setInterval (более быстрая работа)
   
      - Добавлена возможность кастомизации чита
        
      - Отключен самоурон от страйкера

      - WallHack, Remove mines, Freeze tanks теперь активируется до того, как заспавнится локальный танк

      - Оптимизировано меню (меньше нагрузка)

      - Оптимизирован страйкер хак

      - Пофикшена функция GTA Camera

      - Пофикшен баг когда камера застывает на одном месте

      - Улучшена функция отключения коллизии
          * теперь танки полностью игнорируются (раньше коллизия башен танков не отключалась)