# AzureTweetMigration

Tweet Migration is a 3D visualization of tweets.

## How to run

1. Fork or clone this repo

```
git clone git@github.com:Yonet/AzureTweetMigration.git


```

2. Create local.settings.json file at the root level of your project. If you follow the steps-0, this file will be automatically created for you.

```js
{
"IsEncrypted": false,

"Values": {
"AzureWebJobsStorage": "",

"FUNCTIONS_WORKER_RUNTIME": "node"

}
}
```

3. Install dependencies

```
npm install
```

4. Run npm start script

```
npm run start
```

## [Step-0](https://github.com/Yonet/AzureTweetMigration/tree/step-0)

In this step, we will setup our project and create an [Azure Function](https://docs.microsoft.com/azure/azure-functions/?WT.mc_id=aiml-0000-ayyonet) to get [Twitter Search](https://developer.twitter.com/en/docs.html) data into our application. You can follow the below steps or check out "step-0" branch of this repo.

-   Download

*   [Node.js latest LTS version](https://nodejs.org/en/download/). Latest Node is not compatible with the tools we are using and not recommended for most users.

*   [Visual Studio Code 2019](https://visualstudio.microsoft.com/downloads/?WT.mc_id=aiml-0000-ayyonet)

*   [Visual Studio Code Azure Extensions](https://docs.microsoft.com/azure/azure-functions/functions-run-local?WT.mc_id=aiml-0000-ayyonet) or [Azure Functions Extension](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions&WT.mc_id=aiml-0000-ayyonet)

-   Sign up for a [free Azure account](https://azure.microsoft.com/free/?WT.mc_id=aiml-0000-ayyonet)


*   Select Azure Extensions from VS Code [left hand panel](/images/azureToolbar.png).

*   Select [Functions grouping](/images/functionsRepos.png), click on folder icon on the Functions grouping to create a new project. Select a folder or create a new folder for your project repository.

*   To create a function select the second icon on the Functions grouping.

*   You will be prompted to sign in if you have [Azure Tools](https://docs.microsoft.com/azure/azure-functions/functions-run-local?WT.mc_id=aiml-0000-ayyonet) or [Azure Repos](https://github.com/Microsoft/azure-repos-vscode/blob/master/TFVC_README.md?WT.mc_id=azuretweetmigration-github-ayyonet#quick-start) extensions.

*   When prompted, name your function "SearchTweets". Language "TypeScript".

*   When prompted, select "Anonymous". This will make our API public.

*   [Azure Functions Extension](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions&WT.mc_id=aiml-0000-ayyonet) will create our initial configuration files as well as SearchTweet folder with our first Azure function.

*   To run the code you can open up the internal terminal from the Command Palette (⇧⌘P), use the View: Toggle Integrated Terminal command.

*   Run npm start script

```
npm run start
```

-   Start script will download extensions configured in your 'functions.json' file and start a serve your project.

-   In your terminal, you will see a localhost link 'http://localhost:7071/api/SearchTweets'. Click the link to open in a browser.

-   You will see the error message being displayed. This message is set on ['SearchTweet/index.ts'](https://github.com/Yonet/AzureTweetMigration/blob/179f3301b20e7914733291adff208c8605b17687/SearchTweets/index.ts#L16) file.

-   Add a name query value to see the message returned.

```
http://localhost:7071/api/SearchTweets?name=aysegul
```

## Step-1

Adding Twitter Api: we will add twitter authorization and search functionality. We will refactor our code to make sure when the our projects gets bigger, it is still maintainable and structured well.

-   Sign up for [Twitter Developer Api](https://developer.twitter.com/en.html)

-   We will be using the [standard search api](https://developer.twitter.com/en/docs/tweets/search/api-reference/get-search-tweets.html). Add tokens and secrets as well as the search url we will be calling to local.settings.json file.

```ts
{
    "consumer_key": "fs54YbjfXuQWJRs01XatGR",
    "consumer_secret": "lzXoL4GzxGYeItJ327BGcNmZcmPhgjgft62x0tHuZYmad",
    "access_token_key": "35870660-0250Io0UYm5NHtutgM7bq57h9aChvA30FIBYV0j1q",
    "access_token_secret": "RFab22sgMgg9DlwoWRZrQRIizQZultYvo2Ek9C0Xp",
    "search_url": "https://api.twitter.com/1.1/search/tweets.json"
}

```

You can add your keys in any other structure if you like. I can reach to my keys from my functions as:

```ts
process.env.Twitter.consumer_key;
```

- Authentication: we will use [application-only authentication](https://developer.twitter.com/en/docs/basics/authentication/overview/application-only). Application-only authentication offers applications the ability to issue authenticated requests on behalf of the application itself (as opposed to on behalf of a specific user). We need to acquire bearer token using our consumer key and secret.

![Auth](/images/appauth_0.png)