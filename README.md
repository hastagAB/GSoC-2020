<h1 align="center">Google Summer of Code 2020 <img src="https://media2.giphy.com/media/KB8MHRUq55wjXVwWyl/source.gif" width="50"></h1>

<p align="center"><i>A full report on my Google Summer of Code 2020 work with FOSSology</i></p>
<p align="center"><i>Project: "Accelerating Atarashi" </i>  👨‍💻</p>

<p align="center">
        <img src="https://64.media.tumblr.com/c93f16953341ab06acd12b493659bdee/tumblr_mr68hhmVE11r5ikx8o1_400.gif" width="100">
</p>

![Logo](/Assets/GSoC-FOSSology.png)

<img align="center" alt="GIF" src="https://media.giphy.com/media/836HiJc7pgzy8iNXCn/giphy.gif" />

## Google Summer of Code 2020 🚩 Report On Project: "Accelerating Atarashi" 👨‍💻

[![HitCount](http://hits.dwyl.com/hastagAB/GSoC-2020.svg)](http://hits.dwyl.com/hastagAB/GSoC-2020)
![GitHub](https://img.shields.io/github/followers/hastagAB?style=social)
![Twitter](https://img.shields.io/twitter/follow/HastagAB?style=social)
![GitHub Stars](https://img.shields.io/github/stars/hastagAB/GSoC-2020?style=social)



<h1 align="center">CONTRIBUTIONS  <img src="https://media.giphy.com/media/dxn6fRlTIShoeBr69N/giphy.gif" width="30"></h1>
<h2>1. Nirjas ~ নির্যাস <img src="https://media.giphy.com/media/d9IfL7seBexHLct75B/giphy.gif" width="30"></h2>
<p><i>A Python library for Comments and Source Code Extraction</i></p>

- Codebase: [GitHub](https://github.com/fossology/Nirjas)
- Library: [PyPI](https://pypi.org/project/Nirjas/)
- Documentation: [Nirjas-Wiki](https://github.com/fossology/Nirjas/wiki)

To scan for various open-source licenses inside of the file, one of the crucial parts is to extract the comments out of the code so that the base algorithm(agents) can detect the license. The license texts are in the comment section which makes it separate from the original codebase. 

I and Kaushlendra worked on developing a fully dedicated Python library from scratch for these tasks and managed to publish the initial version at PyPI before the first evaluation.

Nirjas is live at PyPI and can be installed using `pip install nirjas`.

The major task was to classify different types of comments and to write separate logic for each one of them. The types are:

1. Single line comments
2. Multi-line comments
3. Continuous single lines (continuous lines commented out using single-line syntax at each line)
4. Inline comments (the comments that are written after the code on the same line)

The library can extract comments as well as code out of files from more than 20 popular programming languages. Along with that the library also serves you with all the required metadata about your Code, Comments and File(s). The library is available for public use and can be used in projects ranging from various domains.

<img align="center" src="https://i.ibb.co/84G8PFX/nirjas.gif" alt="nirjas">

### Major Pull Requests

- [feat(extractor): add language indentifier #1](https://github.com/fossology/Nirjas/pull/1)
- [Complete development branch into master #2](https://github.com/fossology/Nirjas/pull/2)
- [feat(nirjas): Add continuous single line as multiline & bug fixes #6](https://github.com/fossology/Nirjas/pull/6)
- [Add Text File Support and Bug Fix #8](https://github.com/fossology/Nirjas/pull/8)

The complete list of Open and Closed PRs can be found at [Nirjas/Pull requests](https://github.com/fossology/Nirjas/pulls)

<h2>2. Integrating Nirjas with Atarashi <img src="https://i.pinimg.com/originals/5a/0d/cd/5a0dcd8f92afeec3b2b27f617bf0a714.gif" width="80"></h2>

The next task was to replace existing code comment extractor with Nirjas in Atarashi.
At this point, the existing code comment extractor was not working at all and was throwing an error whenever an agent was called. So we took the right decision to create our code comment extractor.

Nirjas supports almost all the major languages currently and will be continuously developed and maintained by FOSSology itself.

The integration is done in such a way that it will extract and pass only those comments which contain a license statement. The comments classification by Nirjas played a big role here followed by our customized list of tokens which helps us find the actual license comment out of all other comments. Earlier the comment extractor used to pass all the comments which made the input string little bit noisier to detect.

A small change was done in the Evaluator where the testing files were zipped and the existing code was improved.

### Pull Request

- [feat(Nirjas): Replace code comment extractor with Nirjas #68](https://github.com/fossology/atarashi/pull/68)

<h2>3. Implementing Inverted Index with TF-IDF <img src="https://raw.githubusercontent.com/lhl/pusheen-stickers/master/gif/pusheen/144884865685780.gif" width="50"></h2>


The main idea was to create an inverted index for all the license texts and then use TF-IDF score to detect the licenses. This was supposed the decrease the detection time drastically and make agents faster. 

<img align="center" alt="Flowchart" src="https://i.ibb.co/yd5dTcK/Inv.jpg" width="500" />

The inverted index created is in the form:

```json
{
    "keyword1": [
        [
            "doc1",
            TF-IDF Score
        ],
        [
            "doc2",
            TF-IDF Score
        ]
    ],
    "keyword2": [
        [
            "doc3",
            TF-IDF Score
        ],
        [
            "doc2",
            TF-IDF Score
        ],
        [
            "doc'n'",
            TF-IDF Score
        ]
    ]
  
}
```

Then for every input comment, we are extracting the keywords and comparing their TF-IDF Scores with the posting inside of Inverted Index file. The documents having the closest TF-IDF scores are ranked in order and the top result is returned as our detected license.

Although the algorithm succeeded in decreasing the scanning time from around 1200 secs to 260 secs (for 100 files) unfortunately we were not able to increase the accuracy.
After applying various searching techniques, the maximum accuracy we got was 50% which is less than the original TF-IDF agent (i.e 59%).

![Inverted-Index with TF-IDF](https://i.ibb.co/Y0ss4QX/result.png)

According to me the two main factors that affect the performance of the algorithms(in terms of accuracy) are:

**1. Irregularity in the size of license texts**

*The license texts are of different sizes and there is a huge difference in terms of keywords count which abrupt the postings of the keywords. Longer texts contain most of the unique keywords which mess up with the uniqueness of keywords in the smaller texts. Due to this, the longer license texts dominate the resulting output. For better results, the texts should be normalized which will give equal opportunity to all the license texts.*

**2. License texts are different than traditional text corpora**

*Usually in the traditional corpus, the documents are different to some extent that differentiate them with each other. But in license texts, most of the tokens are similar in the majority of the texts and there is a very slight difference in the use of these token to create a license statement. The keywords used in these license texts can be found in almost all of them with a slight variation which makes sense because they all the eventually open-source licenses talking about open source software and permissions. These similarities in-licenses make them tough to be differentiated by any traditional information retrieval algorithm.*

### Codebase

- GitHub Repo: [TFIDF-Invterted-Index](https://github.com/hastagAB/TFIDF-Invterted-Index)

<h2>4. Creation of SPDX OSS license dataset <img src="https://2.bp.blogspot.com/-D2LNU-Zsfbc/WHZIdsOhXuI/AAAAAAAAIbc/eTt3Gohpelo14Niqx9nQ8mu35gUGbeW0wCLcB/s1600/online_student_learning_4545.gif" width="50"></h2>

To implement any Machine learning/Deep learning algorithm we need a better and bigger dataset of SPDX Licences.
But unfortunately, there exists no such dataset for open source licenses on the web.

To generate the dataset the base approach we used is to n-gram the paragraphs of license texts and to generate different permutations and combinations of them
Suppose a license text has 5 paragraphs [1,2,3,4,5] in order.
To create a dataset we include subsets like [1], [1,2], [1,2,3], [1,2,3,4], [1,2,3,4,5] for all combinations starting from 1,2,3,4 and 5. each one with the same label.

Using this technique we were able to generate more than 1 million files from 447 SPDX license files.

Now, as all the para are not equally important and most of these will actually create a lot of noise in dataset. To resolve this, we'll be choosing para with high relevance and then will repeat the same process.

Few Updation that we need to do is:

1. Shifting from txt files to SPDX JSON endpoint
2. Differentiating License Header from Full Text
3. Adding FOSSology Nomos agent STRINGS.in regex in dataset creation


### Codebase

- GitHub Repo: [SPDX OSS Dataset](https://github.com/hastagAB/SPDX-OSS-Dataset)

<h2>5. Documenting Nirjas & Atarashi <img src="https://www.kalpataru.com/images/default-source/knowledgehub/documentation/2.gif?Status=Temp&sfvrsn=6245648c_2" width="50"></h2>

During the GSoC period I got the time to create and organize documention for both Atarashi and Nirjas. The documentation contains all the user and developer information of the project and is organized in a way to be easily accisible by all.

The Documentation can be found at:

- Atarashi - [Atarashi GitHub Wiki](https://github.com/fossology/atarashi/wiki)
- Nirjas - [Nirjas GitHub Wiki](https://github.com/fossology/nirjas/wiki)


<h1>👨🏻‍🏫 DELIVERABLES <img src="https://api.ezeelo.com/Scripts/QRCode/Done.gif" width="40"></h1>

| Tasks   | Planned | Completed     | Remarks    |
| :---:       |    :----:   |    :---:      |    :---:      |
| Creating Nirjas     | Yes       | :heavy_check_mark: | Beta version is live & the project will be developed & maintained continuously |
| Publish to PyPI   | Yes        | :heavy_check_mark:  | Nirjas is live and can be installed and used in projects |
| Integrate Nirjas with Atarashi| Yes | :heavy_check_mark: | We are able to select specific license comment part from all comments |
| Implementing Inverted Index with TF-IDF | Yes | :heavy_check_mark: | Desired accuracy can not be achieved with this algorithm |
| Creating SPDX OSS Dataset | No | :heavy_check_mark: | dataset can be improved futher and the development is continuously going on |
| Implementing BERT (OPTIONAL) | Yes but was OPTIONAL | :x: | can only be implemented after the dataset creation |


<h1>🚀 FUTURE PLANS <img src="https://cdn-5e74a325f911c80ca0fe3f0d.closte.com/wp-content/uploads/2020/04/digital-marketing-london-4-1.gif" width="50"></h1>


1. Implement complete regex in Nirjas covering most of the boundry cases.
2. Improving the created SPDX OSS Dataset 
3. Continue developing Nirjas and Atarashi
4. Maintaining Nirjas and Atarashi
5. Searching for other methods to be implemented for license scanning


# 
<h1>📚 Things I learned from Google Summer of Code <img src="https://media1.tenor.com/images/1702679abb72b8bc113028f96c5d20d3/tenor.gif?itemid=10085626" width="60"></h1>


* Learned about various NLP techniques by studying, testing and implementing them
* various Open-Source licenses and their Importance in codes, projects and softwares.
* Learned to develop a complete library from scracth
* Pacakaging of Python Projects and how it is maintained
* Sharpened my skill of GIT
* Various Information retrieval algorithms & traditional searching techniques
* Learned to create a better and cleaner dataset.
* Improved my knowledge of Data Science
* Learned the importance of time management as well as perfect deliverables.
* Improved my documentation skill
* Improved my communication & presentation skill
