# Bash Jira

A simple utility script to automate handling tickets from branch names

## Setup

1. Clone this repository or simply copy the jira file out of the bin directory and place it somewhere in your path.

    For a list of locations on the path run

    ```bash
    echo $PATH
    ```

    from the terminal

2. Make sure the file is executable

    If you clone the repository and copy the file this should already be the case otherwise make it executable by running

    ```bash
    chmod u+x ~/path/to/bin/jira
    ```

3. Set the `$jira_domain` variable to the base url of your jira file

    This should look something like this

    ```shell
    jira_domain="https://yourdomain.atlassian.net"
    ```

4. To verify that everything is set up run

    ```shell
    jira dashboard
    ```

    which should take you to your default dashboard

### To set up boards

1. Navigate to your project's board

2. Grab the query params `?projectKey=KEY&rapidView=123`

3. In the jira script navigate to the `handle_boards` function

4. Copy the switch block in the `handle_boards` function

    ```shell
    "example"*)
        jira_open "${jira_domain}${board_url}?projectKey=KEY&rapidView=886"
        ;;
    ```

5. Paste it on a newline after the ending `;;`, Update the block with your query parms and you'll end up with something like

    ```shell
    "example"*)
        jira_open "${jira_domain}${board_url}?projectKey=KEY&rapidView=886"
        ;;
    "my_board"*)
        jira_open "${jira_domain}${board_url}?projectKey=BOA&rapidView=1337"
        ;;
    ```

6. You should now be able to open your board by doing

    ```shell
    jira board my_board
    ```

7. Update the `list` case to include your newly configured board so you can reference `jira board list` for your configured boards

    ```shell
    "list"*)
        echo "
        Usage:
            jira board example          - takes you to the board you configure
            jira board my_board         - my project's board
        "
        ;;
    ```

## Example Workflow

1. Open your jira board

    if you have it set up run 

    ```shell
    jira board [your keyword]
    ```

    in my case i use (as part of early engagement squad)

    ```shell
    jira board early
    ```

2. Copy the the ticket name and description together

    This ends up as something like

    `PROJKEY-11111 Onboarding: Welcome Page`

    run a script to kebabcase it

    ```shell
    jira ticket kebab
    ```
    this overwrites your clipboard to be

    `PROJKEY-11111/onboarding-welcome-page`

3. Create your branch in the repository using your clipboard

4. when your branch name has a ticket number in it run `jira` and it will take you to the ticket in the branch

    ```shell
    jira
    ```