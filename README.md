# MM-Algorithm for Categorical Schrödinger Bridge Matching (CSBM)


[![License](https://badgen.net/github/license/intsystems/2026-Project-206?color=green)](https://github.com/intsystems/2026-Project-206/blob/main/LICENSE)
[![GitHub Contributors](https://img.shields.io/github/contributors/intsystems/2026-Project-206)](https://github.com/intsystems/2026-Project-206/graphs/contributors)
[![GitHub Issues](https://img.shields.io/github/issues-closed/intsystems/2026-Project-206.svg?color=0088ff)](https://github.com/intsystems/2026-Project-206/issues)
[![GitHub Pull Requests](https://img.shields.io/github/issues-pr-closed/intsystems/2026-Project-206.svg?color=7f29d6)](https://github.com/kisnikser/m1p-template/pulls)

<table>
    <tr>
        <td align="left"> <b> Author </b> </td>
        <td> Fedor Kolotilin </td>
    </tr>
    <tr>
        <td align="left"> <b> Consultant </b> </td>
        <td> Grigoriy Ksenofontov,  </td>
    </tr>
    <tr>
        <td align="left"> <b> Advisor </b> </td>
        <td> Alexandr Korotin, PhD </td>
    </tr>
</table>

## Assets

- [LinkReview](LINKREVIEW.md)
- [Code](code)
- [Paper](paper/main.pdf)
- [Slides](slides/main.pdf)

## Abstract
Задача мостов Шрёдингера играет важную роль в современном машинном обучении, позволяя решать задачи генеративного моделирования при помощи теории оптимального транспорта.

Как в непрерывной так и в дискретной постановке этой задачи есть метод LightSB, сводящий задачу к оптимизации лосса (8, [LIGHT SCHRODINGER BRIDGE
](https://arxiv.org/pdf/2310.01174)):

В данной работе мы предлагаем новый метод оптимизации данной функции потерь, основанный на **MM-алгоритме**, который устраняет недостатки SGD: необходимость подбора гиперпараметров и шум градиентных оценок.

Ключевая идея — построение суррогатной функции потерь как верхней границы исходной целевой функции. Это достигается введением вариационных параметров и применением двух границ:
1. **ELBO**
2. **[Границы Бонинга](https://www.cs.ubc.ca/~murphyk/papers/nips2010.pdf)**

Полученная суррогатная функция оптимизируется чередующимся алгоритмом с **аналитическими обновлениями**:
* **E-шаг:** вычисление оптимальных вариационных параметров при фиксированных $ \theta $;
* **M-шаг:** обновление $ \theta $ в замкнутой форме при фиксированных вариационных параметрах.

Итеративная процедура полностью исключает SGD, шум мини-батчей и подбор скорости обучения. Все обновления параметров становятся детерминированными и сводятся к простым аналитическим пересчетам.

## Keywords

Categorical Schrödinger Bridge, CSBM, мост Шрёдингера, энтропийный оптимальный транспорт, дискретные диффузионные модели, MM-алгоритм, миноризация-максимизация, вариационный вывод, нижняя граница правдоподобия, ELBO, граница Бонинга, квадратичная граница, детерминированная оптимизация, аналитические обновления, непарный перевод между доменами, генеративное моделирование.


## Licence

Our project is MIT licensed. See [LICENSE](LICENSE) for details.