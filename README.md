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

Как в непрерывной так и в дискретной постановке этой задачи есть метод LightSB, сводящий задачу к оптимизации следующего лосса:

$$
KL(q^* \| q_\theta) = \mathcal{L}(\theta) - \mathcal{L}^*,
$$

where

$$
\mathcal{L}(\theta) = \mathbb{E}_{p_0(x_0)} \left[ \log c_\theta(x_0) \right] - \mathbb{E}_{p_1(x_1)} \left[ \log v_\theta(x_1) \right],
\tag{17}
$$

and $\mathcal{L}^* \in \mathbb{R}$ is a constant value not depending on $\theta$, therefore, it can be omitted.

В данной работе мы предлагаем новый метод оптимизации данной функции потерь, основанный на **MM-алгоритме**, который устраняет недостатки SGD: необходимость подбора гиперпараметров и шум градиентных оценок.

Ключевая идея — построение суррогатной функции потерь как верхней границы исходной целевой функции. Это достигается введением вариационных параметров и применением двух границ:
1. **ELBO** — для компонентов, зависящих от $v(x_1)$;
2. **[Границы Бонинга](https://www.cs.ubc.ca/~murphyk/papers/nips2010.pdf)** — для компонентов, связанных с $ c(x_0) $.

Полученная суррогатная функция оптимизируется чередующимся алгоритмом с **аналитическими обновлениями**:
* **E-шаг:** вычисление оптимальных вариационных параметров при фиксированных $ \theta $;
* **M-шаг:** обновление $ \theta $ в замкнутой форме при фиксированных вариационных параметрах.

Итеративная процедура полностью исключает SGD, шум мини-батчей и подбор скорости обучения. Все обновления параметров становятся детерминированными и сводятся к простым аналитическим пересчетам.


<!-- тут есть акцент именно на категориальную постановку задачи. Важен ли он или моя задача более широкая и применяется просто для LightSB? -->

## Introduction

**Categorical Schrödinger Bridge Matching (CSBM)** — это метод для решения задачи построения моста Шрёдингера (Schrödinger Bridge, SB) в дискретных пространствах, предложенный Ксенофонтовым и Коротиным (2025). Задача SB является фундаментальным инструментом для генеративного моделирования и непарного перевода между доменами. Она заключается в поиске наиболее вероятного случайного процесса, который интерполирует между двумя заданными распределениями вероятностей.

Задачи энтропийного оптимального транспорта (EOT) и моста Шрёдингера (SB) привлекают внимание в машинном обучении благодаря приложениям в генеративном моделировании, при этом разработано множество методов для непрерывных пространств (Daniels et al., 2021; Gushchin et al., 2023a; 2024b; Mokrov et al., 2024; Vargas et al., 2021; Chen et al., 2021; Shi et al., 2023; De Bortoli et al., 2024; Korotin et al., 2024; Gushchin et al., 2024a). Однако многие реальные данные дискретны (текст — Austin et al., 2021; Gat et al., 2024; молекулярные графы — Vignac et al., 2022; Qin et al., 2024; Luo et al., 2024; белки — Campbell et al., 2024; векторно-квантованные представления — Van Den Oord et al., 2017; Esser et al., 2021). Несмотря на прогресс в дискретных диффузионных моделях (Hoogeboom et al., 2021; Austin et al., 2021; Campbell et al., 2022; Lou et al., 2023; Sahoo et al., 2024; Campbell et al., 2024; Gat et al., 2024), существующие подходы к EOT/SB в дискретных пространствах ограничены первыми работами (Kim et al., 2024; Ksenofontov & Korotin, 2025), и практические, широко применимые решатели по-прежнему отсутствуют.

**Обозначения.** Рассмотрим дискретное пространство состояний $\mathcal{X} = \mathbb{S}^D$, где $\mathbb{S} = \{0, 1, \ldots, S - 1\}$ — множество из $S$ категорий, а $D$ — размерность. Каждый элемент $x \in \mathcal{X}$ представляет собой $D$-мерный вектор  

$$x = (x^1, \ldots, x^D).$$

Время дискретизируется как $\{t_n\}_{n=0}^{N+1}$ с $0=t_0<t_1<\cdots<t_N<t_{N+1}=1$. Это даёт $N+2$ момента времени и определяет *пространство траекторий* $\mathcal{X}^{N+2}$ с кортежем $x_{\text{in}} := (x_{t_1}, \ldots, x_{t_N}) \in \mathcal{X}^N$, собирающим промежуточные состояния. Множество $P(\mathcal{X}^{N+2})$ включает все дискретные по времени случайные процессы на пространстве траекторий, при этом $M(\mathcal{X}^{N+2}) \subset P(\mathcal{X}^{N+2})$ обозначает подмножество марковских процессов. Любой $q \in M(\mathcal{X}^{N+2})$ допускает прямое и обратное представления:

$$q(x_0, x_{\text{in}}, x_1) = q(x_0) \prod_{n=1}^{N+1} q(x_{t_n} | x_{t_{n-1}}) = q(x_1) \prod_{n=1}^{N+1} q(x_{t_{n-1}} | x_{t_n}).$$

Наконец, $q(\cdot|\cdot)$ используется для обозначения *условных* ($x_0 \to x_1$) и *переходных* ($x_{t_{n-1}} \to x_{t_n}$) распределений.

## Keywords

Categorical Schrödinger Bridge, CSBM, мост Шрёдингера, энтропийный оптимальный транспорт, дискретные диффузионные модели, MM-алгоритм, миноризация-максимизация, вариационный вывод, нижняя граница правдоподобия, ELBO, граница Бонинга, квадратичная граница, детерминированная оптимизация, аналитические обновления, непарный перевод между доменами, генеративное моделирование.


## Licence

Our project is MIT licensed. See [LICENSE](LICENSE) for details.