name: Build APK
on:
  workflow_dispatch:
  push:
permissions:
  contents: write
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-java@v4
        with:
          distribution: temurin
          java-version: 17
      - uses: gradle/actions/setup-gradle@v4
        with:
          gradle-version: 8.7
      - run: unzip -q GeminiAgent.zip
      - run: cd GeminiAgent && gradle assembleDebug --no-daemon
      - env:
          GH_TOKEN: ${{ github.token }}
        run: gh release create "build-${{ github.run_number }}" GeminiAgent/app/build/outputs/apk/debug/app-debug.apk --title "Gemini Agent" --notes "APK"# Jarvis-assistant
