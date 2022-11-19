# **Gitopia Mirror Action**

Gitopia Mirror Action is a GitHub action for mirroring your GitHub repoistories automatically to another remote repository on Gitopia. Branches, tags, and commits can be mirrored.

Gitopia Mirror Action is available in the GitHub Marketplace.

### Pre-requisite:

To use the Gitopia Mirror Action:

- You need to have a repository on GitHub
- You must have at least the Maintainer role for the project
- You need to have a Gitopia wallet file

## 1. To get started, create a new repository on Gitopia.

<img width="1422" alt="gito1" src="https://user-images.githubusercontent.com/78480857/202858715-a88c592f-e74f-4932-afcd-7a34490d7311.png">


## 2. Once successful, you would be redirected to the new repository page.

<img width="1422" alt="gito2" src="https://user-images.githubusercontent.com/78480857/202858721-03da12bc-f3f2-4ddc-9e7a-7777c305f54c.png">


## 3. Copy the remote URL from the repo page, as suggested in the image below.

<img width="1422" alt="gito3" src="https://user-images.githubusercontent.com/78480857/202858725-a07f2073-2a4e-434c-9a71-f1f4b1b0d188.png">



Sample Url: ```gitopia://gitopia130xtlqs7jxggp7emxmhzc00axpy658qwy804ud/hello-world```



## 4. Also download the wallet file by clicking on Download wallet as shown in the image.

<img width="1422" alt="gito4" src="https://user-images.githubusercontent.com/78480857/202858729-095eb265-3d94-490d-b9b1-5f947287258a.png">


## 5. Navigate to the GitHub repository you want to migrate to Gitopia on your browser.

<img width="1422" alt="gito5" src="https://user-images.githubusercontent.com/78480857/202858731-78076baf-e3f8-47bd-93cd-890fd7d3b886.png">


## 6. You need to add Gitopia wallet contents to GitHub secrets called GITOPIA_WALLET. To do this, navigate to the repository *settings* and then click on Secrets as shown in the image.

<img width="1422" alt="gito7" src="https://user-images.githubusercontent.com/78480857/202858739-aed576d9-d91e-4e02-a07b-6f2bd7efb6bb.png">


## 7. Click on Actions under Secrets. This will help you create Secrets for your repository. Secrets are environment variables that are encrypted.

<img width="1422" alt="gito8" src="https://user-images.githubusercontent.com/78480857/202858745-e7d5d930-a7c5-4f6c-b46d-52dd18e40318.png">

## 8. Click on New repository secret.

<img width="1422" alt="gito9" src="https://user-images.githubusercontent.com/78480857/202858748-fa75a642-fdee-4c34-bd4f-74228a9cbded.png">

**It will take you to the Action Secret creation page.**

<img width="1422" alt="gito10" src="https://user-images.githubusercontent.com/78480857/202858754-275bc324-7e7c-4ca5-8253-86f6e7730c0d.png">


## 9. Fill in the details as shown below.


```
- **Name:** GITOPIA_WALLET

- **Value:** Paste the contents for your Gitopia Wallet file here
```

A typical wallet file content is shown below:

```
{
  "name": "gitopia-wallet",
  "mnemonic": "magic practice outdoor car vacant unveil lamp absurd cliff region grape burst always creek cat ill hand magic scorpion acquire seminar jump pen hand",
  "HDpath": "m/44'/118'/0'/0/",
  "password": "aaaaaaaa",
  "prefix": "gitopia",
  "pathIncrement": 0,
  "accounts": [
    {
      "address": "gitopia130xtlqs7jxggp7emxmhzc00axpy658qwy804ud",
      "pathIncrement": 0
    }
  ]
}
```


## 10. Click on Add Secret.

<img width="1422" alt="gito11" src="https://user-images.githubusercontent.com/78480857/202858764-51df9e8c-8332-4d9e-bc18-8b0cd88d69d6.png">


