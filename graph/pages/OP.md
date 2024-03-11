- to install new fork on Comma3:
	- `installer.comma.ai/<username>/<branch>`
	- `installer.comma.ai/twilsonco/tw-staging`
		- When switching OFF twilsonco fork you must first install stock OP - DASHCAM MODE, then reset again, and only then install frog or other
	- `installer.comma.ai/twilsonco/frog-dev`
	- `installer.comma.ai/twilsonco/sunny-dev`
	- `installer.comma.ai/FrogAi/FrogPilot-Staging`
		- `staging.frogpilot.download`
	- `installer.comma.ai/opgm/staging`
	- `Sunnyhaibin/release-c3`
	-
-
- Update over SSH: `git pull && ./rr.sh`
- Switching forks: https://discord.com/channels/469524606043160576/884811574773157949/1186345080924156058
	- Yes and no @steubens ['17 Volt ACC]. You must flash AGNOS to switch back and forth, but if you do it over SSH by switching branches then it doesn't wipe out existing params, so settings will be retained between forks.
	- For example:
	- Start with clone of tw:
		- `git clone -b tw-staging https://www.github.com/twilsonco/openpilot`
		- This adds my fork as the main "origin" repository, and sets it up to only fetch the `tw-staging` branch.
	- Then add Frog and Sunny as remotes. For each, we'll change the configuration so that it only fetches the branch we're interested in (`dev-c3` for Sunny, and `FrogPilot` for Frog), otherwise it will fetch *all* branches for the new remotes, taking up a bunch of space on the comma device
		- Sunny:
			- `git remote add sunny https://github.com/sunnyhaibin/sunnypilot`
			  `git config --unset-all remote.sunny.fetch`
			  `git config --add remote.sunny.fetch '+refs/heads/staging-c3:refs/remotes/sunny/staging-c3'`
			  `git config --add remote.sunny.fetch '+refs/heads/dev-c3:refs/remotes/sunny/dev-c3'`
		- Frog:
			- `git remote add frog https://github.com/frogai/frogpilot`
			  `git config --unset-all remote.frog.fetch`
			  `git config --add remote.frog.fetch '+refs/heads/FrogPilot:refs/remotes/frog/FrogPilot'`
			  `git config --add remote.frog.fetch '+refs/heads/FrogPilot-Staging:refs/remotes/frog/FrogPilot-Staging'`
			  `git config --add remote.frog.fetch '+refs/heads/FrogPilot-Development:refs/remotes/frog/FrogPilot-Development'`
	- Now you can fetch and it will download the specific branch of each remote
		- `git fetch --all`
	- Then you can use checkout the remote branches. The first time you run checkout we'll specify to create the branches locally otherwise they'll be "detached heads" which is messy
		- `git checkout -b dev-c3 sunny/dev-c3`
		  `git checkout -b staging-c3 sunny/staging-c3`
		  `git checkout -b FrogPilot-Development frog/FrogPilot-Development`
		  `git checkout -b FrogPilot-Staging frog/FrogPilot-Staging`
		  `git checkout -b FrogPilot frog/FrogPilot`
	- **Now everything's setup.** At this point you can switch between using a simpler checkout command
		- `git checkout tw-staging`
		  `git checkout dev-c3` <-- sunny
		  `git checkout staging-c3` <-- sunny
		  `git checkout FrogPilot-Development`
		  `git checkout FrogPilot-Staging`
		  `git checkout FrogPilot`
	- pull latest and reboot:
		- `git pull && ./rr.sh`
	- However, again, **when switching between branches running different AGNOS versions (i.e. between `tw-staging` and either of the other two) you'll initiate an AGNOS reflash upon reboot, requiring a strong wifi connection.**