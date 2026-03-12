# Title

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
        <td> Andrey Grabovoy, PhD </td>
    </tr>
    <tr>
        <td align="left"> <b> Advisor </b> </td>
        <td> Andrey Grabovoy, PhD </td>
    </tr>
</table>

## Assets

- [LinkReview](LINKREVIEW.md)
- [Code](code)
- [Paper](paper/main.pdf)
- [Slides](slides/main.pdf)

## Introduction

Contemporary AI agent systems employ large language models (LLMs) as operators that interact with diverse tools to accomplish designated tasks. Such agent architectures typically incorporate multiple models with varying parameter counts, reflecting distinct complexity requirements. The selection of an optimal LLM for specific tools is commonly achieved through Reinforcement Learning from Human Feedback (RLHF), which facilitates adaptation to particular tools and tasks during the training process.


## Abstract

This study addresses the problem of tool complexity estimation as a means to automate and unify the development of AI agent architectures. Crucially, the complexity assessment is confined exclusively to the tools themselves, excluding any model-specific characteristics.

The instrumental data utilized for complexity estimation comprises tool documentation texts and the structural specifications of permissible API requests.

Two baseline approaches for tool complexity estimation are proposed:
1. A complexity metric based on numerical characteristics of the API specification, such as the dimensionality of the argument space that the model must supply to the tools;
2. An intrinsic dimensionality derived from the tool description text, which serves as a proxy for tool complexity.

The anticipated outcome involves a comparative analysis of RLHF convergence rates under two conditions: without prior information regarding tool complexity, and with prior information informed by the proposed complexity estimates.

## Keywords

AI agents, Large Language Models (LLMs), tool complexity estimation, API specifications, RLHF (Reinforcement Learning from Human Feedback), intrinsic dimensionality, LLM adaptation, tool documentation, agent architecture optimization

## Citation

If you find our work helpful, please cite us.
```BibTeX
@article{citekey,
    title={Title},
    author={Name Surname, Name Surname (consultant), Name Surname (advisor)},
    year={2025}
}
```

## Licence

Our project is MIT licensed. See [LICENSE](LICENSE) for details.