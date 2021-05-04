# Course automation: Post hemingway editor score, readability and feedback

## Members

William Skagerström (wska@kth.se) (@wska)
Alexander Kruger (alekru@kth.se) (@TheStar19)
## Introduction

This contribution takes the form of a bot which is capable of providing feedback on an essay submission by utilizing the online-hosted hemingway editor (https://hemingwayapp.com/). The hemingway editor will provide some general information about the text that was published, including:
* A approximative grade of the readability of the paper (generated by a heuristic)
* Word count
* Reading time
* Quantity of adverbs utilized
* Uses of a passive voice
* Phrases which could be rewritten in a simpler manner
* Number of difficult to read sentences
* Number of VERY difficult to read sentences

## General functionality
The program performs the following steps when execution initiates:
1. Scans the pull requests available on the designated repo and looks for comments including the "!hemingway" tag.
2. If the pull request contains a pdf file, it will download it and convert it into a raw text file using pdf2txt.py library (https://pypi.org/project/pdf2text/). 
3. The text will then pass into a hemingway function, which utilizes selenium and google chrome to open a web browser and input the raw text (https://selenium-python.readthedocs.io/).
4. It will then fetch the relevant feedback provided by the hemingway editor and create a short summary.
5. This is then returned and posted as a comment to the original pull request using a dedicated bot-account. Git interactions are done using pygithub (https://pygithub.readthedocs.io/en/latest/index.html).

An example of a PR submission which is scanned and provided with the hemingway scores can be viewed under active pull requests.

## How to use
The bot will trigger its search upon seeing a pull request by default. In order to obtain hemingway feedback, one needs to either include a comment on the pull request which includes "!hemingway" (with a exclamation mark), or include the phrase in the body of the pull request.

The search can also be manually started by going into Github Actions and scheduling the "hemingway'' job. This might be necessary if one makes a comment after a pull request and wants to obtain feedback.

### Configuration
To configure the bot, place the entire repo in one folder. Depending on your repo, the path to pdf2txt.py may be different, please adjust by noticing from the debug text where it attempts to run the pdf2txt.py file and adjust the path on line 120 accordingly.

The heminwayfeedback.yml file needs to be in root/.github.

To start the bot, run python3 main.py TOKENID REPO_ID REPO_NAME. 
This may need to be adjusted in the hemingwayfeedback.yml depending on your setup where line 50 needs to be run: python ./main.py $SUPER_SECRET REPO_ID REPO_NAME or set in main.py manually.

One way to obtain the REPO_ID is to inspect a repository through a modern web browser and paste the following command into the console:

> $("meta[name=octolytics-dimension-repository_id]").getAttribute('content')


<img src="exampleIdOnChrome.PNG" width="400" />


This also makes it possible to target more than one repo at once. The $SUPER_SECRET can be a access token or the GITHUB_TOKEN which is generated for GitHub actions. See [this page for more information.](https://docs.github.com/en/actions/reference/authentication-in-a-workflow)

[An example execution may be viewed at the following PR](https://github.com/wska/hemingwayAutoFeedback/pull/6)
