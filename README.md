# Nektos Act Guide
Tool link: https://github.com/nektos/act

* Q: Why would I, a developer use this tool?
  A: Iterating on a pipline is a time consuming process, and can result in lots of messy commits when you're trying to get feedback on changes. Act shortens your feedback time drastically and doesn't require to push commits to your project repo by lettinggg you run workflows locally

## Quickstart
* install (assuming linux/in your WSL2.0 term) `curl https://raw.githubusercontent.com/nektos/act/master/install.sh | sudo bash`

* Set up a `.secrets` file in your working directory. I placed mine in `.github/`

* Add your repo secrets to this file and `DO NOT TRACK IT` - it is purely for local dev

* Invoke act and specify your job:
`act -P ubuntu-20.04=nektos/act-environments-ubuntu:18.04  --secret-file=.github/act.secrets -j <job.name from any workflow file>`

### Caveats/Gotchas
* If job keys match, it can be confusing to specify which to run, just provide a distinct name for each job and use that value

* The `-P` effectively causes the runner image specified in your workflow jobs `runs-on` key to be overridden. Using `ubuntu-20.04=nektos/act-environments-ubuntu:18.04` is important because the default slim image used by act is likely insufficient for your workflows. The nektos 18.04 is the largest image on offer but obviously this brings environment parity into question. Make sure you check these and override appropriately.

## Extras
* dry run mode with the `-n` flag

* Specify an event (eg: pull_request) instead of a job
`act pull_request -P ubuntu-20.04=nektos/act-environments-ubuntu:18.04  --secret-file=.github/act.secrets`

* specify the default branch, which is handy for when your new workflows depend on default-branch-only events (commonly develop)
`--defaultbranch=feature-to-go-into-develop`

* Want a default runner override? You can set it up like so in `.github/`
`echo '-P ubuntu-20.04=nektos/act-environments-ubuntu:18.04' >> .actrc`

## Known Issues
If runners are interrupted (forced) you can end up with zombie containers that cannot be easily killed. Requires restart? Still looking into this.