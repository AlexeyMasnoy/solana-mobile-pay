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
