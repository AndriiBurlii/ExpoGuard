# Передача в Ubuntu 24.04

Основна копія залишається на Mac у `Lab01/Work/ExpoGuard`; результати з VM спочатку повертати в окрему вхідну папку й порівнювати. Не редагувати дві копії одночасно. Порядок узгоджений із [Study VM-transfer.md](../../../../../../../../VM-transfer.md).

## Архів

У сусідній `../` від кореня проєкту буде `ExpoGuard-LR1-2026-10-03.tar.gz` та файл `.sha256`. Архів містить папку ExpoGuard без `.git`, `firmware/build`, тимчасових файлів та сценаріїв відео. Перевірити хеш до розпакування. Відсутність Git у VM-копії навмисна; Git показувати з основної копії на Mac.

Запустіть Ubuntu VM перед перевіркою B-03, завчасно до відеозапису. Спочатку встановіть, де доступні Docker і VS Code з чинною Wokwi ліцензією. На цьому комп'ютері перевірена установка VS Code на Mac, Ubuntu VM ще не знайдено. Нічого не копіювалося в VM.

Передайте архів і checksum у `~/Downloads` Ubuntu через доступний shared folder або scp. Ім'я VM/SSH адреса ще невідомі; не використовувати `ubuntu-vm` як перевірену адресу. Наведені команди для Ubuntu поки не виконувалися.

```sh
cd ~/Downloads
sha256sum -c ExpoGuard-LR1-2026-10-03.tar.gz.sha256
tar -tzf ExpoGuard-LR1-2026-10-03.tar.gz
mkdir -p ~/study/2026-autumn/SP/Burlii/Lab01
test ! -e ~/study/2026-autumn/SP/Burlii/Lab01/ExpoGuard && tar -xzf ExpoGuard-LR1-2026-10-03.tar.gz -C ~/study/2026-autumn/SP/Burlii/Lab01
```

Якщо `test` не проходить, порівняйте наявну папку з архівом і оберіть нову порожню папку. Не перезаписувати роботу.

## Дві збірки в VM

```sh
cd ~/study/2026-autumn/SP/Burlii/Lab01/ExpoGuard
mkdir -p ../vm-evidence
cat /etc/os-release > ../vm-evidence/os-release.txt
uname -a > ../vm-evidence/host.txt
docker version > ../vm-evidence/docker-version.txt
make clean > ../vm-evidence/build-1.log 2>&1
make build IMAGE=expoguard-lr1-build:local >> ../vm-evidence/build-1.log 2>&1
sha256sum firmware/build/firmware.elf firmware/build/firmware.hex firmware/build/firmware.bin > ../vm-evidence/build-1.sha256
make clean > ../vm-evidence/build-2.log 2>&1
make build IMAGE=expoguard-lr1-build:local >> ../vm-evidence/build-2.log 2>&1
sha256sum firmware/build/firmware.elf firmware/build/firmware.hex firmware/build/firmware.bin > ../vm-evidence/build-2.sha256
cmp ../vm-evidence/build-1.sha256 ../vm-evidence/build-2.sha256
```

Запускайте команди послідовно й перевіряйте exit code кожної, не продовжуйте після помилки. Збережені `docs/validation/build-*` є логами Mac, нові VM логи лежать окремо.

Якщо Wokwi запускається у VM: `code .`, F1 → `Wokwi: Start Simulator`. Якщо він запускається на хості, передайте всі три файли `firmware/build/firmware.*` з VM у відповідну копію ExpoGuard після порівняння. Wokwi не обов'язково працює в VM; важливо показати точне місце збірки та симуляції. Не активувати trial.

## Повернення доказів

Перед rsync робити preview з `-avn`, після перевірки списку використати `-av`, без `--delete`. Невідомі SSH адреса та username мають бути уточнені перед реальною передачею. Логи та screenshots із VM повернути в окрему вхідну папку на Mac, порівняти, тоді додати достовірні результати до проєкту. Редагований звіт зберігати в `Lab01/Reports`, PDF для здачі також додати в репозиторій. Архів не підтверджує виконання в Ubuntu.