**Your repository now has a Action Secret named** ```GITOPIA_WALLET```. **This step is crucial for the Gitopia Mirror Action to function.**


<img width="1422" alt="gito12" src="https://user-images.githubusercontent.com/78480857/202858773-9fa114ee-6ee2-4fd4-81eb-380a0339b9fd.png">


## 11. Click on Actions to navigate to the GitHub Actions sections of your repository.

<img width="1422" alt="gito13" src="https://user-images.githubusercontent.com/78480857/202858783-a89e2e45-2d25-43a8-aebb-df4139451bec.png">


## 12. Click on set up a workflow yourself.

<img width="1422" alt="gito14" src="https://user-images.githubusercontent.com/78480857/202858789-cc476e9b-c493-4940-be76-d9145063e873.png">


**This will help to create a** ```main.yml``` **file in the** ```.github/workflows/``` **directory of your repository. The directory will be created automatically if it does not exist.**

<img width="1422" alt="gito15" src="https://user-images.githubusercontent.com/78480857/202858794-d45079cc-f416-46bb-933e-c74bb17c1859.png">



## 13. Change the file name to ```gitopia-mirror.yml``` and its contents to:

```
name: Mirror to Gitopia

on:
  push:
    branches:
      - '**'

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v2
        with:
          fetch-depth: 0
      - name: Push to Gitopia mirror
        uses: gitopia/gitopia-mirror-action@v0.5.0
        with:
          gitopiaWallet: "${{ secrets.GITOPIA_WALLET }}"
          remoteUrl: "gitopia://gitopia10j4ryjjna69hvsrahgvhwjv3dd46tt6xuprguq/gitopia-mirror-action"
          force: false
```


**Input details:**
- ```gitopiaWallet```: Your wallet file saved as a GitHub secret. Add Gitopia wallet contents to GitHub secrets called ```GITOPIA_WALLET``` as shown in the previous steps.

- ```remoteUrl```: Your Gitopia repository ```remote_url``` created from https://gitopia.com

- ```force``` (optional): true/false, Force push to gitopia remote repository? Default is ```false```, setting this to true will allow the mirror action to overwrite your gitopia remote repository. The changes done directly in your gitopia remote repository will be overwritten.

**NOTE**
The checkout action that we use above, by default, checks out a shallow copy of your repo without history. fetch-depth: 0 configures it to checkout the branch with history.

```
on:
  push:
    branches:
      - '**'
```


In the above snippet, push to any branch will trigger the mirror action. You can change ** to a specific branch and only that branch will be used in the mirror action and get pushed to Gitopia.



Your file, ```gitopia-mirror.yml```, should now look like this:


<img width="1422" alt="gito16" src="https://user-images.githubusercontent.com/78480857/202858796-c20cbeee-6274-4172-838a-adba78d3e49a.png">



## 14. Commit the changes to your repository by clicking on Start commit and then Commit new file after filling in the required details.

<img width="1422" alt="gito17" src="https://user-images.githubusercontent.com/78480857/202858807-1b6287b4-d392-4475-a1be-442f91e83d71.png">




Your repository will now have a ```.github/workflows/``` directory with a file ```gitopia-mirror.yml``` in it.


<img width="1422" alt="gito18" src="https://user-images.githubusercontent.com/78480857/202858817-8207fd72-8f99-49cf-9b1e-ffb4200390c2.png">


## 15. Commiting the changes will triger a Github Action as described in the above ```.yml```. In the future, pushing any branch to GitHub will trigger the Github Action.

<img width="1422" alt="gito19" src="https://user-images.githubusercontent.com/78480857/202858823-a8f70f24-55ce-4a29-ab46-cca1bcf9f32f.png">


## 16. As you can see in the above image, the code has been pushed to Gitopia. Now, we wait for the GItopia transaction to confirm, and voila - you can see your repository directory and files on Gitopia:

<img width="1422" alt="gito20" src="https://user-images.githubusercontent.com/78480857/202858825-6980239c-0aab-45e0-8309-4b541869c0f9.png">

