# Repository Tooling

## Local Quick-start

For local usage or development.

Pre-requisites:

- Docker and Docker Compose
- git
- Python 3.8+
- Invoke

Clone the repository:

```shell
git clone git@github.com:nautobot/cookiecutter-nautobot-app.git
cd cookiecutter-nautobot-app
```

Run all tests:

```shell
invoke tests
```

List all available tasks:

```shell
invoke help
```

Bake a cookie:

```shell
invoke bake --template=nautobot-app
```

## Usage with Docker and Invoke

To use templates locally with Docker and Invoke, you need first set up the repository as explained in [local quick-start](#local-quick-start).

Then, you can use the following command:

```shell
invoke bake
```

Output:

```shell
docker compose run --rm -- dev cookiecutter --output-dir=./outputs   ./nautobot-app
  [1/18] codeowner_github_usernames (@smith-ntc):
  [2/18] full_name (Network to Code, LLC):
  [3/18] email (info@networktocode.com):
  [4/18] github_org (nautobot):
  [5/18] plugin_name (my_plugin):
  [6/18] verbose_name (My Plugin):
  [7/18] plugin_slug (my-plugin):
  [8/18] project_slug (nautobot-plugin-my-plugin):
  [9/18] repo_url (https://github.com/nautobot/nautobot-plugin-my-plugin):
  [10/18] base_url (my-plugin):
  [11/18] min_nautobot_version (1.6.0):
  [12/18] max_nautobot_version (1.9999):
  [13/18] camel_name (MyPlugin):
  [14/18] project_short_description (My Plugin):
  [15/18] model_class_name (None):
  [16/18] Select open_source_license
    1 - Apache-2.0
    2 - Not open source
    Choose from [1/2] (1):
  [17/18] docs_base_url (https://docs.nautobot.com):
  [18/18] docs_app_url (https://docs.nautobot.com/projects/my-plugin/en/latest):

Congratulations! Your cookie has now been baked. It is located at /opt/ntc/nautobot/cookiecutter-nautobot-app/outputs/nautobot-plugin-my-plugin.

⚠️⚠️ Before you start using your cookie you must run the following commands inside your cookie:

* poetry lock
* cp development/creds.example.env development/creds.env
* poetry shell
* invoke makemigrations

The file `creds.env` will be ignored by git and can be used to override default environment variables.
```

This command, bakes a new cookie using the `nautobot-app` template into `outputs` directory.

### Help

To see all arguments run:

```shell
invoke bake --help
```

Output:

```shell
Usage: inv[oke] [--core-opts] bake [--options] [other tasks here ...]

Docstring:
  Bake a new cookie from the template.

Options:
  -d, --debug                      Whether to run in debug mode (defaults to False)
  -i, --[no-]input                 Whether to require user input, ignored with `--json-file` (defaults to True)
  -j STRING, --json-file=STRING    Path to a JSON file containing answers to prompts (defaults to empty)
  -o STRING, --output-dir=STRING   Path to the output directory (defaults to ./outputs)
  -t STRING, --template=STRING     Path to the cookiecutter template to bake (defaults to ./nautobot-app)
```

!!! IMPORTANT !!! The `--output-dir` argument should be located in the current directory, as the Cookiecutter creates the cookie inside the Docker container with the current directory bind mounted. When baking somewhere else, the resulting cookie would not be available from calling host system.

### JSON file

You can reuse existing JSON file with pre-filled template prompts. To do so, you need to pass the path to the JSON file using the `--json-file` argument.

When running the command with `--json-file` argument, the command will not prompt for any input.

First create the `my-app.json` file:

```json
{
    "cookiecutter": {
        "codeowner_github_usernames": "@smith-ntc",
        "full_name": "Network to Code, LLC",
        "email": "info@networktocode.com",
        "github_org": "nautobot",
        "plugin_name": "my_app",
        "verbose_name": "My App",
        "plugin_slug": "my-app",
        "project_slug": "nautobot-plugin-my-app",
        "repo_url": "https://github.com/nautobot/nautobot-plugin-my-app",
        "base_url": "my-app",
        "min_nautobot_version": "2.0.0",
        "max_nautobot_version": "2.9999",
        "camel_name": "MyApp",
        "project_short_description": "My App",
        "model_class_name": "MyModel",
        "open_source_license": "Apache-2.0",
        "docs_base_url": "https://docs.nautobot.com",
        "docs_app_url": "https://docs.nautobot.com/projects/my-app/en/latest"
    }
}
```

Then run the following command:

```shell
invoke bake --json-file=my-app.json
```

Output:

```shell
docker compose run --rm -- dev cookiecutter --output-dir=./outputs  --replay-file=my-app.json ./nautobot-app

Congratulations! Your cookie has now been baked. It is located at /opt/ntc/nautobot/cookiecutter-nautobot-app/outputs/nautobot-plugin-my-app.

⚠️⚠️ Before you start using your cookie you must run the following commands inside your cookie:

* poetry lock
* cp development/creds.example.env development/creds.env
* poetry shell
* invoke makemigrations

The file `creds.env` will be ignored by git and can be used to override default environment variables.
```