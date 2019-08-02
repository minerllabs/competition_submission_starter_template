# NeurIPS 2019: MineRL Competition Starter Kit

[![Discourse status](https://img.shields.io/discourse/https/discourse.aicrowd.com/status.svg)](https://discourse.aicrowd.com/) [![Discord](https://img.shields.io/discord/565639094860775436.svg)](https://discord.gg/BT9uegr)



This repository is the main MineRL Competition **submission template and starter kit**! Compete to solve `MineRLObtainDiamond-v0` now!

**This repository contains**:
*  **Documentation** on how to submit your agent to the leaderboard
*  **The procedure** for Round 1 (how long you should train your agent, how we evaluate and re-train your agent, etc.)
*  **Starter code** for you to base your submission!

**Other Resources**:
- [MineRL Competition Page](https://www.aicrowd.com/challenges/neurips-2019-minerl-competition) - Main registration page & leaderboard.
- [MineRL Documentation](http://minerl.io/docs) - Documentation for the `minerl` package and dataset!
- [Example Baselines](https://github.com/minerllabs/baselines) - A set of competition and non-competition baselines for `minerl`.



![](https://i.imgur.com/XB1WORT.gif)


#  Competition Procedure: Round 1

Welcome to Round 1! The following is a high level description of how this round works.

![](http://minerl.io/assets/images/round1_procedure.png)


1. **Sign up** to join the competition [on the AIcrowd website.](https://www.aicrowd.com/challenges/neurips-2019-minerl-competition)
2. **Clone** this repo  and start developing your submissions.

3. **Train** your models against `MineRLObtainDiamond-v0` using the `train_locally.sh` or on Azure  with **only 8,000,000 samples** in less than **four days** using hardware **no powerful than a NG6v2 instance** (6 CPU cores, 112 GiB RAM, 736 GiB SDD, and a single NVIDIA P100 GPU.) 

3. [**Submit**]() your trained models to [AIcrowd Gitlab](https://gitlab.aicrowd.com) for evaluation [(full instructions below)]().  The automated evaluation setup will evaluate the submissions against the validation environment, to compute and report the metrics on the leaderboard of the competition.

Once Round 1 is complete, the organizers will:

1. **Examine** the code repositories of the top submissions on the leaderboard to ensure compliance with the competition rules.
2. **Retrain** the top submissions from scratch to ensure reproducibility of the leaderboard score!  **NOTE: Make sure that you train your models in UNDER 8,000,000 samples using a similar (or worse) hardware spec than above** so that you are **not disqualified** for a score mismatch!

2. **Evaluate** the resulting models again over several hundred episodes to determine the final ranking.

The code repositories associated with the corresponding submissions will be forked and scrubbed of any files larger than 15MB to ensure that participants are not using any pre-trained models in the subsequent round.  


# How to Submit a Model!

## Setup



1.  **Clone the github repository** or press the "Use this Template" button on GitHub!

    ```
    git clone https://github.com/minerllabs/competition_submission_starter_template.git
    ```

2. **Install** competition specific dependencies! **Make sure you have the [JDK 8 installed first](http://minerl.io/docs/tutorials/getting_started.html)!**
    ```
    # 1. Make sure to install the JDK first
    # -> Go to http://minerl.io/docs/tutorials/getting_started.html

    # 2. Install the `minerl` package and the dependencies for the competition
    cd competition_submission_starter_template
    pip3 install -r requirements.txt
    ```

3. **Specify** your specific submission dependencies (PyTorch, Tensorflow, kittens, etc.)

    * (Optional) **Anaconda Environment**. If you would like to use anaconda to manage your environment, make sure at least version `4.5.11` is required to correctly populate `environment.yml` (By following instructions [here](https://www.anaconda.com/download)). Then:
        * **Create your new conda environment**

            ```sh
            conda create --name minerl_challenge
            conda activate minerl_challenge
            ```

      * **Your code specific dependencies**
        ```sh
        conda install <your-package>
        ```

    * **Pip Packages** If you are using specific Python packages **make sure to add them to** `requirements.txt`! Here's an example:
      ```
      # requirements.txt
      minerl>0.2.3
      
      matplotlib
      tensorflow
      ```
    * **Apt Packages** If your training procedure or agent depends on specific Debian (Ubuntu, etc.) packages, add them to `apt.txt`.



### Your Submission

To get started with the environment and dataset [please check out our quick start guide here](http://minerl.io/docs/tutorials/getting_started.html)!

This competition uses the [MineRL](http://minerl.io/docs)  Gym environments based on Malmo. The environment and dataset loader will be available through a pip package.

The main task of the competition is solving the `MineRLObtainDiamond-v0` environment. In this environment, the agent begins in a random starting location without any items, and is tasked with obtaining a diamond. This task can only be accomplished by navigating the complex item hierarchy of Minecraft.


## How do I specify my software runtime ?

The software runtime is specified by exporting your `conda` env to the root
of your repository by doing :

```
conda env export --no-build > environment.yml

# It might make sense here to remove the requirements.txt here to avoid confusion as environment.yml takes precedence over requirements.txt
```

This `environment.yml` file will be used to recreate the `conda environment`. This repository includes an example `environment.yml`

## What should my code structure be like ?

Please follow the example structure shared in the starter kit for the code structure.
The different files and directories have following meaning:

```
.
├── aicrowd.json           # Submission meta information like your username
├── apt.txt                # Packages to be installed inside docker image
├── data                   # The downloaded data, the path to directory is also available as `MINERL_DATA_ROOT` env variable
├── requirements.txt       # Python packages to be installed
├── run.sh                 # The default entry point which will execute your submission
├── test.py                # IMPORTANT: Your testing/inference phase code
├── train                  # Your trained model can be saved inside this directory
├── train.py               # IMPORTANT: Your training phase code
└── utility                # The utility scripts to provide smoother experience to you.
    ├── debug_build.sh
    ├── docker_run.sh
    ├── environ.sh
    ├── evaluation_locally.sh
    ├── parser.py
    ├── train_locally.sh
    └── verify_or_download_data.sh
```

## Important Concepts

### Repository Structure

- `aicrowd.json`
  Each repository should have a `aicrowd.json` with the following content :

```json
{
  "challenge_id": "aicrowd-neurips-2019-minerl-challenge",
  "grader_id": "aicrowd-neurips-2019-minerl-challenge",
  "authors": ["your-aicrowd-username"],
  "description": "sample description about your awesome agent",
  "license": "MIT",
  "gpu": true
}
```

This is used to map your submission to the said challenge, so please remember to use the correct `challenge_id` and `grader_id` as specified above.

Please specify if your code will a GPU or not for the evaluation of your model. If you specify `true` for the GPU, a **NVIDIA Tesla K80 GPU** will be provided and used for the evaluation.

### Packaging of your software environment

The recommended way is to use Anaconda configuration files using **environment.yml** files.

```sh
# The included environment.yml is generated by the command below, and you do not need to run it again
# if you did not add any custom dependencies

conda env export --no-build > environment.yml

# Note the `--no-build` flag, which is important if you want your anaconda env to be replicable across all
# Or if you have a rather simple softwar runtime, then even a :
#
# pip freeze > requirements.txt
#
# could work.
```

### Code Entrypoint

The evaluator will use `/home/aicrowd/run.sh` as the entrypoint, so please remember to have a `run.sh` at the root, which can instantitate any necessary environment variables, and also start executing your actual code. This repository includes a sample `run.sh` file.
If you are using a Dockerfile to specify your software environment, please remember to create a `aicrowd` user, and place the entrypoint code at `run.sh`.

The `run.sh` in turn calls your training and testing code present in `train.py` & `test.py` respectively. The inline documentation in these files will guide you in interfacing with evaluator properly.

## Dataset location

You **don't** need to upload the data set in submission and it will be provided in online submissions at `MINERL_DATA_ROOT` path. For local training and evaluations, you can download it once in your system via `./utility/verify_or_download_data.sh` or place manually into `data/` folder.

## Time constraints

### Round 1

You have to train your models locally **with under 8,000,000 samples** and with **worse or comprable hardware to that above** and upload the trained model in `train/` directory. But, to make sure, your training code is compatible with further round's interface, the training code will be executed in this round as well. The constraints will be timeout of 5 minutes.

### Round 2

You are expected to train your model online using the training phase docker container and output the trained model in `train/` directory. You need to ensure that your submission is trained in under 8,000,000 samples and within 4 days period. Otherwise, the container will be killed

## Local evaluation

You can perform local training and evaluation using utility scripts shared in this directory. To mimic the online training phase you can run `./utility/train_locally.sh` from repository root, you can specify `--verbose` for complete logs.

```
aicrowd_minerl_starter_kit❯ ./utility/train_locally.sh --verbose
2019-07-22 07:58:38 root[77310] INFO Training Start...
2019-07-22 07:58:38 crowdai_api.events[77310] DEBUG Registering crowdAI API Event : CROWDAI_EVENT_INFO training_started {'event_type': 'minerl_challenge:training_started'} # with_oracle? : False
2019-07-22 07:58:40 minerl.env.malmo.instance.17c149[77310] INFO Starting Minecraft process: ['/var/folders/82/wsds_18s5dq321scc1j531m40000gn/T/tmpnyzpjrsc/Minecraft/launchClient.sh', '-port', '9001', '-env', '-runDir', '/var/folders/82/wsds_18s5dq321scc1j531m40000gn/T/tmpnyzpjrsc/Minecraft/run']
2019-07-22 07:58:40 minerl.env.malmo.instance.17c149[77310] INFO Starting process watcher for process 77322 @ localhost:9001
2019-07-22 07:58:48 minerl.env.malmo.instance.17c149[77310] DEBUG This mapping 'snapshot_20161220' was designed for MC 1.11! Use at your own peril.
2019-07-22 07:58:48 minerl.env.malmo.instance.17c149[77310] DEBUG #################################################
2019-07-22 07:58:48 minerl.env.malmo.instance.17c149[77310] DEBUG          ForgeGradle 2.2-SNAPSHOT-3966cea
2019-07-22 07:58:48 minerl.env.malmo.instance.17c149[77310] DEBUG   https://github.com/MinecraftForge/ForgeGradle
2019-07-22 07:58:48 minerl.env.malmo.instance.17c149[77310] DEBUG #################################################
2019-07-22 07:58:48 minerl.env.malmo.instance.17c149[77310] DEBUG                Powered by MCP unknown
2019-07-22 07:58:48 minerl.env.malmo.instance.17c149[77310] DEBUG              http://modcoderpack.com
2019-07-22 07:58:48 minerl.env.malmo.instance.17c149[77310] DEBUG          by: Searge, ProfMobius, Fesh0r,
2019-07-22 07:58:48 minerl.env.malmo.instance.17c149[77310] DEBUG          R4wk, ZeuX, IngisKahn, bspkrs
2019-07-22 07:58:48 minerl.env.malmo.instance.17c149[77310] DEBUG #################################################
2019-07-22 07:58:48 minerl.env.malmo.instance.17c149[77310] DEBUG Found AccessTransformer: malmomod_at.cfg
2019-07-22 07:58:49 minerl.env.malmo.instance.17c149[77310] DEBUG :deobfCompileDummyTask
2019-07-22 07:58:49 minerl.env.malmo.instance.17c149[77310] DEBUG :deobfProvidedDummyTask
...
```

For local evaluation of your code, you can use `./utility/evaluation_locally.sh`, add `--verbose` if you want to view complete logs.

```
aicrowd_minerl_starter_kit❯ ./utility/evaluation_locally.sh
{'state': 'RUNNING', 'score': {'score': 0.0, 'score_secondary': 0.0}, 'instances': {'1': {'totalNumberSteps': 1001, 'totalNumberEpisodes': 0, 'currentEnvironment': 'MineRLObtainDiamond-v0', 'state': 'IN_PROGRESS', 'episodes': [{'numTicks': 1001, 'environment': 'MineRLObtainDiamond-v0', 'rewards': 0.0, 'state': 'IN_PROGRESS'}], 'score': {'score': 0.0, 'score_secondary': 0.0}}}}
{'state': 'RUNNING', 'score': {'score': 0.0, 'score_secondary': 0.0}, 'instances': {'1': {'totalNumberSteps': 2001, 'totalNumberEpisodes': 0, 'currentEnvironment': 'MineRLObtainDiamond-v0', 'state': 'IN_PROGRESS', 'episodes': [{'numTicks': 2001, 'environment': 'MineRLObtainDiamond-v0', 'rewards': 0.0, 'state': 'IN_PROGRESS'}], 'score': {'score': 0.0, 'score_secondary': 0.0}}}}
{'state': 'RUNNING', 'score': {'score': 0.0, 'score_secondary': 0.0}, 'instances': {'1': {'totalNumberSteps': 3001, 'totalNumberEpisodes': 0, 'currentEnvironment': 'MineRLObtainDiamond-v0', 'state': 'IN_PROGRESS', 'episodes': [{'numTicks': 3001, 'environment': 'MineRLObtainDiamond-v0', 'rewards': 0.0, 'state': 'IN_PROGRESS'}], 'score': {'score': 0.0, 'score_secondary': 0.0}}}}
{'state': 'RUNNING', 'score': {'score': 0.0, 'score_secondary': 0.0}, 'instances': {'1': {'totalNumberSteps': 4001, 'totalNumberEpisodes': 0, 'currentEnvironment': 'MineRLObtainDiamond-v0', 'state': 'IN_PROGRESS', 'episodes': [{'numTicks': 4001, 'environment': 'MineRLObtainDiamond-v0', 'rewards': 0.0, 'state': 'IN_PROGRESS'}], 'score': {'score': 0.0, 'score_secondary': 0.0}}}}
{'state': 'RUNNING', 'score': {'score': 0.0, 'score_secondary': 0.0}, 'instances': {'1': {'totalNumberSteps': 5001, 'totalNumberEpisodes': 0, 'currentEnvironment': 'MineRLObtainDiamond-v0', 'state': 'IN_PROGRESS', 'episodes': [{'numTicks': 5001, 'environment': 'MineRLObtainDiamond-v0', 'rewards': 0.0, 'state': 'IN_PROGRESS'}], 'score': {'score': 0.0, 'score_secondary': 0.0}}}}
{'state': 'RUNNING', 'score': {'score': 0.0, 'score_secondary': 0.0}, 'instances': {'1': {'totalNumberSteps': 6001, 'totalNumberEpisodes': 0, 'currentEnvironment': 'MineRLObtainDiamond-v0', 'state': 'IN_PROGRESS', 'episodes': [{'numTicks': 6001, 'environment': 'MineRLObtainDiamond-v0', 'rewards': 0.0, 'state': 'IN_PROGRESS'}], 'score': {'score': 0.0, 'score_secondary': 0.0}}}}
...
```

The steps to mimic above in a docker environment will be added shortly.

## Submission

To make a submission, you will have to create a private repository on [https://gitlab.aicrowd.com/](https://gitlab.aicrowd.com/).

You will have to add your SSH Keys to your GitLab account by following the instructions [here](https://docs.gitlab.com/ee/gitlab-basics/create-your-ssh-keys.html).
If you do not have SSH Keys, you will first need to [generate one](https://docs.gitlab.com/ee/ssh/README.html#generating-a-new-ssh-key-pair).

Then you can create a submission by making a _tag push_ to your repository on [https://gitlab.aicrowd.com/](https://gitlab.aicrowd.com/).
**Any tag push (where the tag name begins with "submission-") to your private repository is considered as a submission**  
Then you can add the correct git remote, and finally submit by doing :

```
cd neurips2019_minerl_challenge_starter_kit
# Add AIcrowd git remote endpoint
git remote add aicrowd git@gitlab.aicrowd.com:<YOUR_AICROWD_USER_NAME>/neurips2019_minerl_challenge_starter_kit.git
git push aicrowd master

# Create a tag for your submission and push
git tag -am "submission-v0.1" submission-v0.1
git push aicrowd master
git push aicrowd submission-v0.1

# Note : If the contents of your repository (latest commit hash) does not change,
# then pushing a new tag will **not** trigger a new evaluation.
```

You now should be able to see the details of your submission at :
[gitlab.aicrowd.com/<YOUR_AICROWD_USER_NAME>/neurips2019_minerl_challenge_starter_kit/issues](gitlab.aicrowd.com//<YOUR_AICROWD_USER_NAME>/neurips2019_minerl_challenge_starter_kit/issues)

**NOTE**: Remember to update your username in the link above :wink:

In the link above, you should start seeing something like this take shape (each of the steps can take a bit of time, so please be patient too :wink: ) :
![](https://i.imgur.com/FqScw4m.png)

and if everything works out correctly, then you should be able to see the final scores like this :
![](https://i.imgur.com/u00qcif.png)

**Best of Luck** :tada: :tada:

# Team

The quick-start kit was authored by 
**[Shivam Khandelwal](https://twitter.com/skbly7)** with help from [William H. Guss](http://wguss.ml)

The competition is organized by the following team:

* [William H. Guss](http://wguss.ml) (Carnegie Mellon University)
* Mario Ynocente Castro (Preferred Networks)
* Cayden Codel (Carnegie Mellon University)
* Katja Hofmann (Microsoft Research)
* Brandon Houghton (Carnegie Mellon University)
* Noboru Kuno (Microsoft Research)
* Crissman Loomis (Preferred Networks)
* Keisuke Nakata (Preferred Networks)
* Stephanie Milani (University of Maryland, Baltimore County and Carnegie Mellon University)
* Sharada Mohanty (AIcrowd)
* Diego Perez Liebana (Queen Mary University of London)
* Ruslan Salakhutdinov (Carnegie Mellon University)
* Shinya Shiroshita (Preferred Networks)
* Nicholay Topin (Carnegie Mellon University)
* Avinash Ummadisingu (Preferred Networks)
* Manuela Veloso (Carnegie Mellon University)
* Phillip Wang (Carnegie Mellon University)


<img src="https://d3000t1r8yrm6n.cloudfront.net/images/challenge_partners/image_file/35/CMU_wordmark_1500px-min.png" width="50%"> 

  <img src="https://d3000t1r8yrm6n.cloudfront.net/images/challenge_partners/image_file/34/MSFT_logo_rgb_C-Gray.png" width="20%" style="margin-top:10px">

 <img src="https://raw.githubusercontent.com/AIcrowd/AIcrowd/master/app/assets/images/misc/aicrowd-horizontal.png" width="20%">   <img src="https://d3000t1r8yrm6n.cloudfront.net/images/challenge_partners/image_file/38/PFN_logo.png" width="15%" style="margin-top:10px">
