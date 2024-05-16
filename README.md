# Repository Cleaning

Scan! Clean! Scan!

1. Clone the Repository: `git clone --mirror {URL} ./repositories/`
	- This is a bare repo, which means your normal files won't be visible, but it is a full copy of the Git database of your repository.
2. Create the following files/folders:
	- `./expressions.txt`
		- This is created, but **MUST NOT** be committed.
	- `./gitleaks.csv`
		- This is created, but **MUST NOT** be committed.
	- `./bfg-reports/`

## Scan (1)

1. Run the scan: `docker compose --file scan.docker-compose.yaml up -d --force-recreate`
	- This can take some time! Use the following to watch the output: `docker logs gitleaks-scan-gitleaks-1 --follow`
2. Look at the `giteaks.csv` to view the results!
	- **NOTE**: The scan outputs CONTAIN the secrets.

## Clean (2)

1. Create a backup! Maybe even multiple!
	- This can be as simple as doing another clone to another directory (`repository_test`).
2. Use the, [scan created](#scan-1), gitleaks.csv. Use the `Secret` column and paste secrets you want changes within [expressions.txt](./expressions.txt).
	- Do **NOT** include the quotation marks.
	- Ensure it is actually a secret. Some false positives are found/added to the secret.
	- Each secret should be on a new line.
	- Add a single empty new line to the end.
3. Run the clean command: `docker compose --file scan.docker-compose.yaml up -d --force-recreate`
	- This will NOT change protected commits. This includes the latest commit.
		- You can clean the lastest commit by:
			1. Doing a full clone.
			2. Updating the content to no longer need the file/content within it.
			3. Commit the change without the content/file present.
				- A file removal may require a [`.gitignore`](https://git-scm.com/docs/gitignore) update to the repository prevent it from re-appearing in the repository.
	- Use the following to watch the output: `docker logs bfg-repo-cleaner-bfg-1 --follow`
4. The [bfg-reports](./bfg-reports/) folder (Larger number, more recent report) can be used to view what occured in the cleaning.

## Scan (3)

Perform [step 1](#scan-1) again to ensure the changes stuck.

## Scary Big Changes


The next step is to update the remote repository, and this means overwriting the remote version.




## Shoutouts

- [BFG Repo Cleaner](https://github.com/rtyley/bfg-repo-cleaner) - Used for the Git repository cleaning.
- [Gitleaks](https://github.com/gitleaks/gitleaks) - Used to scan and detect secrets within a Git repository history.
- [tagplus5](https://github.com/tagplus5/docker-git-bfg) for the Docker BFG image on [Dockerhub](https://hub.docker.com/r/tagplus5/git-bfg).
- [zricethezav](https://github.com/gitleaks/gitleaks) for the Docker Gitleaks image on [Dockerhub](https://hub.docker.com/r/zricethezav/gitleaks).