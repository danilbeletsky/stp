# DataConverter — учебный проект для лабораторной по CI/CD

DataConverter — консольная утилита на C++17 для преобразования табличных данных между форматами CSV, JSON и YAML.

## Состав проекта

- `src/` — библиотека преобразования и программа `dataconv`
- `tests/` — модульные тесты GoogleTest
- `examples/` — примеры CSV, JSON и YAML
- `third_party/nlohmann/json.hpp` — заголовочная библиотека JSON, ничего не скачивается
- `CMakeLists.txt` — сборка. yaml-cpp 0.9.0 и GoogleTest 1.14.0 подключаются через FetchContent
- `.github/workflows/ci.yml` — конвейер GitHub Actions

Стандарт языка C++17 задан в CMake и одинаков для локальной сборки и для всех операционных систем в CI.

## Локальная сборка и тесты

Нужны CMake 3.20 или новее, компилятор с поддержкой C++17 и интернет при первой конфигурации: CMake скачивает yaml-cpp и GoogleTest.

```bash
cmake -S . -B build -DCMAKE_BUILD_TYPE=Release
cmake --build build --parallel
ctest --test-dir build --output-on-failure
```

На Windows генератор Visual Studio многоконфигурационный, поэтому тесты запускаются так:

```bash
ctest --test-dir build -C Release --output-on-failure
```

## Запуск

```bash
./build/dataconv examples/people.csv people.json
./build/dataconv examples/people.json people.yaml
```

Формат определяется по расширению файла. Вход и выход должны отличаться.

## CI

Файл `.github/workflows/ci.yml` запускается на `push`, `pull_request` и вручную (`workflow_dispatch`).

Сборка и одни и те же тесты выполняются на `ubuntu-latest`, `macos-latest` и `windows-latest`. Каталог `build/_deps` кэшируется отдельно для каждой ОС. Отчёт CTest и собранная программа сохраняются артефактами.
