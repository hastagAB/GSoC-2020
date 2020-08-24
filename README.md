<h1 align="center">Google Summer of Code 2020 <img src="https://media2.giphy.com/media/KB8MHRUq55wjXVwWyl/source.gif" width="50"></h1>

<p align="center"><i>A full report on my Google Summer of Code 2020 work with FOSSology</i></p>
<p align="center"><i>Project: "Accelerating Atarashi" </i>  👨‍💻</p>

<p align="center">
        ✨ 🍰 ✨&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
</p>

![Logo](/Assets/GSoC-FOSSology.png)

<img align="right" alt="GIF" src="https://media.giphy.com/media/836HiJc7pgzy8iNXCn/giphy.gif" />

Google Summer of Code 2020 🚩 Report On Project: "Accelerating Atarashi" 👨‍💻

[![HitCount](http://hits.dwyl.com/hastagAB/GSoC-2020.svg)](http://hits.dwyl.com/hastagAB/GSoC-2020)
![GitHub](https://img.shields.io/github/followers/hastagAB?style=social)
![Twitter](https://img.shields.io/twitter/follow/HastagAB?style=social)
![GitHub Stars](https://img.shields.io/github/stars/hastagAB/GSoC-2020?style=social)



<h1 align="center">CONTRIBUTIONS  <img src="https://media.giphy.com/media/dxn6fRlTIShoeBr69N/giphy.gif" width="30"></h1>
<h2>1. Nirjas ~ নির্যাস</h2>
<p><i>A Python library for Comments and Source Code Extraction</i></p>

- Codebase: [GitHub](https://github.com/fossology/Nirjas)
- Library: [PyPI](https://pypi.org/project/Nirjas/)
- Documentation: [Nirjas-Wiki](https://github.com/fossology/Nirjas/wiki)

To scan for various open-source licenses inside of the file, one of the crucial parts is to extract the comments out of the code so that the base algorithm(agents) can detect the license. The license texts are in the comment section which makes it separate from the original codebase. 

I and Kaushlendra worked on developing a fully dedicated Python library from scratch for these tasks and managed to publish the initial version at PyPI before the first evaluation.

The major task was to classify different types of comments and to write separate logic for each one of them. The types are:

1. Single line comments
2. Multi-line comments
3. Continuous single lines (continuous lines commented out using single-line syntax at each line)
4. Inline comments (the comments that are written after the code on the same line)

The library can extract comments as well as code out of files from more than 20 popular programming languages. Along with that the library also serves you with all the required metadata about your Code, Comments and File(s). The library is available for public use and 

<img align="center" src="https://i.ibb.co/84G8PFX/nirjas.gif" alt="nirjas">

### Major Pull Requests

- [feat(extractor): add language indentifier #1](https://github.com/fossology/Nirjas/pull/1)
- [Complete development branch into master #2](https://github.com/fossology/Nirjas/pull/2)
- [feat(nirjas): Add continuous single line as multiline & bug fixes #6](https://github.com/fossology/Nirjas/pull/6)
- [Add Text File Support and Bug Fix #8](https://github.com/fossology/Nirjas/pull/8)

The complete list of Open and Closed PRs can be found at [Nirjas/Pull requests](https://github.com/fossology/Nirjas/pulls)

<h2>2. Integrating Nirjas with Atarashi</h2>
