# FunctionSeries

Десктопное WPF-приложение для разложения функций в ряды Фурье и Тейлора с построением графиков.
Также позволяет получить ряд Фурье по набору точек, расставленных вручную на графике.

## Возможности

- **Ряд Фурье** функции с заданным периодом и количеством членов. Поддерживаются три типа разложения:
  - `CosSin` — функция общего вида (синусы и косинусы);
  - `Cos` — чётная функция (только косинусы);
  - `Sin` — нечётная функция (только синусы).

  Чётность функции определяется автоматически (`FunctionHelper.GetType`).
- **Ряд Тейлора** в окрестности точки x0 с заданной степенью. Производные вычисляются символьно с помощью MathNet.Symbolics.
- **Аппроксимация по точкам** (`FourierApprox`, обычное и быстрое преобразование Фурье).
  В окне «Получение ряда по точкам»: `s` — поставить точку на график, `d` — удалить ближайшую к курсору точку.
- **Текстовый вывод ряда** — формула полученного ряда в отдельном окне.
- **Операции над рядами** (`FourierSeries`, `TaylorSeries`): `+`, `-`, унарный минус, умножение на число, `==`,
  `GetDerivative()`, `GetIntegral()`, `Compute(x)`.

## Синтаксис функций

- Единственная допустимая переменная — `x`. Можно задать и функцию без переменных (константу).
- Функции записываются в виде `функция(...)`: `sin`, `cos`, `tan`, `ctan`, `asin`, `acos`, `atan`, `actan`, `log`, `ln`, `sqrt` (или `√`).
- Операторы: `^ + - * / !`.

Примеры: `x^2 + 5`, `sin(x) * cos(x)`, `e^x`, `sqrt(x + 1)`.

## Структура решения

| Проект | Описание |
| --- | --- |
| `FunctionSeriesClassLibrary` | Ядро: `FourierSeries`, `TaylorSeries`, `FourierApprox`, `Interpreter` (разбор выражений через польскую запись), `FunctionHelper` |
| `WpfFunctionSeries` | Графический интерфейс (графики — ScottPlot.WPF) |
| `FunctionSeriesConsoleApp` | Консольное приложение для экспериментов |
| `FunctionSeriesTest` | Модульные тесты (MSTest) |

## Требования

- .NET 6 SDK
- Windows (WPF-приложение собирается под `net6.0-windows`)

NuGet-зависимости (`MathNet.Symbolics`, `ScottPlot.WPF`) восстанавливаются автоматически при сборке.

## Сборка и запуск

```sh
dotnet build FunctionSeries.sln
dotnet run --project WpfFunctionSeries
```

Либо откройте `FunctionSeries.sln` в Visual Studio или Rider и запустите проект `WpfFunctionSeries`.

## Тесты

```sh
dotnet test FunctionSeriesTest
```

## Пример использования библиотеки

```csharp
using FunctionSeriesClassLibrary;

// Ряд Фурье функции x с периодом 6.28 по синусам, 5 членов
var fs = new FourierSeries(5, 6.28, "x", FourierSeriesType.Sin);
Console.WriteLine(fs);
Console.WriteLine(fs.Compute(1));

// Ряд Тейлора cos(x) в окрестности x0 = 0, степень 5
var ts = new TaylorSeries("cos(x)", 0, 5);
Console.WriteLine(ts.GetIntegral());
Console.WriteLine(ts.GetDerivative().Compute(2));
Console.WriteLine(ts + ts);
```
