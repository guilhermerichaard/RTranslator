# English Ear MVP

Branch de trabalho: `english-ear-mvp`

Objetivo: adaptar o RTranslator para funcionar como um assistente offline de inglês no ouvido, usando um fone Bluetooth e ativação sob demanda.

## Abrir no Android Studio

1. Abra Android Studio.
2. Escolha **Get from VCS**.
3. Use a URL:
   `https://github.com/guilhermerichaard/RTranslator.git`
4. Depois do clone, selecione a branch `english-ear-mvp`.
5. Aguarde o Gradle Sync.
6. Em **Settings > Build, Execution, Deployment > Build Tools > Gradle**, use JDK 17.
7. Garanta no SDK Manager:
   - Android SDK Platform 33
   - CMake 3.22.1
   - NDK 25.1.8937393
8. Conecte um Android físico por USB e execute o módulo `app`.

Não faça upgrade automático do Android Gradle Plugin ou das dependências antes de termos um build-base funcionando.

## Base técnica atual

- Android application ID: `nie.translator.rtranslator`
- minSdk: 24
- targetSdk: 32
- compileSdk: 33
- Android Gradle Plugin: 8.2.2
- JDK esperado: 17
- Arquitetura nativa: arm64-v8a
- Whisper local para reconhecimento de fala
- NLLB local para tradução
- TTS do Android para reprodução
- Permissões de microfone e Bluetooth já existem no app

## MVP desejado

Fluxo principal:

1. App fica em espera.
2. Usuário ativa por comando curto ou botão do headset.
3. O app escuta a próxima fala em inglês.
4. Whisper transcreve localmente.
5. NLLB traduz para português localmente.
6. A tradução é reproduzida no fone Bluetooth.
7. O app volta ao estado de espera.

## Comandos planejados

- `Traduzir` — traduz a próxima frase em inglês.
- `Repete` — repete a última tradução.
- `Inglês` — reproduz/mostra a frase original.
- `Devagar` — reproduz a frase em inglês mais lentamente.
- `Como digo ...` — recebe português e devolve uma frase em inglês.
- `Como respondo?` — sugere uma resposta curta em inglês.

## Primeira implementação

Antes de integrar wake word, validar:

- captura de áudio usando o headset Bluetooth;
- inglês -> texto;
- texto inglês -> português;
- saída TTS exclusivamente pelo headset;
- funcionamento com a tela apagada/background;
- latência total por frase.

Depois disso, integrar um detector de wake word local de baixo consumo, mantendo Whisper/NLLB inativos enquanto o app está em espera.

## CI

O branch possui `.github/workflows/android-build.yml`, que tenta compilar `assembleDebug` com JDK 17 e os pacotes Android necessários. Use o resultado desse workflow como verificação inicial antes de alterar Gradle/AGP.
