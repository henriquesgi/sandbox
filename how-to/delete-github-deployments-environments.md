# How to Delete GitHub Deployments Environments

## **Requirements**
**Dificulty**: Easy \
**What you'll need**: 
- Personal access token

## **Step-by-step**
- **Generate your personal access token**:

The purpose of this token is to give you access to the [GitHub REST API](https://docs.github.com/en/rest).

Go to GitHub > Settings > Developer settings > Personal access tokens > Tokens (classic) > Generate new token

- **Listing the IDs**:

```sh
curl -L \
  -H "Accept: application/vnd.github+json" \
  -H "Authorization: Bearer <YOUR-TOKEN>" \
  -H "X-GitHub-Api-Version: 2022-11-28" \
  "https://api.github.com/repos/OWNER/REPO/deployments"
```

You should consider the following:
> If the repository only has one deployment, you can delete the deployment regardless of its status. If the repository has more than one deployment, you can only delete inactive deployments. This ensures that repositories with multiple deployments will always have an active deployment. Anyone with repo or repo_deployment scopes can delete a deployment.
>
> To set a deployment as inactive, you must:
>
> -  Create a new deployment that is active so that the system has a record of the current state, then delete the previously active deployment.
>  -  Mark the active deployment as inactive by adding any non-successful deployment status.

Check out more details here [Delete a deployment](https://docs.github.com/en/rest/deployments/deployments#delete-a-deployment).

- **Changing status**:

```sh
#!/bin/bash
for id in ids
do
 curl -L \
  -X POST \
  -H "Accept: application/vnd.github+json" \
  -H "Authorization: Bearer <YOUR-TOKEN>" \
  -H "X-GitHub-Api-Version: 2022-11-28" \
  "https://api.github.com/repos/OWNER/REPO/deployments/$id/statuses" \
  -d '{ "state": "inactive" }'
done
```

Just copy the curl instruction if you have only one or a few deployments.

- **Deleting**

```sh
#!/bin/bash
for id in ids
do
 curl -L \
  -X DELETE \
  -H "Accept: application/vnd.github+json" \
  -H "Authorization: Bearer <YOUR-TOKEN>" \
  -H "X-GitHub-Api-Version: 2022-11-28" \
  "https://api.github.com/repos/OWNER/REPO/deployments/$id"
done
```

It's done.
