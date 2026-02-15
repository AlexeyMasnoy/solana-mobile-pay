# solana-mobile-pay

## 1) Команды для запуска в этом Codespace

```bash
# из каталога /workspace
npx create-expo-app@latest solana-mobile-pay \
  --template expo-template-bare-minimum \
  --yes

cd solana-mobile-pay

# опционально: отключить iOS-скрипт и сфокусироваться только на Android
npm pkg set scripts.ios="echo 'iOS disabled for this project'"

# собрать и запустить Android
npx expo run:android
```

Если вы хотите явно зафиксировать TypeScript, убедитесь, что `tsconfig.json` существует (bare TypeScript-шаблон обычно создаёт его автоматически):

```bash
test -f tsconfig.json || npx expo customize tsconfig.json
```

## 2) Почему bare React Native (а не чистый web)

Solana Mobile Pay ориентирован на нативные Android-сценарии для кошелька и платежей. **Bare Expo React Native-приложение** даёт прямой доступ к нативным Android API и нативным модулям (безопасное хранение ключей, интеграции NFC/Bluetooth, фоновые сервисы, deep links и продвинутые SDK-обвязки кошельков), при этом сохраняя удобные инструменты Expo там, где это полезно. Чистое web-приложение не обеспечивает такой же уровень нативных Android-возможностей и интеграций с устройством, которые нужны для надёжных мобильных платежей.

## 3) Если `expo run:android` падает с `Failed to resolve the Android SDK path` и `spawn adb ENOENT`

В Codespace обычно не настроены Android SDK и `adb` «из коробки». Ошибка из вашего лога означает, что Expo не нашёл SDK по пути `/home/codespace/Android/sdk` и не нашёл `adb` в `PATH`.

Запустите:

```bash
# 1) Java
sudo apt-get update
sudo apt-get install -y openjdk-17-jdk wget unzip

# 2) Android SDK command-line tools
mkdir -p "$HOME/Android/cmdline-tools"
cd "$HOME/Android"
wget -O cmdline-tools.zip https://dl.google.com/android/repository/commandlinetools-linux-11076708_latest.zip
unzip -q cmdline-tools.zip -d cmdline-tools
mv cmdline-tools/cmdline-tools cmdline-tools/latest

# 3) Переменные окружения
cat >> ~/.bashrc <<'BASHRC'
export ANDROID_HOME=$HOME/Android
export ANDROID_SDK_ROOT=$ANDROID_HOME
export PATH=$PATH:$ANDROID_HOME/cmdline-tools/latest/bin:$ANDROID_HOME/platform-tools:$ANDROID_HOME/emulator
BASHRC
source ~/.bashrc

# 4) Установка SDK-пакетов
yes | sdkmanager --licenses
sdkmanager "platform-tools" "platforms;android-34" "build-tools;34.0.0"

# 5) Проверка
adb version

# 6) Повторный запуск проекта
cd /workspace/solana-mobile-pay
npx expo run:android
```

> Если вы запускаете на удалённом устройстве/эмуляторе, убедитесь, что `adb devices` показывает хотя бы одно устройство.
