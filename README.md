<img height="100" alt="hyperexecute_logo" src="https://user-images.githubusercontent.com/1688653/159473714-384e60ba-d830-435e-a33f-730df3c3ebc6.png">

HyperExecute is a smart test orchestration platform to run end-to-end Selenium tests at the fastest speed possible. HyperExecute lets you achieve an accelerated time to market by providing a test infrastructure that offers optimal speed, test orchestration, and detailed execution logs.

The overall experience helps teams test code and fix issues at a much faster pace. HyperExecute is configured using a YAML file. Instead of moving the Hub close to you, HyperExecute brings the test scripts close to the Hub!

* <b>HyperExecute HomePage</b>: https://www.lambdatest.com/hyperexecute
* <b>Lambdatest HomePage</b>: https://www.lambdatest.com
* <b>LambdaTest Support</b>: [support@lambdatest.com](mailto:support@lambdatest.com)

To know more about how HyperExecute does intelligent Test Orchestration, do check out [HyperExecute Getting Started Guide](https://www.lambdatest.com/support/docs/getting-started-with-hyperexecute/)


[<img alt="Try it now" width="200 px" align="center" src="images/Try it Now.svg" />](https://hyperexecute.lambdatest.com/?utm_source=github&utm_medium=repository&utm_content=python&utm_term=pyunit)

## Gitpod

Follow the below steps to run Gitpod button:

1. Click '**Open in Gitpod**' button (You will be redirected to Login/Signup page).
2. Login with Lambdatest credentials and it will be redirected to Gitpod editor in new tab and current tab will show hyperexecute dashboard.

[<img alt="Run in Gitpod" width="200 px" align="center" src="images/Gitpod.svg" />](https://hyperexecute.lambdatest.com/?type=gitpod&framework=PyUnit)
---
<!---If logged in, it will be redirected to Gitpod editor in new tab where current tab will show hyperexecute dashboard.

If not logged in, it will be redirected to Login/Signup page and simultaneously redirected to Gitpod editor in a new tab where current tab will show hyperexecute dashboard.

If not signed up, you need to sign up and simultaneously redirected to Gitpod in a new tab where current tab will show hyperexecute dashboard.--->

# How to run Selenium automation tests on HyperExecute (using PyUnit framework)

* [Pre-requisites](#pre-requisites)
   - [Download HyperExecute CLI](#download-hyperexecute-cli)
   - [Configure Environment Variables](#configure-environment-variables)

* [Auto-Split Execution with PyUnit](#auto-split-execution-with-pyunit)
   - [Core](#core)
   - [Pre Steps and Dependency Caching](#pre-steps-and-dependency-caching)
   - [Post Steps](#post-steps)
   - [Artifacts Management](#artifacts-management)
   - [Test Execution](#test-execution)

* [Matrix Execution with PyUnit](#matrix-execution-with-pyunit)
   - [Core](#core-1)
   - [Pre Steps and Dependency Caching](#pre-steps-and-dependency-caching-1)
   - [Post Steps](#post-steps-1)
   - [Artifacts Management](#artifacts-management-1)
   - [Test Execution](#test-execution-1)

* [Secrets Management](#secrets-management)
* [Navigation in Automation Dashboard](#navigation-in-automation-dashboard)

# Pre-requisites

Before using HyperExecute, you have to download HyperExecute CLI corresponding to the host OS. Along with it, you also need to export the environment variables *LT_USERNAME* and *LT_ACCESS_KEY* that are available in the [LambdaTest Profile](https://accounts.lambdatest.com/detail/profile) page.

## Download HyperExecute CLI

HyperExecute CLI is the CLI for interacting and running the tests on the HyperExecute Grid. The CLI provides a host of other useful features that accelerate test execution. In order to trigger tests using the CLI, you need to download the HyperExecute CLI binary corresponding to the platform (or OS) from where the tests are triggered:

Also, it is recommended to download the binary in the project's parent directory. Shown below is the location from where you can download the HyperExecute CLI binary:

* Mac: https://downloads.lambdatest.com/hyperexecute/darwin/hyperexecute
* Linux: https://downloads.lambdatest.com/hyperexecute/linux/hyperexecute
* Windows: https://downloads.lambdatest.com/hyperexecute/windows/hyperexecute.exe

## Configure Environment Variables

Before the tests are run, please set the environment variables LT_USERNAME & LT_ACCESS_KEY from the terminal. The account details are available on your [LambdaTest Profile](https://accounts.lambdatest.com/detail/profile) page.

For macOS:

```bash
export LT_USERNAME=LT_USERNAME
export LT_ACCESS_KEY=LT_ACCESS_KEY
```

For Linux:

```bash
export LT_USERNAME=LT_USERNAME
export LT_ACCESS_KEY=LT_ACCESS_KEY
```

For Windows:

```bash
set LT_USERNAME=LT_USERNAME
set LT_ACCESS_KEY=LT_ACCESS_KEY
```

## Auto-Split Execution with PyUnit

Auto-split execution mechanism lets you run tests at predefined concurrency and distribute the tests over the available infrastructure. Concurrency can be achieved at different levels - file, module, test suite, test, scenario, etc.

For more information about auto-split execution, check out the [Auto-Split Getting Started Guide](https://www.lambdatest.com/support/docs/getting-started-with-hyperexecute/#smart-auto-test-splitting)

### Core

Auto-split YAML file (*yaml/win/pyunit_hyperexecute_autosplit_sample.yaml*) in the repo contains the following configuration:

```yaml
globalTimeout: 90
testSuiteTimeout: 90
testSuiteStep: 90
```

Global timeout, testSuite timeout, and testSuite timeout are set to 90 minutes.
 
The *runson* key determines the platform (or operating system) on which the tests are executed. Here we have set the target OS as Windows.

```yaml
runson: win
```

Auto-split is set to true in the YAML file.

```yaml
 autosplit: true
```

*retryOnFailure* is set to true, instructing HyperExecute to retry failed command(s). The retry operation is carried out till the number of retries mentioned in *maxRetries* are exhausted or the command execution results in a *Pass*. In addition, the concurrency (i.e. number of parallel sessions) is set to 2.

```yaml
retryOnFailure: true
maxRetries: 5
concurrency: 2
```

## Pre Steps and Dependency Caching

To leverage the advantage offered by *Dependency Caching* in HyperExecute, the integrity of *requirements.txt* is checked using the checksum functionality.

```yaml
cacheKey: '{{ checksum "requirements.txt" }}'
```

By default, *pip* in Python saves the downloaded packages in the cache so that next time, the package download request can be serviced from the cache (rather than re-downloading it again).

The caching advantage offered by *pip* can be leveraged in HyperExecute, whereby the downloaded packages can be stored (or cached) in a secure server for future executions. The packages available in the cache will only be used if the checksum stage results in a Pass.

The *cacheDirectories* directive is used for specifying the directory where the packages have to be cached. The mentioned directory will override the default directory where Python packages are usually cached; further information about caching in pip is available [here](https://pip.pypa.io/en/stable/cli/pip_cache/). The packages downloaded using pip will be cached in the directory (or location) mentioned under the *cacheDirectories* directive.

In our case, the downloaded packages are cached in the *CacheDir* folder in the project's root directory. The folder is automatically created when the packages mentioned in *requirements.txt* are downloaded.  

```yaml
cacheDirectories:
  - CacheDir
```

Content under the *pre* directive is the precondition that will run before the tests are executed on the HyperExecute grid. The *--cache-dir* option in *pip3* is used for specifying the cache directory. It is important to note that downloaded cached packages are securely uploaded to a secure cloud before the execution environment is auto-purged after build completion. Please modify *requirements.txt* as per the project requirements.

```yaml
pip3 install -r requirements.txt  --cache-dir CacheDir
```

## Post Steps

The *post* directive contains a list of commands that run as a part of post-test execution. Here, the contents of *yaml/win/pyunit_hyperexecute_autosplit_sample.yaml* are read using the *cat* command as a part of the post step.

```yaml
post:
  - cat yaml/win/pyunit_hyperexecute_autosplit_sample.yaml
```

The *testDiscovery* directive contains the command that gives details of the mode of execution, along with detailing the command that is used for test execution. Here, we are fetching the list of Python files that would be further executed using the *value* passed in the *testRunnerCommand*

```yaml
testDiscovery:
  type: raw
  mode: dynamic
  command: grep -nri 'hyperexecutePyUnit' tests -ir --include=\*.py | sed 's/:.*//'

testRunnerCommand: python3 -s $test
```

Running the above command on the terminal will give a list of Python files that are located in the Project folder:

* tests/lt_selenium_playground.py
* tests/lt_sample_todo.py

The *testRunnerCommand* contains the command that is used for triggering the test. The output fetched from the *testDiscoverer* command acts as an input to the *testRunner* command.

```yaml
testRunnerCommand: python3 -s $test
```

### Artifacts Management

The *mergeArtifacts* directive (which is by default *false*) is set to *true* for merging the artifacts and combing artifacts generated under each task.

The *uploadArtefacts* directive informs HyperExecute to upload artifacts [files, reports, etc.] generated after task completion.  In the example, *path* consists of a regex for parsing the directory (i.e. *example_1* and *example_2* that contains the test reports).

```yaml
mergeArtifacts: true

uploadArtefacts:
  - name: TestReport1
    path:
     - example_1/**
  - name: TestReport2
    path:
     - example_2/**
```
HyperExecute also facilitates the provision to download the artifacts on your local machine. To download the artifacts, click on *Artifacts* button corresponding to the associated TestID.

<img width="1235" alt="pyunit_autosplit_artefacts_1" src="https://user-images.githubusercontent.com/1688653/160467003-d7e671a7-cc14-4941-b7fa-40bbe1fc8502.png">

Now, you can download the artifacts by clicking on the *Download* button as shown below:

<img width="1420" alt="pyunit_autosplit_artefacts_2" src="https://user-images.githubusercontent.com/1688653/160467011-787d5de6-f45c-4125-b969-4e845688d9f2.png">

### Test Execution

The CLI option *--config* is used for providing the custom HyperExecute YAML file (i.e. *yaml/win/pyunit_hyperexecute_autosplit_sample.yaml* for Windows and *yaml/linux/pyunit_hyperexecute_autosplit_sample.yaml* for Linux).

#### Execute PyUnit tests using Autosplit mechanism on Windows platform

Run the following command on the terminal to trigger the tests in Python files with HyperExecute platform set to Windows. The *--download-artifacts* option is used to inform HyperExecute to download the artifacts for the job.

```bash
./hyperexecute --download-artifacts --verbose --config yaml/win/pyunit_hyperexecute_autosplit_sample.yaml
```

#### Execute PyUnit tests using Autosplit mechanism on Linux platform

Run the following command on the terminal to trigger the tests in Python files with HyperExecute platform set to Linux. The *--download-artifacts* option is used to inform HyperExecute to download the artifacts for the job.

```bash
./hyperexecute --download-artifacts --verbose --config yaml/linux/pyunit_hyperexecute_autosplit_sample.yaml
```

Visit [HyperExecute Automation Dashboard](https://automation.lambdatest.com/hyperexecute) to check the status of execution

<img width="1426" alt="pyunit_autosplit_execution_latest" src="https://user-images.githubusercontent.com/1688653/160467003-d7e671a7-cc14-4941-b7fa-40bbe1fc8502.png">

Shown below is the execution screenshot when the YAML file is triggered from the terminal:

<img width="1421" alt="pyunit_autosplit_execution_1" src="https://user-images.githubusercontent.com/1688653/162371076-e3ed10de-2425-4a88-8ba5-cbf4dbbe67a4.png">

<img width="1408" alt="pyunit_autosplit_cli2_execution" src="https://user-images.githubusercontent.com/1688653/162371098-8129ed25-a585-4b09-84ba-7c23ffb1ada0.png">

# Matrix Execution with PyUnit

Matrix-based test execution is used for running the same tests across different test (or input) combinations. The Matrix directive in HyperExecute YAML file is a *key:value* pair where value is an array of strings.

Also, the *key:value* pairs are opaque strings for HyperExecute. For more information about matrix multiplexing, check out the [Matrix Getting Started Guide](https://www.lambdatest.com/support/docs/getting-started-with-hyperexecute/#matrix-based-build-multiplexing)

### Core

In the current example, matrix YAML file (*yaml/win/pyunit_hyperexecute_matrix_sample.yaml*) in the repo contains the following configuration:

```yaml
globalTimeout: 90
testSuiteTimeout: 90
testSuiteStep: 90
```

Global timeout, testSuite timeout, and testSuite timeout are set to 90 minutes.
 
The target platform is set to Windows. Please set the *[runson]* key to *[mac]* if the tests have to be executed on the macOS platform.

```yaml
runson: win
```

Python files in the 'tests' folder contain the test suites run on the HyperExecute grid. In the example, the tests in the files *tests/lt_sample_todo.py* and *tests/lt_selenium_playground.py* run in parallel using the specified input combinations.

```yaml
files: ["tests/lt_sample_todo.py", "tests/lt_selenium_playground.py"]
```

The *testSuites* object contains a list of commands (that can be presented in an array). In the current YAML file, commands for executing the tests are put in an array (with a '-' preceding each item). The Python command is used to run tests in *.py* files. The files are mentioned as an array to the *files* key that is a part of the matrix.

```yaml
testSuites:
  - python3 -s  $files
```

### Pre Steps and Dependency Caching

Dependency caching is enabled in the YAML file to ensure that the package dependencies are not downloaded in subsequent runs. The first step is to set the Key used to cache directories.

```yaml
cacheKey: '{{ checksum "requirements.txt" }}'
```

Set the array of files & directories to be cached. In the example, all the packages will be cached in the *CacheDir* directory.

```yaml
cacheDirectories:
  - CacheDir
```

Steps (or commands) that must run before the test execution are listed in the *pre* run step. In the example, the packages listed in *requirements.txt* are installed using the *pip3* command.

The *--cache-dir* option is used for specifying the location of the directory used for caching the packages (i.e. *CacheDir*). It is important to note that downloaded cached packages are securely uploaded to a secure cloud before the execution environment is auto-purged after build completion. Please modify *requirements.txt* as per the project requirements.

```yaml
pre:
  - pip3 install -r requirements.txt --cache-dir CacheDir
```

### Post Steps

Steps (or commands) that need to run after the test execution are listed in the *post* step. In the example, we *cat* the contents of *yaml/win/pyunit_hyperexecute_matrix_sample.yaml*

```yaml
post:
  - cat yaml/win/pyunit_hyperexecute_matrix_sample.yaml
```

### Artifacts Management

The *mergeArtifacts* directive (which is by default *false*) is set to *true* for merging the artifacts and combing artifacts generated under each task.

The *uploadArtefacts* directive informs HyperExecute to upload artifacts [files, reports, etc.] generated after task completion. In the example, *path* consists of a regex for parsing the directory (i.e. *example_1* and *example_2* that contains the test reports).

```yaml
mergeArtifacts: true

uploadArtefacts:
  - name: TestReport1
    path:
     - example_1/**
  - name: TestReport2
    path:
     - example_2/**
```

HyperExecute also facilitates the provision to download the artifacts on your local machine. To download the artifacts, click on Artifacts button corresponding to the associated TestID.

<img width="1425" alt="pyunit_matrix_artefacts_1" src="https://user-images.githubusercontent.com/1688653/162371113-8aab18de-33b9-42c3-838f-2db0505bbd07.png">

Now, you can download the artifacts by clicking on the Download button as shown below:

<img width="1425" alt="pyunit_matrix_artefacts_2" src="https://user-images.githubusercontent.com/1688653/162371124-a751efaf-da80-4f9c-bdc8-d98fc8b2f61e.png">

## Test Execution

The CLI option *--config* is used for providing the custom HyperExecute YAML file (i.e. *yaml/win/pyunit_hyperexecute_matrix_sample.yaml* for Windows and *yaml/win/pyunit_hyperexecute_matrix_sample.yaml* for Linux).

#### Execute PyUnit tests using Matrix mechanism on Windows platform

Run the following command on the terminal to trigger the tests in Python files with HyperExecute platform set to Windows. The *--download-artifacts* option is used to inform HyperExecute to download the artifacts for the job.

```bash
./hyperexecute --download-artifacts --config --verbose yaml/win/pyunit_hyperexecute_matrix_sample.yaml
```

#### Execute PyUnit tests using Matrix mechanism on Linux platform

Run the following command on the terminal to trigger the tests in Python files with HyperExecute platform set to Linux. The *--download-artifacts* option is used to inform HyperExecute to download the artifacts for the job.

```bash
./hyperexecute --download-artifacts --config --verbose yaml/linux/pyunit_hyperexecute_matrix_sample.yaml
```

Visit [HyperExecute Automation Dashboard](https://automation.lambdatest.com/hyperexecute) to check the status of execution:

<img width="1430" alt="pyunit_matrix_execution_latest" src="https://user-images.githubusercontent.com/1688653/162371113-8aab18de-33b9-42c3-838f-2db0505bbd07.png">

Shown below is the execution screenshot when the YAML file is triggered from the terminal:

<img width="1413" alt="pyunit_cli_execution" src="https://user-images.githubusercontent.com/1688653/162371132-1ba32fa8-c57a-4add-8039-f860d32dec2a.png">

<img width="1101" alt="pyunit_cli2_execution" src="https://user-images.githubusercontent.com/1688653/162371142-35c8f214-1bcd-4b57-984d-b70d43647f20.png">

## Secrets Management

In case you want to use any secret keys in the YAML file, the same can be set by clicking on the *Secrets* button the dashboard.

<img width="703" alt="pyunit_secrets_key_1" src="https://user-images.githubusercontent.com/1688653/152540968-90e4e8bc-3eb4-4259-856b-5e513cbd19b5.png">

Now create a *secret* key that you can use in the HyperExecute YAML file.

<img width="359" alt="secrets_management_1" src="https://user-images.githubusercontent.com/1688653/153250877-e58445d1-2735-409a-970d-14253991c69e.png">

All you need to do is create an environment variable that uses the secret key:

```yaml
env:
  PAT: ${{ .secrets.testKey }}
```

## Navigation in Automation Dashboard

HyperExecute lets you navigate from/to *Test Logs* in Automation Dashboard from/to *HyperExecute Logs*. You also get relevant get relevant Selenium test details like video, network log, commands, Exceptions & more in the Dashboard. Effortlessly navigate from the automation dashboard to HyperExecute logs (and vice-versa) to get more details of the test execution.

Shown below is the HyperExecute Automation dashboard which also lists the tests that were executed as a part of the test suite:

<img width="1238" alt="pyunit_hyperexecute_automation_dashboard" src="https://user-images.githubusercontent.com/1688653/162371032-b24db722-ee2f-4a83-a3e2-80a69c2846ec.png">

Here is a screenshot that lists the automation test that was executed on the HyperExecute grid:

<img width="1425" alt="pyunit_testing_automation_dashboard" src="https://user-images.githubusercontent.com/1688653/162371043-cb2a143d-2b17-46f6-8734-9b2b76b202d1.png">

## LambdaTest Community :busts_in_silhouette:
***
The [LambdaTest Community](https://community.lambdatest.com/) allows people to interact with tech enthusiasts. Connect, ask questions, and learn from tech-savvy people. Discuss best practises in web development, testing, and DevOps with professionals from across the globe.

## Documentation & Resources :books:
***
      
If you want to learn more about the LambdaTest's features, setup, and usage, visit the [LambdaTest documentation](https://www.lambdatest.com/support/docs/). You can also find in-depth tutorials around test automation, mobile app testing, responsive testing, manual testing on [LambdaTest Blog](https://www.lambdatest.com/blog/) and [LambdaTest Learning Hub](https://www.lambdatest.com/learning-hub/).     
      
 ## About LambdaTest
***
[LambdaTest](https://www.lambdatest.com) is a leading test execution and orchestration platform that is fast, reliable, scalable, and secure. It allows users to run both manual and automated testing of web and mobile apps across 3000+ different browsers, operating systems, and real device combinations. Using LambdaTest, businesses can ensure quicker developer feedback and hence achieve faster go to market. Over 500 enterprises and 1 Million + users across 130+ countries rely on LambdaTest for their testing needs.

[<img height="70" src="https://user-images.githubusercontent.com/70570645/169649126-ed61f6de-49b5-4593-80cf-3391ca40d665.PNG">](https://accounts.lambdatest.com/register)
      
## We are here to help you :headphones:
***
* Got a query? we are available 24x7 to help. [Contact Us](mailto:support@lambdatest.com)
* For more info, visit - https://www.lambdatest.com

## License
Licensed under the [MIT license](LICENSE).
